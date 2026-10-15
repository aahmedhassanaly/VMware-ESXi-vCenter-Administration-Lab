# Task 08 — Add Hosts to vCenter and Organize Inventory

## Objective

Add the existing ESXi hosts to vCenter and verify that the hosts and their virtual machines appear in the vSphere inventory.

## Lab Environment

| Component | Details |
|---|---|
| vCenter Server | `vcsa01.lab.local` |
| vCenter IP | `192.168.1.200` |
| Datacenter | `Lab-Datacenter` |
| ESXi01 | `192.168.1.41` |
| ESXi02 | `192.168.1.68` |
| ESXi01 version shown during host addition | ESXi 8.0.3 build `24677879` |
| ESXi01 image shown by the wizard | ESXi 8.0 U3e — `24677879` |

## 1. Create the Datacenter

A datacenter named `Lab-Datacenter` was created under `vcsa01.lab.local`.

A vSphere datacenter is a logical inventory container. It organizes hosts, clusters, virtual machines, networks, and storage objects in vCenter. It does not provide CPU, memory, or storage by itself.

## 2. Add ESXi Hosts

The Add Host wizard was used to add these hosts to `Lab-Datacenter`:

- ESXi01: `192.168.1.41`
- ESXi02: `192.168.1.68`

During the wizard for ESXi01, the host lifecycle option was set to **Use the current image on host**. Lockdown Mode was set to **Disabled** to preserve direct Host Client access during this learning lab.

The wizard showed ESXi 8.0.3 build `24677879` and image `ESXi 8.0 U3e - 24677879` for ESXi01.

The host addition initially displayed an internal error on `192.168.1.68` stating that the task failed to start and asking to ensure the system had enough resources. The user later confirmed that both hosts were added and appeared in the inventory. The initial error is recorded as an observed transient failure; its root cause was not independently confirmed.

## 3. Verify the Inventory

The vSphere Client inventory showed:

- `Lab-Datacenter`
- Two hosts: `192.168.1.41` and `192.168.1.68`
- Three virtual machines under ESXi01:
  - `debian-13.7.0`
  - `debian-13.7.0-clone`
  - `VMware vCenter Server`

The Datacenters view showed `Lab-Datacenter` with 2 hosts and 3 VMs. The host list showed both hosts as `Connected`.

This verifies that both hosts were added and that the inventory is visible in vCenter. It does not by itself verify vMotion, shared storage, vSphere HA, or DRS.

## 4. Review Resource Alarms

The vSphere Client showed these alarms:

| Object | Alarm | Severity |
|---|---|---|
| ESXi01 (`192.168.1.41`) | Host memory usage | Warning |
| Datastore `data store` | Datastore usage on disk | Critical |
| VCSA (`vcsa01.lab.local`) | Memory Exhaustion on vcsa01 | Warning |

ESXi01's displayed capacity and usage were 8 GB memory capacity, 7.48 GB used, and 537.98 MB free. Its datastore showed 99.75 GB capacity, 88 GB used, and 11.75 GB free.

These alarms are open operational constraints. They were not cleared or reset as part of this task. Because datastore space is limited and a critical datastore alarm is present, no additional VMs or virtual disks should be created until storage usage is reviewed.

## Verification Checklist

- [x] Created `Lab-Datacenter`.
- [x] Added ESXi01 (`192.168.1.41`) to vCenter.
- [x] Added ESXi02 (`192.168.1.68`) to vCenter.
- [x] Confirmed both hosts appear as `Connected`.
- [x] Confirmed the datacenter inventory shows 2 hosts and 3 VMs.
- [x] Confirmed the existing VMs appear under ESXi01.
- [x] Recorded the current memory and datastore alarms.
- [ ] Investigate the critical datastore usage alarm before creating additional VMs or disks.

## Key Concepts

- **vCenter Server:** Central management for the registered vSphere inventory.
- **Datacenter:** A logical container for organizing vSphere resources.
- **Host:** An ESXi hypervisor that runs virtual machines.
- **Cluster:** A group of hosts used for shared cluster-level capabilities. A cluster was not created in this task.
- **Connected:** vCenter currently has a connection to the host; this does not mean every host resource is healthy.

## Task Status

**Completed for host registration and inventory organization.** Both ESXi hosts are connected and visible in vCenter, and the existing virtual machines are visible under ESXi01. Resource alarms remain open, including a critical datastore usage alarm.

## Next Task

**Task 09 — Configure vCenter Permissions and Roles**
