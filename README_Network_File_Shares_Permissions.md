# Network File Shares & Permissions (Active Directory Lab)

> **Status:** Complete • **Author:** Josephine Lopez • **Tech Focus:** Windows Server + AD + SMB Share Security  
> _A hands-on lab demonstrating how I designed, implemented, and verified NTFS/Share permissions and group-based access control in a small Active Directory environment._

---

## 🔎 Project Summary

This project shows how I built and validated secure **network file shares** using **Active Directory** and **group-based permissions**. I created role-appropriate folders (read/write/no-access/accounting), tested access from a standard user workstation, and then enforced least privilege by granting access only through a dedicated security group (**ACCOUNTANTS**).

**Environments used:**  
- Microsoft Azure (VMs)  
- Windows Server (DC-1)  
- Windows 10 (Client-1)  

**Technologies/Services:** Active Directory, SMB Shares, NTFS, Local/Network Access Tokens, AD Security Groups, RDP

---

## 🛠️ Implementation Steps

### 1) Power on VMs
- Ensure **DC-1** and **Client-1** are running in Azure.

### 2) Organizational Prep & Admin Actions
- Logged in with administrative credentials  
  ![Admin UAC Prompt](images/network%20discovery3.PNG)

- Created an Organizational Unit `_GROUPS` in Active Directory Users and Computers  
  ![New OU](images/new%20organazational%20unit.PNG)

- Created a new **ACCOUNTANTS** group inside `_GROUPS`  
  ![New Group Menu](images/new%20group.PNG)  
  ![Group created](images/new%20group2.PNG)

### 3) Create baseline shares on DC-1
On **C:\\** create four folders:  
`read-access`, `write-access`, `no-access`, `accounting`  

![Creating four folders](images/create%204%20folders.PNG)  
![Folders created](images/create%204%20folders2.PNG)

### 4) Configure Share/NTFS Permissions
- `read-access` → Domain Users: **Read only**  
  ![Read-only permissions](images/Permission%20Read(1).PNG)  
  ![Read-only permissions](images/Permission%20Read(2).PNG)  
  ![Read-only permissions](images/Permission%20Read(3).PNG)

- `write-access` → Domain Users: **Read/Write**  
  ![Write permissions](images/Permissions%20Read&Write.PNG)

- `no-access` → Domain Admins only  
  ![No-access restricted](images/no-access.PNG)

- `accounting` → ACCOUNTANTS security group (Read/Write)  
  ![Accounting folder permissions](images/accounting%20folder%20permissions.PNG)

- General sharing view  
  ![Share dialog](images/sharefolder.PNG)

### 5) Validate from Client-1 as a standard user
- Attempt to write to `read-access` → **Denied** ✅  
  ![Read-access attempt denied](images/access%20the%20folders1.PNG)

- Write to `write-access` → **Successful** ✅  
  ![Write-access working](images/access%20the%20folders2.PNG)

- Open `no-access` → **Denied** ❌  
  ![No-access denied](images/access%20the%20folders3.PNG)

### 6) Enforce role access with a security group
- Added `gef.hoc` into the **ACCOUNTANTS** group.  
  ![Adding user to ACCOUNTANTS group](images/member%20of%20the%20ACCOUNTANTS.PNG)  
  ![gef.hoc as member of ACCOUNTANTS](images/member%20of%20the%20ACCOUNTANTS(2).PNG)

- Tested access to the `accounting` folder:  
  - **Before group membership** → Access denied ❌  
    ![Access denied to accounting](images/try%20to%20access%20the%20accountants%20folder.%20It%20should%20fail.PNG)

  - **After group membership** → Able to create/edit files ✅  
    ![Successful access to accounting](images/try%20to%20access%20the%20accounting%20again.PNG)

### 7) Network Discovery Troubleshooting
- At first, Client-1 could not see DC-1 shares.  
- Enabled **Network Discovery** and **File Sharing**.  
  ![Network discovery disabled](images/network%20discovery.PNG)  
  ![Turning on network discovery](images/network%20discovery2.PNG)  
  ![Admin enabling discovery](images/network%20discovery3.PNG)

---

## 📚 What I Learned

- Difference between **Share vs NTFS permissions** (NTFS is more restrictive).  
- Importance of **group-based access** control.  
- Why **user re-login** is required after group membership changes.  
- Troubleshooting access issues with **network discovery**.

---

## 🔒 Security Takeaways

- Always apply **least privilege**.  
- Use **groups, not users** for permissions.  
- Validate with real **end-user accounts**.  
- Document every access configuration.

