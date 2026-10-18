# Task 11 — Configure and Test vMotion

## Objective

Configure a VMkernel interface for vMotion on both ESXi hosts, verify connectivity between the vMotion interfaces, and migrate a running virtual machine to the second host.

## Environment

- vCenter: `vcsa01.lab.local` (`192.168.1.200`)
- Datacenter: `Lab-Datacenter`
- ESXi01 management IP: `192.168.1.41`
- ESXi02 management IP: `192.168.1.68`
- Distributed switch: `DSwitch-Lab`
- Distributed port group: `DPortGroup`
- vMotion VMkernel interface on ESXi01: `vmk1`, `192.168.1.161`
- vMotion VMkernel interface on ESXi02: `vmk1`, `192.168.1.58`

## Implementation

### 1. Configure vMotion VMkernel Networking

A VMkernel adapter named `vmk1` was configured on each host using `DSwitch-Lab` and `DPortGroup`. The vMotion service was enabled on both adapters.

| Host | Management IP | vMotion VMkernel | vMotion IP |
|---|---|---|---|
| ESXi01 | `192.168.1.41` | `vmk1` | `192.168.1.161` |
| ESXi02 | `192.168.1.68` | `vmk1` | `192.168.1.58` |

The existing management adapter `vmk0` remained on `vSwitch0`. The management network was not moved.
<img width="1532" height="518" alt="image" src="https://github.com/user-attachments/assets/4248a7c9-a7c8-481b-9b4b-804824e0c7b3" />
<img width="1533" height="491" alt="image" src="https://github.com/user-attachments/assets/549c7f4f-cc0b-45bf-a2ab-28003d4693c0" />

### 2. Verify vMotion Network Connectivity

The following tests were run successfully:

- From ESXi01: `vmkping -I vmk1 192.168.1.58`
- From ESXi02: `vmkping -I vmk1 192.168.1.161`

The user confirmed that both tests succeeded. This verifies IP connectivity between the two vMotion VMkernel addresses; it does not independently verify every vMotion prerequisite.
<img width="1536" height="720" alt="image" src="https://github.com/user-attachments/assets/1c4abd13-a245-4d36-a335-37d4ee93d901" />

### 3. Migrate the Running VM

The VM `debian-13.7.0-clone` was migrated using the vCenter migration wizard with **Change both compute resource and storage**. The wizard initially displayed a network adapter compatibility warning for `Network adapter 1`; the VM used an `E1000` adapter connected to `DPortGroup`. The migration subsequently completed successfully.

The final vCenter Summary showed:

- Power state: `Powered On`
- Host: `192.168.1.68` (ESXi02)
- Guest OS: Debian GNU/Linux 13 (64-bit)
- VMware Tools: Running, version `12448` (Guest Managed)
- Guest IPv4 address: `192.168.1.43`
- Network: `DPortGroup` (connected)
- Datastore: `data-store`
- Virtual disk: 15 GB, Thin Provision
<img width="1536" height="699" alt="image" src="https://github.com/user-attachments/assets/497c2bdd-571f-4ede-aa47-a159f462bad6" />
<img width="1536" height="521" alt="image" src="https://github.com/user-attachments/assets/85a7c785-2ee4-4cbf-853e-3de50f6c4fef" />

## Verification

- [x] vMotion service enabled on `vmk1` on ESXi01 and ESXi02.
- [x] vMotion VMkernel IP connectivity succeeded in both directions.
- [x] `debian-13.7.0-clone` migration completed.
- [x] The VM was shown as powered on after migration.
- [x] The destination host was shown as ESXi02 (`192.168.1.68`).
- [x] The destination datastore was shown as `data-store`.
- [x] VMware Tools was shown as running after migration.

## Notes and Follow-up

- The two hosts showed different datastore names in vCenter: `data store` on ESXi01 and `data-store` on ESXi02. A shared datastore was not established during this task. The successful **Change both compute resource and storage** migration demonstrates that the selected migration workflow completed in this lab.
- The migration wizard displayed a network adapter compatibility warning before the migration completed. The VM's configured adapter was `E1000`. Review the VM's configured Guest OS type and adapter compatibility separately; do not change the adapter without a reason and a recovery plan.
- Before repeating migrations, verify that both VMkernel IP addresses are unique and reachable, and check host resource alarms and datastore free space.
- ESXi01 previously showed high memory usage and low datastore free space. These resource warnings remain operational follow-up items.
- This task verifies one successful VM migration. It does not prove that all VM types, workloads, or migration scenarios are compatible.

## Task Status

**Completed** — vMotion VMkernel connectivity was tested in both directions, and the Debian clone was successfully migrated to ESXi02 and verified as powered on.
