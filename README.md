# VMware ESXi and vCenter Administration Lab

A hands-on home lab for learning VMware ESXi host administration and vCenter management. The lab is built in small tasks and each task is documented only after it has been completed and verified.

## Lab Environment

- Physical host: Windows
- Virtualization platform: VMware Workstation
- Nested hypervisor: VMware ESXi
- Network: NAT for initial host management
- Scope: ESXi administration, storage, virtual machines, host networking, vCenter, and selected vSphere features
- Veeam: excluded from this project

## Task Progress

| Task | Description | Status | Documentation |
|---|---|---|---|
| 01 | Install and Configure ESXi01 | Completed | [Task 01](Documentation/01-esxi01-installation.md) |
| 02 | Configure ESXi Management Network and Host Access | Planned | Added after completion |
| 03 | Configure Datastores and VMFS Storage | Planned | Added after completion |
| 04 | Configure Standard vSwitches and Port Groups | Planned | Added after completion |
| 05 | Create and Manage Virtual Machines | Planned | Added after completion |
| 06 | Manage Snapshots, Clones and Templates | Planned | Added after completion |
| 07 | Manage ESXi Users, Roles and Security | Planned | Added after completion |
| 08 | Deploy and Configure ESXi02 | Planned | Added after completion |
| 09 | Configure Networking Between ESXi Hosts | Planned | Added after completion |
| 10 | Configure Shared Storage for Both Hosts | Planned | Added after completion |
| 11 | Deploy vCenter Server Appliance | Planned | Added after completion |
| 12 | Add Hosts to vCenter and Organize Inventory | Planned | Added after completion |
| 13 | Configure vCenter Permissions and Roles | Planned | Added after completion |
| 14 | Configure Distributed Switching | Planned | Added after completion |
| 15 | Configure and Test vMotion | Planned | Added after completion |
| 16 | Configure vSphere HA | Planned | Added after completion |
| 17 | Configure vSphere DRS | Planned | Added after completion |
| 18 | Host Maintenance and Lifecycle Management | Planned | Added after completion |

## Working Method

1. Complete the task in the lab.
2. Verify the result.
3. Record the actual configuration, verification, and any troubleshooting.
4. Add the task documentation to this repository.
5. Update this README.

Advanced features depend on the ESXi/vCenter versions, licensing, hardware resources, and network/storage design. They will be tested before being marked complete.

## Current Result

ESXi01 starts successfully, has a management IP address, and its web interface is accessible. A datastore was created through the ESXi Host Client.
