<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# osTicket-prereqs
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>

A full video walkthrough will be added here soon!  
> 🔗 [Watch the Video Tutorial (Coming Soon)](#)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)
- Windows 10 (running on Azure VM)
- HeidiSQL
- MySQL 5.5.62
- PHP 7.3.8


<h2>List of Prerequisites</h2>

- [OsTicket Download](https://drive.google.com/uc?export=download&id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD)
- [Microsoft Azure Account](https://portal.azure.com/)
- Remote Desktop Client (RDP)

<h2>Installation Steps</h2>

### **Step 1 - Create an Azure Account**


To follow along, you’ll need an Azure account. If you don’t already have one:

Go to https://portal.azure.com

<img src="https://i.imgur.com/iCdPlm0.png" alt="OSTicket Login" width="600"/>

Sign up or log in

Azure offers a free tier with credits for new users

Once you’re logged into the Azure Portal, you're ready to begin the Lab.


---



### **Step 2: Explore the Azure Portal**

Once you log into the Azure Portal, you’ll land on the dashboard where you can manage all your cloud resources.



<img src="./screenshots/azure-dashboard.png" alt="Azure Portal Overview" width="700"/>

From here, you can:

✅ Create new resources (like virtual machines or storage)

📁 Organize your environment using Resource Groups

🧠 Try out Azure AI, App Services, Kubernetes, and more

💬 Access tutorials and support through the Quickstart Center

📝 Note: You’ll see a message about your remaining free credit and options to upgrade when you’re ready.

---

### **Step 3: Create a Virtual Machine in Azure**

We’ll now set up the virtual machine that will host our osTicket system using the Azure portal.



#### 🔹 1. Start VM Creation

From the Azure portal dashboard, click:

**Create a resource → Compute → Virtual Machine**



#### 🔹 2. Fill Out the Basics Tab

On the **Basics** tab, enter your core VM details.

<img src="https://i.imgur.com/K1nOprr.png" alt="Azure VM Review and Create Tab" width="700"/>
<img src="./screenshots/vmcreate2.png" alt="Azure VM Review and Create Tab" width="700"/>
**Example values:**

- **Resource group:** `osticket-Lab`  
- **Virtual machine name:** `osticket-vm`  
- **Region:** `East US` (or your closest region)  
- **Image:** `Windows 10 Pro`  
- **Size:** `Standard_E2s_v3`  
- **Authentication type:** Password  
- **Username/Password:** Create secure login credentials  
- **Inbound ports:** Allow **RDP (3389)**

‼️Make sure you check the box under **Licensing**‼️
> ⚠️ *Save your username and password — you’ll need them to access the VM later.*

<br>

#### 🔹 3. Leave Default Settings for Remaining Tabs

You can leave all the following tabs at their default settings:

- **Disks**
- **Networking**
- **Management**
- **Advanced**
- **Tags**

No additional configuration is needed for this tutorial.



#### 🔹 4. Review and Create the VM

Once you complete the Basics tab and skip the rest, you'll land on the **Review + Create** page.

<img src="./screenshots/reviewcreate.png" alt="Azure VM Basics Tab" width="700"/>

If everything looks correct, click **Create** to deploy the virtual machine.

> 🛠️ *Deployment may take a couple of minutes. Once it’s complete, you’ll see a confirmation and a link to view your resource.*

---

### **Step 4: Connect to the Virtual Machine Using RDP**

Now that your VM is deployed, you can connect to it using Remote Desktop Protocol (RDP).



#### 🔹 1. Get the Public IP Address

After deployment, you’ll be taken to the **VM Overview page**. Here, you’ll see your VM’s **Public IP address** listed on the right-hand side.

Copy this IP address — you’ll use it to connect to the VM.

> 💡 *You can also find this later by going to: Azure Portal → Virtual Machines → Select your VM → Overview tab.*



#### 🔹 2. Open Remote Desktop on Your Computer

On your local machine:

- Open the **Remote Desktop Connection** app (on Windows, search for "RDP" or "Remote Desktop")
- Paste the **public IP address** into the **Computer** field
- Click **Connect**

<img src="./screenshots/rdp ip address.png" alt="Azure VM Basics Tab" width="700"/>


#### 🔹 3. Log Into the VM

When prompted:

- Enter the **username and password** you created during the VM setup
- You may see a warning about certificate trust — click **Yes** to proceed

You’ll now be connected to your virtual machine’s desktop environment.

> ✅ *From here, we will begin installing software, setting up your ticketing system, or making system changes as needed.*



---

### **Step 5: Download and Prepare the osTicket Installation Files**

Inside your VM, download the osTicket installation package and extract it to the desktop.



#### 🔹 1. Download the ZIP

Click the link below to download the installation files:

👉 [osTicket Download](https://drive.google.com/uc?export=download&id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD)

- When prompted, click **Download**
- If your browser flags the file, choose **Download Anyway**

<img src="./screenshots/googledrivedownload.png" alt="Azure VM Basics Tab" width="500"/>
  

> 💡 This ZIP file includes osTicket and some required dependencies already packaged.



#### 🔹 2. Unzip to the Desktop

- Right-click the downloaded file → **Extract All**
- Choose **Desktop** as the destination
- You should now see a folder named: osTicket-Installation-Files

- 
Located here:
C:\Users<YourUser>\Desktop\osTicket-Installation-Files


> ✅ We’ll use the files in this folder to install osTicket and its required components in the upcoming steps.


---

### **Step 6: Install and Enable IIS with CGI Support**

To run osTicket on Windows, we need to install **IIS (Internet Information Services)** and enable **CGI** to allow PHP to function properly.

This is done through Windows Features in the Control Panel.



#### 🔹 1. Open Windows Features

Inside your VM:

- Open the **Control Panel**
- Go to **Programs**
- Click **“Turn Windows features on or off”**

> 💡 You can also search “Windows Features” in the Start menu to access it directly.



#### 🔹 2. Enable IIS and CGI

In the **Windows Features** window:

- Scroll down and check **Internet Information Services**
- Expand it by clicking the `+` symbol
- Then expand **World Wide Web Services**
- Expand **Application Development Features**
- Check the box for **CGI**


<img src="https://i.imgur.com/Vt5lolM.png" alt="Enable IIS and CGI in Windows Features" width="700"/>

Click **OK** to install IIS and CGI. Windows will take a few minutes to apply the changes.

---

### **Step 7: Install Required IIS Extensions and Set Up PHP Directory**

Now that IIS and CGI are enabled, it’s time to install a few important components from the `osTicket-Installation-Files` folder to make IIS compatible with PHP and URL rewriting — both necessary for osTicket to run.



#### 🔹 1. Open the Installation Folder

Navigate to the Desktop and open the folder named: `osTicket-Installation-Files`


<img src="https://i.imgur.com/5aKvqIU.png" alt="osTicket Installation Files Folder" width="700"/>



#### 🔹 2. Install PHP Manager for IIS

- Double-click **`PHPManagerForIIS_V1.5.0.msi`**
- Go through the installer and complete the setup

> 🛠️ This tool allows you to easily manage and configure PHP settings within IIS.



#### 🔹 3. Install URL Rewrite Module

- Double-click **`rewrite_amd64_en-US.msi`**
- Follow the prompts to complete the installation

> 🔁 This module is required by osTicket to support user-friendly URLs and redirects.



#### 🔹 4. Create the PHP Directory

Now create a directory that will hold your PHP files:

1. Open **File Explorer**
2. Navigate to the **C:\** drive
3. Right-click inside the window → **New > Folder**
4. Name the folder: `PHP`


> 📁 You should now have an empty folder at `C:\PHP` ready for the next step, where we’ll extract and configure PHP.

   

#### 🔹 5. Extract PHP, Install VC++ Runtime, and MySQL

Next, extract and install the necessary components for osTicket to run properly.



##### ✅ Extract PHP

- Inside the installation folder, find:`php-7.3.8-nts-Win32-VC15-x86.zip`

- Right-click → **Extract All**
- Set the destination to: `C:\PHP`

<img src="https://i.imgur.com/1Ifha26.png" alt="Extracted PHP Folder in C Drive" width="700"/>

  
> ✅ When done, your `C:\PHP` folder should now contain the PHP files needed for IIS.



##### ✅ Install Visual C++ Redistributable

- Run **`VC_redist.x86.exe`**
- Accept the license terms and complete the setup

> 🧱 Required for PHP 7.3.8 to function correctly on Windows



##### ✅ Install MySQL 5.5.62

- Run **`mysql-5.5.62-win32.msi`**
- Choose **Typical Setup**
- After install, launch the **MySQL Instance Configuration Wizard**

In the wizard:

- Choose **Standard Configuration**
- Set login credentials to:
Username: `root`
Password: `root`


> 🔐 Remember these credentials — they’ll be used to connect osTicket to the database later.


At this point, your VM is ready with PHP, MySQL, and all required IIS modules installed. You’re now set to configure IIS and link it with PHP.

---

### **Step 8: Register PHP with IIS Using PHP Manager**

Now that PHP has been extracted and installed, we need to register it within IIS using PHP Manager so the server knows how to handle `.php` files.



#### 🔹 1. Open IIS as Administrator

- Click **Start**, search for **IIS** or **Internet Information Services (IIS) Manager**
- Right-click it and select **Run as Administrator**

> ⚠️ PHP Manager may not show or work properly unless IIS is opened with admin rights.



#### 🔹 2. Register PHP with PHP Manager

- In IIS, click your **server name** in the left panel (not a specific site)
- Double-click **PHP Manager**

<img src="https://i.imgur.com/8Czhjwd.png" alt="Open PHP Manager in IIS" width="700"/>

- Click **“Register new PHP version”**
- Browse to: **C:\PHP\php-cgi.exe**


<img src="https://i.imgur.com/tPHKCyx.png" alt="Browse for php-cgi.exe" width="700"/>

- Click **OK** to register PHP

You should now see PHP listed as registered in PHP Manager.

<img src="https://i.imgur.com/j9IS2H3.png" alt="PHP Successfully Registered in IIS" width="350"/>

> ✅ PHP is now linked to IIS and ready to process `.php` files.



#### 🔹 3. Reload IIS

To apply the changes:

- In IIS, click your **server name** again
- In the right panel (Actions), click **Stop**
- Then click **Start**
 <img src="https://i.imgur.com/sOOVgpa.png" alt="Select php-cgi.exe in PHP Manager" width="150"/>

> 🔄 This restarts the IIS service with PHP enabled and ready to go.

---

### **Step 9: Install osTicket v1.15.8**

Now that IIS is configured and PHP is registered, it’s time to deploy the osTicket web files.



#### 🔹 1. Extract the osTicket ZIP File

Inside the `osTicket-Installation-Files` folder on your Desktop, locate: **osTicket-v1.15.8.zip**


- Right-click the ZIP file → **Extract All**
- You’ll see a folder named **`upload`** after extraction



#### 🔹 2. Copy the Upload Folder to IIS Web Root

- Navigate to: **C:\inetpub\wwwroot**


- Copy the **`upload`** folder from the extracted osTicket files
- Paste it into the **`wwwroot`** directory



#### 🔹 3. Rename the Folder to “osTicket”

Still in `C:\inetpub\wwwroot`:

- Right-click the folder named **upload**
- Choose **Rename**
- Rename it to: `osTicket`


<img src="https://i.imgur.com/e7Wnh9Y.png" alt="osTicket folder in wwwroot" width="600"/>

> ✅ IIS will now serve the osTicket application from `http://localhost/osTicket` or your VM's public IP.


--- 

### **Step 10: Launch the osTicket Web Installer**

With the osTicket files in place and IIS properly configured, it’s time to test that the web app loads correctly.



#### 🔹 1. Reload IIS and Start the osTicket Site

- Open **IIS Manager** (as Administrator)
- In the **left panel**, click your **server name**
- In the **right panel**, under Actions, click: **Stop**


Then immediately click: **Start**


> 🔄 This restarts the IIS service to ensure all changes are applied.



In IIS:

- Expand **Sites**
- Expand **Default Web Site**
- Click on **osTicket**
- In the right-hand panel under **Actions**, click: **Browse *:80 (http)**

  <img src="https://i.imgur.com/l6eXFup.png" alt="Browse osTicket in IIS" width="500"/>

> 🌐 This will open your default web browser and navigate to `http://localhost/osTicket` on the VM.

If everything was done correctly, you should see the osTicket installation page like the one below:

<img src="https://i.imgur.com/Z3FvIln.png" alt="osTicket Installer Page" width="500"/>

If everything was done correctly, you should see the osTicket installation page.

> ✅ *If the page doesn’t load, double-check that you renamed the folder to `osTicket`, restarted IIS, and that IIS is serving from the correct directory (`C:\inetpub\wwwroot\osTicket`).*

#### 🔹 2. Enable Missing PHP Extensions

You may notice that certain PHP extensions are reported as missing or disabled on the installer page.

To enable them:

- Go back to **IIS**
- Expand **Sites → Default Web Site → osTicket**
- Double-click **PHP Manager**

<img src="https://i.imgur.com/8Czhjwd.png" alt="PHP Manager in IIS" width="600"/>

- Click **“Enable or disable an extension”**



- Enable the following extensions:

`php_imap.dll`
`php_intl.dll`
`php_opcache.dll`

<img src="https://i.imgur.com/o3JgLQb.png" alt="Enable or Disable PHP Extensions" width="400"/>



- After enabling them, refresh the osTicket installer page in your browser to confirm the extensions are now active



#### 🔹 3. Rename the osTicket Config File

Before continuing with installation, you need to rename the config file:

- Navigate to: C:\inetpub\wwwroot\osTicket\include


- Find the file: ost-sampleconfig.php




- Rename it to: ost-config.php

<img src="https://i.imgur.com/mrROIUc.png" alt="Enabled PHP Extensions" width="400"/>





#### 🔹 4. Set Permissions for ost-config.php

- Right-click on `ost-config.php` → **Properties**
- Go to the **Security** tab







- Click **Advanced**
- Click **Disable inheritance**

<img src="https://i.imgur.com/PPlPk4s.png" alt="ost-sampleconfig.php Location" width="400"/>

- Remove all existing permissions
- Click **Add** → Set new principal as **Everyone**

<img src="https://i.imgur.com/EVLAuAJ.png" alt="Renamed to ost-config.php" width="400"/>

- Grant **Full control** to **Everyone**

<img src="https://i.imgur.com/rEWABdT.png" alt="Security Tab in Properties" width="400"/>


- Click **Apply** and **OK**

<img src="https://i.imgur.com/i0ObfEW.png" alt="Set Everyone to Full Control" width="200"/>

> 🔐 This ensures osTicket can write to the config file during installation.


---

### **Step 11: Complete the osTicket Installation & Configure Database (HeidiSQL)**

Now that the environment is fully configured, it’s time to finalize the osTicket installation via the browser and set up your MySQL database using HeidiSQL.



#### 🔹 1. Launch the osTicket Web Installer

In your browser, with `http://localhost/osTicket` open, begin filling in the setup fields.

<img src="https://i.imgur.com/TkHNQ0L.png" alt="osTicket Installer Page Fields" width="600"/>

- Under **Helpdesk Settings**, you’ll be asked to enter things like:
  - Helpdesk name
  - Default email
  - Admin name
  - Admin username/password

> ⚠️ **Note:** I’m building a helpdesk for a mock cybersecurity firm called CyberFix. You can follow along with the same setup, or create your own company name and branding if you want to customize it.  
> Just **make sure you remember the admin username and password** so you can log in afterward.



#### 🔹 2. Install HeidiSQL

- Go to your **osTicket-Installation-Files** folder
- Locate and install: **HeidiSQL_Installer.exe**

<img src="https://i.imgur.com/w2pwuj9.png" alt="Create osTicket DB in HeidiSQL" width="400"/>

- After installing, open **HeidiSQL**





#### 🔹 3. Create a New MySQL Session

- In HeidiSQL, create a new session with the following credentials:

Username: `root`
Password: `root`


- Click **Open** to connect

- Once inside, right-click the left panel → **Create New → Database**

- Name the new database: `osTicket`


> ✅ The database is now ready to connect with osTicket





#### 🔹 4. Finish osTicket Setup in Browser

- Go back to the **osTicket installation page**
- Scroll to the **MySQL Database Settings** section

Enter:

MySQL Database: `osTicket`

MySQL Username: `root`

MySQL Password: `root`

<img src="https://i.imgur.com/cTYDNei.png" alt="Open HeidiSQL" width="500"/>

- Click: **Install Now!**


> 🎉 If everything is entered correctly, osTicket will complete the installation and you’ll be taken to the admin panel login page.











