# VMware ESXi and vCenter Administration Lab

**Portfolio Priority: Virtualization & IT Infrastructure Operations**

A hands-on lab for practicing VMware ESXi administration and vCenter management. The focus is practical configuration, verification, troubleshooting, and accurate documentation—not simply completing a list of features.

## Lab Environment

| Component | Details |
|---|---|
| Physical host | Windows 11 Pro |
| Virtualization platform | VMware Workstation |
| Hypervisor | Nested VMware ESXi |
| ESXi version observed during VM creation | ESXi 8.0 Update 2 |
| ESXi01 management IP | `192.168.1.41` |
| ESXi02 management IP | `192.168.1.68` |
| vCenter hostname | `vcsa01.lab.local` |
| vCenter IP | `192.168.1.200` |
| DNS server | Technitium DNS on physical Windows 11 host (`192.168.1.144`) |
| Scope | ESXi host administration, virtual machines, networking, storage, vCenter, and selected vSphere features |

**Project boundary:** Veeam is managed in a separate repository and is excluded from this project.

## Lab Components

- **ESXi01** — first nested ESXi host used for the initial tasks.
- **ESXi02** — second nested ESXi host; Host Client access, datastore presence, and basic virtual networking were verified.
- **VCSA** — vCenter Server Appliance `8.0.3.00400`, hostname `vcsa01.lab.local`, IP `192.168.1.200`.
- **Technitium DNS** — DNS server on the physical Windows 11 host, resolving the VCSA forward and reverse records.
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
| 07 | [Deploy vCenter Server Appliance](Documentation/07-vcenter-server-appliance.md) | ✅ Completed |
| 08 | [Add Hosts to vCenter and Organize Inventory](Documentation/08-add-hosts-vcenter-inventory.md) | ✅ Completed |
| 09 | [Configure vCenter Permissions and Roles](Documentation/09-vcenter-permissions-roles.md) | ✅ Completed |
| 10 | [Configure Distributed Switching](Documentation/10-configure-distributed-switching.md) | ✅ Completed |
| 11 | [Configure and Test vMotion](Documentation/11-configure-and-test-vmotion.md) | ✅ Completed |
| 12 | Configure vSphere HA | Planned |
| 13 | Configure vSphere DRS | Planned |
| 14 | Host Maintenance and Lifecycle Management | Planned |

Task status is updated only after practical work and verification. Planned tasks are not presented as implemented features.

## Core Skills Practiced

### Virtualization
- Nested ESXi deployment
- VM creation and virtual hardware configuration
- Guest OS installation
- ISO and datastore use
- VM network adapter and port group selection
- vCenter Server Appliance deployment and management

### Networking and DNS
- Standard vSwitches and port groups
- Distributed vSwitch and distributed port group configuration
- ESXi management VMkernel networking
- Basic guest network verification
- DNS A and PTR records for VCSA
- Forward and reverse DNS lookup verification
- Distinguishing UDP DNS tests from TCP port verification

### Storage
- Datastore creation
- Virtual disk capacity and provisioning types
- Distinguishing virtual disk size from guest partition and filesystem capacity
- VM snapshots, revert testing, OVF/VMDK export, and clone deployment

### Operations
- Verify actual results before documenting them
- Record open follow-up items explicitly
- Keep task documentation aligned with the README
- Track resource alerts rather than hiding them

## Current Verified Result

- ESXi01 is accessible at `192.168.1.41`.
- ESXi02 is accessible at `192.168.1.68`; its Host Client showed ESXi 8.0 Update 3 and host state Normal. A 100 GB datastore and `vSwitch0` with `Management Network` and `VM Network` were present.
- Ping succeeded between ESXi01 (`192.168.1.41`) and ESXi02 (`192.168.1.68`) in both directions with 0% packet loss.
- VCSA version `8.0.3.00400`, build `24322831`, is deployed with hostname `vcsa01.lab.local` and IP `192.168.1.200`.
- Technitium DNS on Windows 11 (`192.168.1.144`) successfully resolved VCSA forward and reverse records from both Windows and VCSA.
- VCSA SSO domain `vsphere.local` showed status Running in the VAMI screenshot.
- VCSA memory was reduced from 12 GB to 4 GB after installation. VAMI then reported that the appliance was running low on memory. This remains an open resource constraint.
- vCenter inventory contains `Lab-Datacenter`, with ESXi01 (`192.168.1.41`) and ESXi02 (`192.168.1.68`) both shown as `Connected`. Three VMs are shown under ESXi01: `debian-13.7.0`, `debian-13.7.0-clone`, and `VMware vCenter Server`.
- Open vSphere alarms include `Host memory usage` on ESXi01, `Datastore usage on disk` at Critical severity, and `Memory Exhaustion on vcsa01`. ESXi01's datastore showed 99.75 GB capacity, 88 GB used, and 11.75 GB free. Avoid creating additional VMs or virtual disks until the critical datastore alarm is reviewed.
- Debian 13 is installed on VM `debian-13.7.0`.
- A snapshot named `Debian-Before-Changes` was created and tested; a test directory created after the snapshot disappeared after Revert.
- `debian-13.7.0-clone` was deployed from exported OVF/VMDK files and booted successfully. The clone was assigned IPv4 address `192.168.1.43`, while the original showed `192.168.1.252`.
- The VM has 1 vCPU, 2 GB configured memory, a 15 GB configured virtual disk, and a network adapter connected to `VM Network`.
- The user confirmed the guest boots, opens a browser, and has Internet access.
- `DSwitch-Lab` was created with version `8.0.0`, Network I/O Control enabled, and one uplink.
- ESXi01 and ESXi02 were added to `DSwitch-Lab`; `vmnic1` on both hosts was assigned to `Uplink 1` and displayed `Link Up`. The existing `vmnic0` and management `vmk0` were left on the standard switch.
- Debian clone `debian-13.7.0-clone` was connected to distributed port group `DPortGroup`. Ping to both ESXi management IPs succeeded, and the user confirmed that ping to gateway `192.168.1.1` subsequently succeeded.

## Open Follow-up Items

- Verify DNS over TCP port 53 separately; successful UDP lookups do not prove TCP connectivity.
- Restart Windows and verify Technitium starts automatically and the DNS records remain available.
- Monitor VCSA service health and memory pressure with the current 4 GB allocation.
- Verify access to vSphere Client using `https://vcsa01.lab.local/ui`.
- Correct the Debian VM's configured Guest OS type to Debian 13 and confirm the mismatch warning is gone.
- Verify the final saved virtual disk provisioning type.
- Check whether Debian's partition and filesystem can use the full configured virtual disk capacity.
- Investigate storage usage only if it becomes relevant to later work.

## Working Method

1. Perform the task in the lab one step at a time.
2. Verify the actual result using the UI, command output, or a relevant test.
3. Document only what was implemented and observed.
4. Preserve existing screenshots and evidence when updating documentation.
5. Update the task status and README after verification.
6. Keep unresolved findings in a follow-up section instead of hiding or overstating them.

Advanced features depend on the ESXi/vCenter version, licensing, available resources, and storage/network design. They will be tested before being marked complete.
