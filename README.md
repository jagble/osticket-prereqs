<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# osTicket-prereqs
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

- [OsTicket Download](https://drive.google.com/uc?export=download&id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD)
- [Pre-Installation Checklist](https://docs.google.com/document/d/1DyjX8LeVU98LjhXO2t2K2F0aHywI2N9GD57T3taO5qo/edit?tab=t.0)
- Item 3

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

> ✅ *From here, you can begin installing software, setting up your ticketing system, or making system changes as needed.*



---
