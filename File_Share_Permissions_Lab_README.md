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

## 🖼️ Media  
👉 Screenshots will be integrated into the demonstration steps below.  

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
3. Shared the folders and assigned permissions:  
   - **read-access** → `Domain Users` → *Read*  
   - **write-access** → `Domain Users` → *Read/Write*  
   - **no-access** → `Domain Admins` → *Read/Write*  
   - **accounting** → left untouched for now  

**What I Learned:**  
File share permissions let you design controlled access. By default, users don’t have access unless explicitly granted.  

---

### **Step 3 – Test File Shares as Normal User**  
On **Client-1** (logged in as `mydomain\gef.hoc`):  
- Navigated to `\\DC-1` in File Explorer.  
- Attempted to access each folder:  
  - ✅ Could open **read-access**, but only read files.  
  - ✅ Could open **write-access** and also create/edit files.  
  - ❌ Could **not** open **no-access**.  

**What I Learned:**  
Permissions behaved exactly as configured. Normal domain users could read or write only where allowed, while restricted folders remained locked.  

---

### **Step 4 – Configure ACCOUNTANTS Group**  
On **DC-1**:  
1. In Active Directory Users and Computers, created a new security group: **ACCOUNTANTS**.  
2. On the **accounting** folder, granted:  
   - **ACCOUNTANTS** → *Read/Write*.  

On **Client-1**:  
- As a normal user (not in group), access to **accounting** failed as expected.  
- After adding the same user into **ACCOUNTANTS** group on DC-1, logging back in allowed full access.  

**What I Learned:**  
Group-based access control is scalable and efficient. Instead of editing each user’s permissions individually, roles (like “Accountants”) can manage access cleanly across the domain.  

---

## ✅ Conclusion  
This lab reinforced my understanding of **file share permissions** and **Active Directory group management**. I practiced:  
- Creating and sharing folders with different access levels.  
- Testing user vs. admin permissions.  
- Implementing group-based security for controlled access.  

These skills are essential for **System Administrators**, especially when managing file servers and sensitive departmental data.  
