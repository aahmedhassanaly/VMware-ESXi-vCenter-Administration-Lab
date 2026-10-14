# VMware ESXi and vCenter Administration Lab

A hands-on home lab for learning VMware ESXi host administration and vCenter management. Tasks are combined when they are too small to be useful on their own. Each task is documented after its work is completed and verified.

## Lab Environment

- Physical host: Windows
- Virtualization platform: VMware Workstation
- Nested hypervisor: VMware ESXi
- ESXi01 network mode in VMware Workstation: Bridged
- ESXi01 management IP: 192.168.1.41
- Scope: ESXi administration, storage, virtual machines, host networking, vCenter, and selected vSphere features
- Veeam: excluded from this project

## Task Progress

| Task | Description | Status | Documentation |
|---|---|---|---|
| 01 | Install ESXi01, Configure Management Access, and Create a Datastore | Completed | [Task 01](Documentation/01-esxi01-installation.md) |
| 02 | Configure Standard vSwitches and Port Groups | Completed | [Task 02](Documentation/02-standard-vswitches-port-groups.md) |
| 03 | Create and Manage Virtual Machines | Completed | [Task 03](Documentation/03-create-manage-vms.md) |
| 04 | Manage Snapshots, Clones and Templates | Planned | Added after completion |
| 05 | Manage ESXi Users, Roles and Security | Planned | Added after completion |
| 06 | Deploy and Configure ESXi02 | Planned | Added after completion |
| 07 | Configure Networking Between ESXi Hosts | Planned | Added after completion |
| 08 | Configure Shared Storage for Both Hosts | Planned | Added after completion |
| 09 | Deploy vCenter Server Appliance | Planned | Added after completion |
| 10 | Add Hosts to vCenter and Organize Inventory | Planned | Added after completion |
| 11 | Configure vCenter Permissions and Roles | Planned | Added after completion |
| 12 | Configure Distributed Switching | Planned | Added after completion |
| 13 | Configure and Test vMotion | Planned | Added after completion |
| 14 | Configure vSphere HA | Planned | Added after completion |
| 15 | Configure vSphere DRS | Planned | Added after completion |
| 16 | Host Maintenance and Lifecycle Management | Planned | Added after completion |

## Working Method

1. Complete the task in the lab.
2. Verify the result.
3. Record the actual configuration, verification, and any troubleshooting.
4. Add or update the task documentation in this repository.
5. Update this README.

Advanced features depend on ESXi/vCenter versions, licensing, hardware resources, and network/storage design. They will be tested before being marked complete.

## Current Result

ESXi01 starts successfully, management access works at 192.168.1.41, and a datastore is available. The standard switch vSwitch0 contains the Management Network and the VM Network port groups. A Debian 13 virtual machine has been installed and verified with basic browser and network access.
