![image](https://github.com/user-attachments/assets/883d9ab0-bb49-4d89-ba55-b43123f1ceb3)

# Network File Sharing and Permissions Lab

## Azure Resources Used
- **Domain Controller VM (DC-1)** — Windows Server VM acting as the Active Directory Domain Controller
- **Client VM (Client-1)** — Windows client VM joined to the domain
- **Virtual Network (VNet)** — Networking infrastructure connecting the VMs
- **Network Security Groups (NSGs)** — Controlling inbound/outbound traffic for VMs

---

## Lab Objective
Learn how to create shared folders with different permission levels in an Active Directory environment and test access from client machines using different user accounts.

---

## Step 1: Prepare File Shares on Domain Controller (DC-1)

- Log into **DC-1** as your domain admin (e.g., `mydomain.com\jane_admin`)
- On the **C:\ drive**, create 4 folders:
  - `read-access`
  - `write-access`
  - `no-access`
  - `accounting`
    
  ![image](https://github.com/user-attachments/assets/d9f26d7a-620f-4986-b20b-8b28d4ebefd7)

- Share each folder and set permissions as follows:
  - Folder: `read-access`  
    Group: **Domain Users**  
    Permission: **Read**
  - Folder: `write-access`  
    Group: **Domain Users**  
    Permission: **Read/Write**
  - Folder: `no-access`  
    Group: **Domain Admins**  
    Permission: **Read/Write**
  - Folder: `accounting`  
    *(Permissions will be set later)*

    ![image](https://github.com/user-attachments/assets/6d191dd3-71c7-40e5-abab-bb8bc514f701)
    ![image](https://github.com/user-attachments/assets/05e0e3c8-87a0-4819-b8c4-cadefab9c053)
    ![image](https://github.com/user-attachments/assets/4c9018c5-bbd6-4ddf-bcf4-0e5ac9faacc2)




---

## Step 2: Test Access as Normal User

- Log into **Client-1** as a regular domain user (e.g., `mydomain\<someuser>`)
- Access shared folders by navigating to `\\dc-1` (e.g., via Run dialog)
- Test and verify:
  - Which folders are accessible?
  - Which folders allow file creation/modification?
  - Do the permissions match expectations?
![image](https://github.com/user-attachments/assets/8e376756-83d8-4297-9780-9c255ba98b5e)
![image](https://github.com/user-attachments/assets/13efaf20-8b6a-4d91-9e76-bbe5701876da)
![image](https://github.com/user-attachments/assets/12b4a846-1ea0-4c04-a780-83a25a76ff1d)



---

## Step 3: Create Security Group and Assign Permissions

- On **DC-1**, open Active Directory Users and Computers (ADUC)
- Create a new security group named `ACCOUNTANTS`
- Assign permissions on the `accounting` folder as follows:
  - Group: `ACCOUNTANTS`  
  - Permission: **Read/Write**
- As `<someuser>` on **Client-1**, attempt to access `\\dc-1\accounting`  
  - It should **fail** initially
- Log out from Client-1
- Add `<someuser>` to the `ACCOUNTANTS` group in ADUC
- Log back into Client-1 as `<someuser>` and try accessing `\\dc-1\accounting` again  
  - Access should now be **granted**


![image](https://github.com/user-attachments/assets/c6c71e88-d1ca-471e-8ef7-3386e4d9b921)
![image](https://github.com/user-attachments/assets/097c5ddc-26a8-4796-a826-319de7b3cb3e)
![image](https://github.com/user-attachments/assets/61a81826-b9ee-47d7-b966-df65e3b31d32)
![image](https://github.com/user-attachments/assets/8148e1c9-0a0e-436e-9deb-31a2221b766d)



---

## Summary

This lab demonstrates:

- Creating shared folders with various permissions on a domain controller
- Using Active Directory security groups to control folder access
- Testing permissions from a client workstation using different domain users
