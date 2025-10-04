# Network File Shares & Permissions (Active Directory Lab)

---

## 🔎 Project Summary

This project shows how I built and validated secure **network file shares** using **Active Directory** and **group-based permissions**. I created role-appropriate folders (read/write/no-access/accounting), tested access from a standard user workstation, and then enforced least privilege by granting access only through a dedicated security group (**ACCOUNTANTS**).

**Environments used:**  
- Microsoft Azure (VMs)  
- Windows Server (DC-1)  
- Windows 10 (Client-1)  

**Technologies/Services:** Active Directory, SMB Shares, NTFS, Local/Network Access Tokens, AD Security Groups, RDP

---

## 🖼️ Media (Screenshots)

### Organizational Prep & Admin Actions
- UAC Prompt for admin credentials  
  ![Admin UAC Prompt](images/network%20discovery3.PNG)

- Creating an Organizational Unit for groups  
  ![New OU](images/new%20organazational%20unit.PNG)

- Creating a new group (ACCOUNTANTS) in ADUC  
  ![New Group Menu](images/new%20group.PNG)  
  ![Group created](images/new%20group2.PNG)

### Folder Creation
![Creating four folders](images/create%204%20folders.PNG)  
![Folders created](images/create%204%20folders2.PNG)

### Share Permission Settings
- **Read-Only folder shared with Domain Users**  
  ![Read-only permissions](images/Permission%20Read(1).PNG)  
  ![Read-only permissions](images/Permission%20Read(2).PNG)  
  ![Read-only permissions](images/Permission%20Read(3).PNG)

- **Write-access shared with Domain Users (Read/Write)**  
  ![Write permissions](images/Permissions%20Read&Write.PNG)

- **No-access restricted to Domain Admins**  
  ![No-access restricted](images/no-access.PNG)

- **Accounting shared with ACCOUNTANTS group**  
  ![Accounting folder permissions](images/accounting%20folder%20permissions.PNG)

- **General folder sharing view**  
  ![Share dialog](images/sharefolder.PNG)

### Access Tests (Client-1 as standard user)
- **Read-Only Share (Denied Write Attempt):**  
  ![Read-access attempt denied](images/access%20the%20folders1.PNG)

- **Write-Access Share (Able to Create/Edit):**  
  ![Write-access working](images/access%20the%20folders2.PNG)

- **No-Access Share (Completely Denied):**  
  ![No-access denied](images/access%20the%20folders3.PNG)

### Group Membership
![Adding user to ACCOUNTANTS group](images/member%20of%20the%20ACCOUNTANTS.PNG)  
![gef.hoc as member of ACCOUNTANTS](images/member%20of%20the%20ACCOUNTANTS(2).PNG)

### Network Discovery Troubleshooting
![Network discovery disabled](images/network%20discovery.PNG)  
![Turning on network discovery](images/network%20discovery2.PNG)  
![Admin enabling discovery](images/network%20discovery3.PNG)

---

## 🛠️ Implementation Steps (Server + Client)

### 1) Power on VMs
- Ensure **DC-1** and **Client-1** are running in Azure.

### 2) Create baseline shares on DC-1
On **C:\\** create four folders:  
`read-access`, `write-access`, `no-access`, `accounting`  

![All four folders visible on C drive](images/create%204%20folders2.PNG)

### 3) Configure Share/NTFS Permissions
- `read-access` → Domain Users: **Read only**  
- `write-access` → Domain Users: **Read/Write**  
- `no-access` → Domain Admins only  
- `accounting` → ACCOUNTANTS security group (Read/Write)

### 4) Validate from Client-1 as a standard user
- Attempt to write to `read-access` → **Denied** ✅  
- Write to `write-access` → **Successful** ✅  
- Open `no-access` → **Denied** ❌  

### 5) Enforce role access with a security group
- Created **ACCOUNTANTS** group inside `_GROUPS` OU.  
- Shared the `accounting` folder with **ACCOUNTANTS (Read/Write)**.  
- Added `gef.hoc` into the **ACCOUNTANTS** group.  
- After re-login → `\\DC-1\accounting` accessible ✅  

---

## 🎥 Demonstration Results

- **Before group membership**: user is denied access to the `accounting` folder.  
  ![Access denied to accounting](images/try%20to%20access%20the%20accountants%20folder.%20It%20should%20fail.PNG)

- **After adding to ACCOUNTANTS**: user can open and create/edit files.  
  ![Successful access to accounting](images/try%20to%20access%20the%20accounting%20again.PNG)

- **read-access**: user can open but **cannot write**.  
- **write-access**: user can create and edit files.  
- **no-access**: blocked from entry.  
- **accounting**: blocked until added to **ACCOUNTANTS**, then successful access.

---

## 📚 What I Learned

- Difference between **Share vs NTFS permissions** (NTFS is more restrictive).  
- Importance of **group-based access** control.  
- Why **user re-login** is required after group membership changes.  
- Troubleshooting access issues with **network discovery**.

---

## 🗂️ Repo Layout

```
.
├─ README.md  ← you are here
├─ images/    ← all screenshots above
└─ docs/
   └─ checklist-and-rubric-notes.md
```

---

## 🔒 Security Takeaways

- Always apply **least privilege**.  
- Use **groups, not users** for permissions.  
- Validate with real **end-user accounts**.  
- Document every access configuration.

