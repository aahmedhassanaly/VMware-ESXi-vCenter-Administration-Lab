# VMware ESXi and vCenter Administration Lab

**Portfolio Priority: Virtualization & IT Infrastructure Operations**

A hands-on lab for practicing VMware ESXi administration and vCenter management. The focus is practical configuration, verification, troubleshooting, and accurate documentation—not simply completing a list of features.

## Lab Environment

| Component | Details |
|---|---|
| Physical host | Windows |
| Virtualization platform | VMware Workstation |
| Hypervisor | Nested VMware ESXi |
| ESXi version observed during VM creation | ESXi 8.0 Update 2 |
| ESXi01 management IP | `192.168.1.41` |
| ESXi02 management IP | `192.168.1.68` |
| ESXi02 version observed | ESXi 8.0 Update 3 |
| VMware Workstation network mode for ESXi01 | Bridged |
| Storage | Datastore created on ESXi01 |
| Scope | ESXi host administration, virtual machines, networking, storage, vCenter, and selected vSphere features |

**Project boundary:** Veeam is managed in a separate repository and is excluded from this project.

## Lab Components

- **ESXi01** — first nested ESXi host used for the initial tasks.
- **ESXi02** — second nested ESXi host; Host Client access, datastore presence, and basic virtual networking were verified.
- **vSwitch0** — standard virtual switch on ESXi01.
- **Management Network** — port group used by the ESXi management VMkernel interface.
- **VM Network** — port group created for virtual machine networking.
- **debian-13.7.0** — Debian 13 guest VM used to practice VM creation and guest installation.
- **debian-13.7.0-clone** — VM imported from the original VM's exported OVF/VMDK files and booted successfully.

## Tasks

| # | Task | Status |
|---:|---|:---:|
| 01 | [Install ESXi01, Configure Management Access, and Create a Datastore](Documentation/01-esxi01-installation.md) | ✅ Completed |
| 02 | [Configure Standard vSwitches and Port Groups](Documentation/02-standard-vswitches-port-groups.md) | ✅ Completed |
| 03 | [Create and Manage Virtual Machines](Documentation/03-create-manage-vms.md) | ✅ Completed |
| 04 | [Manage Snapshots and Clones](Documentation/04-snapshots-and-clones.md) | ✅ Completed |
| 05 | [Manage ESXi Users and Roles](Documentation/05-esxi-users-and-roles.md) | ✅ Completed |
| 06 | [Deploy and Configure ESXi02](Documentation/06-esxi02-deployment.md) | ✅ Completed |
| 07 | Configure Shared Storage for Both Hosts | Planned |
| 08 | Deploy vCenter Server Appliance | Planned |
| 09 | Add Hosts to vCenter and Organize Inventory | Planned |
| 10 | Configure vCenter Permissions and Roles | Planned |
| 11 | Configure Distributed Switching | Planned |
| 12 | Configure and Test vMotion | Planned |
| 13 | Configure vSphere HA | Planned |
| 14 | Configure vSphere DRS | Planned |
| 15 | Host Maintenance and Lifecycle Management | Planned |


Task status is updated only after the practical work and its verification are completed. Planned tasks are not presented as implemented features.

## Core Skills Practiced

### Virtualization
- Nested ESXi deployment
- VM creation and virtual hardware configuration
- Guest OS installation
- ISO and datastore use
- VM network adapter and port group selection

### Networking
- Standard vSwitches and port groups
- ESXi management VMkernel networking
- Basic guest network verification
- Understanding the difference between a virtual switch, port group, VM virtual NIC, and ESXi uplink

### Storage
- Datastore creation
- Virtual disk capacity and provisioning types
- Distinguishing virtual disk size from guest partition and filesystem capacity
- VM snapshots, revert testing, OVF/VMDK export, and clone deployment

### Operations
- Verify actual results before documenting them
- Record open follow-up items explicitly
- Keep task documentation aligned with the README

## Current Verified Result

- ESXi local user `labadmin` was created and assigned the `Read-only` role at the host level. Login and viewing host details succeeded, and a tested change operation was blocked.
- The current Acceptance Level was observed as `Partner`; Active Directory and Smart card authentication were both shown as disabled. These settings were reviewed but not changed.

- ESXi01 is accessible at `192.168.1.41`.
- ESXi02 is accessible at `192.168.1.68`; its Host Client showed ESXi 8.0 Update 3 and host state Normal. A 100 GB datastore and `vSwitch0` with `Management Network` and `VM Network` were present.
- Ping succeeded between ESXi01 (`192.168.1.41`) and ESXi02 (`192.168.1.68`) in both directions with 0% packet loss.
- `vSwitch0` contains the `Management Network` and `VM Network` port groups.
- Debian 13 is installed on VM `debian-13.7.0`.
- A snapshot named `Debian-Before-Changes` was created and tested; a test directory created after the snapshot disappeared after Revert.
- `debian-13.7.0-clone` was deployed from exported OVF/VMDK files and booted successfully. The clone was assigned IPv4 address `192.168.1.43`, while the original showed `192.168.1.252`.
- The VM has 1 vCPU, 2 GB configured memory, a 15 GB configured virtual disk, and a network adapter connected to `VM Network`.
- The user confirmed the guest boots, opens a browser, and has Internet access.

## Open Follow-up Items

- Correct the VM's configured Guest OS type to Debian 13 and confirm the mismatch warning is gone.
- Verify the final saved virtual disk provisioning type.
- Check whether Debian's partition and filesystem can use the full configured virtual disk capacity.
- Investigate the storage usage figure only if it becomes relevant to later work.

These items are intentionally listed as open rather than represented as completed.

## Working Method

1. Perform the task in the lab one step at a time.
2. Verify the actual result using the UI, command output, or a relevant test.
3. Document only what was implemented and observed.
4. Preserve existing screenshots and evidence when updating documentation.
5. Update the task status and README after verification.
6. Keep unresolved findings in a follow-up section instead of hiding or overstating them.

Advanced features depend on the ESXi/vCenter version, licensing, available resources, and storage/network design. They will be tested before being marked complete.
