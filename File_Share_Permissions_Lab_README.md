# 🔐 File Share Permissions Lab: Active Directory Domain Environment  

## 📌 Project Summary  
This project demonstrates how I configured and tested **file shares with varying permissions** in an Active Directory domain hosted on **Microsoft Azure**.  
I worked with two virtual machines:  

- **DC-1** → Windows Server Domain Controller (`mydomain.com`)  
- **Client-1** → Windows 10 domain-joined client  

**Technologies & Environments Used:**  
- Microsoft Azure (Virtual Machines/Compute)  
- Windows Server (Domain Controller & File Sharing)  
- Windows 10 Client  
- Active Directory Users & Computers (ADUC)  
- File Explorer (Network Shares)  

---

## 🛠️ Demonstration  

### **Step 1 – Prepare VMs**  
- Turned on **DC-1** and **Client-1** VMs in the Azure Portal.  
- Verified domain connectivity.  

**What I Learned:**  
Before doing anything with permissions, it’s important to make sure the servers and clients can communicate. If the machines aren’t connected, nothing else will work.

---

### **Step 2 – Create File Shares on DC-1**  
On **DC-1**:  
1. Logged in as **`mydomain.com\jane_admin`**.  
2. Created 4 folders on the `C:\` drive:  
   - `read-access`  
   - `write-access`  
   - `no-access`  
   - `accounting`
<img width="1477" height="810" alt="create 4 folders" src="https://github.com/user-attachments/assets/ca95adcb-dff1-4929-ad1e-b6684118f1d2" />
<img width="1082" height="708" alt="create 4 folders2" src="https://github.com/user-attachments/assets/73fa6dc0-26ca-4efa-b54d-35c417c0857b" />


3. Shared the folders and assigned permissions:  
   - **read-access** → `Domain Users` → *Read*  
   - **write-access** → `Domain Users` → *Read/Write*  
   - **no-access** → `Domain Admins` → *Read/Write*  
   - **accounting** → left untouched for now  
<img width="1417" height="880" alt="Permission Read(1)" src="https://github.com/user-attachments/assets/94f95104-a95c-4949-a23a-a9d62078881d" />
<img width="532" height="607" alt="Permission Read(2)" src="https://github.com/user-attachments/assets/194bb655-01b4-44e5-849a-5da1698802f7" />
<img width="706" height="541" alt="Permission Read(3)" src="https://github.com/user-attachments/assets/1da34816-18e5-4511-848d-613a22b7a567" />
<img width="1003" height="614" alt="Permissions Read Write" src="https://github.com/user-attachments/assets/cb319be5-fcf1-46ff-9c45-a537853ebaf8" />
<img width="1297" height="747" alt="no-access" src="https://github.com/user-attachments/assets/832237c5-a431-494a-86c3-74ffd448ce4f" />



**What I Learned:**  
Different folders can be given different levels of access. Some users can only view files, others can edit them, and some folders are locked down completely. This is the basic idea behind secure file sharing.  


---

### **Step 3 – Test File Shares as Normal User**  
On **Client-1** (logged in as `mydomain\gef.hoc`):  
- Navigated to `\\DC-1` in File Explorer.
<img width="1481" height="808" alt="sharefolder" src="https://github.com/user-attachments/assets/b266979a-a288-4096-9257-c7d07944bd82" />
 - Attempted to access each folder:  
  - ✅ Could open **read-access**, but only read files.
 <img width="1510" height="806" alt="access the folders1" src="https://github.com/user-attachments/assets/33e86f90-2438-4b4a-8f93-1841604fd4bc" />
  - ✅ Could open **write-access** and also create/edit files.
 <img width="1590" height="762" alt="access the folders2" src="https://github.com/user-attachments/assets/89a62219-213e-4dee-91b3-19872b7af1ab" />
  - ❌ Could **not** open **no-access**.
 <img width="1482" height="811" alt="access the folders3" src="https://github.com/user-attachments/assets/2053ed7e-374a-496a-ae7c-6c45a5f95929" />


**What I Learned:**  
Permissions behaved exactly as configured. Normal domain users could read or write only where allowed, while restricted folders remained locked.  

---

### **Step 4 – Configure the `ACCOUNTANTS` Group & Secure the Share**  

#### 4.1 Create a Security Group in AD
1. On **DC-1**, open **Active Directory Users and Computers (ADUC)**.  
2. (Optional) Create an **Organizational Unit (OU)** for better organization.  
   ![Create OU](https://github.com/user-attachments/assets/44bbee92-8b32-4a11-87dc-e3273d5c28cd)  
3. Right-click → **New → Group**.  
   ![New Group Dialog](https://github.com/user-attachments/assets/0f150da6-2228-400f-8a71-de33222fd5a7)  
4. Name it **ACCOUNTANTS** (Global / Security).  
   ![Group Properties](https://github.com/user-attachments/assets/0216a283-92cd-4f23-b6a1-cc0e84cfe359)  

---

#### 4.2 Grant Permissions on the `accounting` Folder
1. On **DC-1**, right-click `C:\accounting` → **Properties → Sharing** → enable sharing.  
2. Add **ACCOUNTANTS** group → give **Read/Write**.  
   ![Accounting Folder Permissions](https://github.com/user-attachments/assets/33158de0-bedb-4fe2-996b-803db1b40929)  

---

#### 4.3 Validate Access from Client-1 (Before Membership)
- On **Client-1**, logged in as a normal user → tried to open `\\DC-1\accounting`.  
- **Result:** Access denied.  
  ![Access Denied Before Membership](https://github.com/user-attachments/assets/85877bb1-df09-46d5-8c0d-dbf4577a916b)  

---

#### 4.4 Add User to `ACCOUNTANTS` Group
1. On **DC-1**, open **ADUC** → user properties → **Member Of → Add… → ACCOUNTANTS**.  
   ![User Member Of 1](https://github.com/user-attachments/assets/45c8db09-3f18-41e0-bddc-df4b157d474d)  
   ![User Member Of 2](https://github.com/user-attachments/assets/facab7d6-4ca9-472f-b1cd-196784314955)  

---

#### 4.5 Re-Test Access After Membership
- On **Client-1**, signed back in as the same user → opened `\\DC-1\accounting`.  
- **Result:** Access granted with Read/Write.  
  ![Access Granted After Membership](https://github.com/user-attachments/assets/55819ef9-d5f9-4f87-a7a5-aee9a91d3b62)  

**What I Learned:**  
Using groups made the process much easier. Instead of giving access to each person one at a time, I just put the user in the **ACCOUNTANTS** group. This gave them access right away and showed me how group-based permissions make managing folders simpler and more secure.  


---

## ✅ Conclusion  
This lab reinforced my understanding of **file share permissions** and **Active Directory group management**. I practiced:  
- Creating and sharing folders with different access levels.  
- Testing user vs. admin permissions.  
- Implementing group-based security for controlled access.  

These skills are essential for **System Administrators**, especially when managing file servers and sensitive departmental data.  
