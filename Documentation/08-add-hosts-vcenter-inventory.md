# Task 08 — Add Hosts to vCenter and Organize Inventory

## Objective

Add the existing ESXi hosts to vCenter and organize them under a datacenter. Verify the host connection states and confirm that the existing virtual machines appear in the inventory.

## Environment

- vCenter Server: `vcsa01.lab.local`
- vCenter IP: `192.168.1.200`
- Datacenter: `Lab-Datacenter`
- ESXi01: `192.168.1.41`
- ESXi02: `192.168.1.68`
- ESXi01 version shown during host addition: ESXi 8.0.3, build `24677879`
- ESXi01 image shown by the wizard: `ESXi 8.0 U3e - 24677879`

## Implementation

### 1. Create the Datacenter

1. Opened **Hosts & Clusters** in the vSphere Client.
2. Created a datacenter named `Lab-Datacenter` under `vcsa01.lab.local`.

A datacenter is a logical inventory container. It organizes hosts and related vSphere objects; it does not add physical CPU, memory, or storage capacity.

### 2. Add ESXi01

1. Started **Add Host** from `Lab-Datacenter`.
2. Entered `192.168.1.41` as the host address.
3. Selected **Use the current image on host** in the host lifecycle step.
4. Left Lockdown Mode **Disabled** to preserve direct Host Client access during the learning lab.
5. Reviewed the summary and continued through the wizard.

The wizard showed ESXi 8.0.3 build `24677879` and image `ESXi 8.0 U3e - 24677879`.

### 3. Add ESXi02

Added `192.168.1.68` to the same datacenter. The first attempt displayed an internal error saying that the task failed to start and advising that the host have enough resources. The user later confirmed that both hosts were added and appeared in vCenter. The root cause of the initial error was not independently verified.

## Verification

The vSphere Client showed the following inventory:

- `Lab-Datacenter`
- Two hosts: `192.168.1.41` and `192.168.1.68`
- Three VMs under ESXi01:
  - `debian-13.7.0`
  - `debian-13.7.0-clone`
  - `VMware vCenter Server`

The Datacenters view showed **2 Hosts** and **3 VMs** for `Lab-Datacenter`. The host list showed both ESXi hosts as **Connected**.

- [x] `Lab-Datacenter` created.
- [x] ESXi01 added to vCenter.
- [x] ESXi02 added to vCenter.
- [x] Both hosts shown as Connected.
- [x] Existing VMs visible under ESXi01.
- [x] Datacenter inventory count showed 2 hosts and 3 VMs.

These checks confirm host registration and inventory visibility. They do not verify vMotion, shared storage, vSphere HA, or DRS.

## Troubleshooting and Open Issues

The vSphere Client showed these alarms:

| Object | Alarm | Severity |
|---|---|---|
| ESXi01 (`192.168.1.41`) | Host memory usage | Warning |
| Datastore `data store` | Datastore usage on disk | Critical |
| VCSA (`vcsa01.lab.local`) | Memory Exhaustion on vcsa01 | Warning |

The ESXi01 Summary page showed:

| Resource | Observed value |
|---|---:|
| Memory capacity | 8 GB |
| Memory used | 7.48 GB |
| Memory free | 537.98 MB |
| Datastore capacity | 99.75 GB |
| Datastore used | 88 GB |
| Datastore free | 11.75 GB |

The alarms were not reset or cleared. The Critical datastore alarm has not yet been investigated in detail.

**Operational precaution:** Do not create additional VMs or virtual disks until the datastore alarm and available capacity have been reviewed. The memory alerts also remain open.

## Lessons Learned

- vCenter provides a central inventory for registered ESXi hosts and their VMs.
- A datacenter is a logical container, not a resource pool or a physical server.
- A host showing **Connected** does not mean that all its resources are healthy.
- Successful host registration does not prove that advanced features such as vMotion, HA, or DRS are ready.

## Task Status

**Completed** — Both ESXi hosts were added to vCenter and shown as Connected under `Lab-Datacenter`. The existing VMs appeared in the inventory. Memory and datastore alarms remain open.

## Next Task

**Task 09 — Configure vCenter Permissions and Roles**
