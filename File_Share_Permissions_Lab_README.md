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
Setting up and verifying connectivity is critical before testing any permissions, ensuring that the domain controller and clients can communicate.  

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
File share permissions let you design controlled access. By default, users don’t have access unless explicitly granted.  

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

### **Step 4 – Configure ACCOUNTANTS Group**  
On **DC-1**:  
1. In Active Directory Users and Computers, created a new security group: **ACCOUNTANTS**.                                                                          <img width="1182" height="715" alt="new organazational unit" src="https://github.com/user-attachments/assets/44bbee92-8b32-4a11-87dc-e3273d5c28cd" />
<img width="1066" height="755" alt="new group" src="https://github.com/user-attachments/assets/0f150da6-2228-400f-8a71-de33222fd5a7" />
<img width="575" height="532" alt="new group2" src="https://github.com/user-attachments/assets/0216a283-92cd-4f23-b6a1-cc0e84cfe359" />
2. On the **accounting** folder, granted:  
   - **ACCOUNTANTS** → *Read/Write*.  
<img width="1424" height="744" alt="accounting folder permissions" src="https://github.com/user-attachments/assets/33158de0-bedb-4fe2-996b-803db1b40929" />


On **Client-1**:  
- As a normal user (not in group), access to **accounting** failed as expected.
  <img width="1442" height="755" alt="try to access the accountants folder  It should fail" src="https://github.com/user-attachments/assets/85877bb1-df09-46d5-8c0d-dbf4577a916b" />
- After adding the same user into **ACCOUNTANTS** group on DC-1, logging back in allowed full access.
  **Adding user**
 <img width="965" height="644" alt="member of the ACCOUNTANTS" src="https://github.com/user-attachments/assets/45c8db09-3f18-41e0-bddc-df4b157d474d" />
 <img width="576" height="591" alt="member of the ACCOUNTANTS(2)" src="https://github.com/user-attachments/assets/facab7d6-4ca9-472f-b1cd-196784314955" />
  **accessing Folder again**
 <img width="1562" height="643" alt="try to access the accounting again" src="https://github.com/user-attachments/assets/55819ef9-d5f9-4f87-a7a5-aee9a91d3b62" />
 

 
**What I Learned:**  
Group-based access control is scalable and efficient. Instead of editing each user’s permissions individually, roles (like “Accountants”) can manage access cleanly across the domain.  

---

## ✅ Conclusion  
This lab reinforced my understanding of **file share permissions** and **Active Directory group management**. I practiced:  
- Creating and sharing folders with different access levels.  
- Testing user vs. admin permissions.  
- Implementing group-based security for controlled access.  

These skills are essential for **System Administrators**, especially when managing file servers and sensitive departmental data.  
