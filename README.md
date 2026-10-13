# VMware ESXi & vCenter Administration Lab

**Portfolio Priority: Virtualization & Infrastructure Operations**

A practical VMware home lab built with nested ESXi in VMware Workstation. The project focuses on ESXi host administration, virtual networking, storage, VM lifecycle management, security, vCenter, vMotion, High Availability, DRS, and host maintenance.

## Lab Environment

| Component | Planned Configuration |
|---|---|
| Physical Host | Windows with VMware Workstation |
| ESXi01 | 4 vCPU, 8 GB RAM, 100 GB virtual disk |
| ESXi02 | 2–4 vCPU, 4–6 GB RAM, 100 GB virtual disk |
| vCenter | VCSA, deployed in a later task after checking available resources |
| Backup Platform | Not included in this project |

The lab is for learning and testing. Feature availability depends on the VMware version, license, hardware resources, networking, and storage configuration. Advanced features will only be documented as working after they are tested.

## Tasks

| # | Task | Status |
|---:|---|:---:|
| 01 | [Install and Configure ESXi01](Documentation/01-esxi01-installation.md) | ⬜ Not Started |
| 02 | [Configure ESXi Management Network and Host Access](Documentation/02-esxi-management-network.md) | ⬜ Not Started |
| 03 | [Configure Datastores and VMFS Storage](Documentation/03-datastore-vmfs.md) | ⬜ Not Started |
| 04 | [Configure Standard vSwitches and Port Groups](Documentation/04-standard-switches-port-groups.md) | ⬜ Not Started |
| 05 | [Create and Manage Virtual Machines](Documentation/05-vm-lifecycle.md) | ⬜ Not Started |
| 06 | [Manage Snapshots, Clones, and Templates](Documentation/06-snapshots-clones-templates.md) | ⬜ Not Started |
| 07 | [Configure ESXi Users, Roles, and Security](Documentation/07-esxi-security.md) | ⬜ Not Started |
| 08 | [Deploy and Configure ESXi02](Documentation/08-esxi02-installation.md) | ⬜ Not Started |
| 09 | [Configure Networking Between ESXi Hosts](Documentation/09-host-networking.md) | ⬜ Not Started |
| 10 | [Configure Shared Storage for Both Hosts](Documentation/10-shared-storage.md) | ⬜ Not Started |
| 11 | [Deploy vCenter Server Appliance](Documentation/11-vcenter-deployment.md) | ⬜ Not Started |
| 12 | [Add Hosts to vCenter and Organize Inventory](Documentation/12-vcenter-host-inventory.md) | ⬜ Not Started |
| 13 | [Configure vCenter Permissions and Roles](Documentation/13-vcenter-permissions.md) | ⬜ Not Started |
| 14 | [Configure vSphere Distributed Switching](Documentation/14-distributed-switching.md) | ⬜ Not Started |
| 15 | [Configure and Test vMotion](Documentation/15-vmotion.md) | ⬜ Not Started |
| 16 | [Configure vSphere High Availability](Documentation/16-vsphere-ha.md) | ⬜ Not Started |
| 17 | [Configure vSphere DRS](Documentation/17-vsphere-drs.md) | ⬜ Not Started |
| 18 | [Host Maintenance and Lifecycle Management](Documentation/18-host-maintenance-lifecycle.md) | ⬜ Not Started |

Detailed task documentation: [Documentation](Documentation/).

## Documentation Method

Each task follows this workflow:

**Scenario → Requirements → Implementation → Verification → Troubleshooting → Lessons Learned**

Troubleshooting follows:

**Problem → Evidence → Hypothesis → Test → Fix → Verify**

Task status and results will be updated after the relevant steps have been performed and verified. The README will be reviewed as the final project summary after Task 18.

## Important Notes

- This is a nested home lab, not a production design.
- ESXi01 and ESXi02 resource allocations may need adjustment to keep the physical PC responsive.
- vCenter and advanced vSphere features have specific resource, compatibility, networking, storage, and licensing requirements.
- Do not assume a feature is available until its requirements have been checked and the lab test has succeeded.
- No Veeam tasks are included in this project.
