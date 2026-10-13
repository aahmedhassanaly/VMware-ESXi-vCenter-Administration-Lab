# VMware ESXi and vCenter Administration Lab

A hands-on home lab for learning VMware ESXi host administration and vCenter management. Each task is documented only after it has been completed and verified.

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
| 02 | Verify Management Access and Configure Datastores/VMFS Storage | Planned | Added after completion |
| 03 | Configure Standard vSwitches and Port Groups | Planned | Added after completion |
| 04 | Create and Manage Virtual Machines | Planned | Added after completion |
| 05 | Manage Snapshots, Clones and Templates | Planned | Added after completion |
| 06 | Manage ESXi Users, Roles and Security | Planned | Added after completion |
| 07 | Deploy and Configure ESXi02 | Planned | Added after completion |
| 08 | Configure Networking Between ESXi Hosts | Planned | Added after completion |
| 09 | Configure Shared Storage for Both Hosts | Planned | Added after completion |
| 10 | Deploy vCenter Server Appliance | Planned | Added after completion |
| 11 | Add Hosts to vCenter and Organize Inventory | Planned | Added after completion |
| 12 | Configure vCenter Permissions and Roles | Planned | Added after completion |
| 13 | Configure Distributed Switching | Planned | Added after completion |
| 14 | Configure and Test vMotion | Planned | Added after completion |
| 15 | Configure vSphere HA | Planned | Added after completion |
| 16 | Configure vSphere DRS | Planned | Added after completion |
| 17 | Host Maintenance and Lifecycle Management | Planned | Added after completion |

## Working Method

1. Complete the task in the lab.
2. Verify the result.
3. Record the actual configuration, verification, and any troubleshooting.
4. Add the task documentation to this repository.
5. Update this README.

Advanced features depend on the ESXi/vCenter versions, licensing, hardware resources, and network/storage design. They will be tested before being marked complete.

## Current Result

ESXi01 starts successfully, has a management IP address, and its web interface is accessible. A datastore was created through the ESXi Host Client.
