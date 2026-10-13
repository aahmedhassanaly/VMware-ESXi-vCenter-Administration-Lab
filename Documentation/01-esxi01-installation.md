# Task 01 — Install ESXi01and Create a Datastore

## Objective

Install ESXi as a nested hypervisor in VMware Workstation, confirm management access, and prepare local storage for later lab tasks.

## Environment

- Physical host OS: Windows
- Virtualization platform: VMware Workstation
- Nested host: ESXi01
- VM resources: 4 vCPU, 8 GB RAM, and a 100 GB virtual disk
- Network mode: NAT
- ESXi management IP: 192.168.1.41
- ESXi version: not recorded during this task

## Implementation

### 1. Install ESXi01

1. Created the ESXi01 virtual machine in VMware Workstation.
2. Attached the ESXi installation ISO and started the installer.
3. Installed ESXi on the virtual disk assigned to ESXi01.
4. Restarted the VM and reached the ESXi console.
 
### 2. Verify Management Access

1. Confirmed that ESXi received the management IP address **192.168.1.41**.
2. Opened the ESXi Host Client at **https://192.168.1.41/ui**.
3. Confirmed that the web interface opened successfully.
<img width="995" height="500" alt="image" src="https://github.com/user-attachments/assets/dd622097-052e-42eb-b3d5-7f29b1454c73" />

### 3. Create a Datastore

1. Opened **Storage → Datastores** in the ESXi Host Client.
2. Found that a usable datastore was not initially available.
3. Used the **New datastore** option to create a datastore on the virtual disk assigned to ESXi01.
4. The datastore is referred to in this lab as **data store**.
<img width="1166" height="686" alt="image" src="https://github.com/user-attachments/assets/7c632f2e-b3f6-4baa-94f0-186f1f6ce1f9" />

## Verification

- [x] ESXi01 starts successfully.
- [x] Management IP address **192.168.1.41** is assigned.
- [x] The ESXi Host Client opens in a browser.
- [x] A datastore was created through the ESXi interface.

## Troubleshooting

### No datastore was available initially

**Problem:** The host interface did not show a usable datastore.

**Action:** Opened the datastore creation wizard and created a datastore on the virtual disk assigned to ESXi01.

**Result:** Datastore creation was completed.

## Lessons Learned

- Nested ESXi requires suitable CPU virtualization support and VM settings.
- The management IP address is used to access the ESXi Host Client.
- A newly installed host may need a datastore to be created manually.
- Confirm the selected disk carefully before creating a VMFS datastore. Creating one can erase existing data on the selected disk.

## Task Status

**Completed** — ESXi01 was installed, management access at **192.168.1.41** was verified, and a datastore was created.
