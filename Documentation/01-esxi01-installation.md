# Task 01 — Install and Configure ESXi01

## Objective

Install VMware ESXi as a nested hypervisor in VMware Workstation and confirm that the host is ready for administration.

## Environment

- Physical host OS: Windows
- Virtualization platform: VMware Workstation
- Nested host name: ESXi01
- Virtual CPU and memory: configured for the lab
- Virtual disk: configured for the lab
- Network mode: NAT
- ESXi version: not recorded during this task

## Implementation

1. Created the ESXi01 virtual machine in VMware Workstation.
2. Attached the ESXi installation ISO and started the installer.
3. Installed ESXi on the virtual disk assigned to ESXi01.
4. Restarted the virtual machine and reached the ESXi console.
5. Confirmed that the host received a management IP address.
6. Opened the ESXi Host Client in a web browser.
7. Created a datastore from the Host Client because a usable datastore was not initially available.

## Verification

- [x] ESXi01 starts successfully.
- [x] A management IP address is assigned.
- [x] The ESXi web interface opens.
- [x] A datastore was created from the ESXi interface.

## Troubleshooting

### No datastore was available initially

**Problem:** The host interface did not show a usable datastore.

**Action:** Opened the datastore creation option in the ESXi Host Client and created a datastore on the virtual disk assigned to ESXi01.

**Result:** The datastore creation step was completed.

## Lessons Learned

- ESXi can run as a nested hypervisor when the host platform and CPU virtualization settings support it.
- A management IP address allows access to the ESXi Host Client.
- Confirm the selected disk carefully before creating a datastore. Creating a VMFS datastore can erase existing data on the selected disk.

## Task Status

**Completed** — ESXi was installed, the web interface was opened, and a datastore was created.
