# Task 06 — Deploy and Configure ESXi02

## Objective

Deploy a second ESXi host in VMware Workstation and verify that its management interface, storage, and standard virtual networking are available.

## Lab Environment

| Component | Details |
|---|---|
| Physical host | Windows |
| Virtualization platform | VMware Workstation |
| Hypervisor | Nested VMware ESXi |
| Host name shown in Host Client | `localhost.home` |
| ESXi version | ESXi 8.0 Update 3 |
| Management IP | `192.168.1.68` |
| Configured virtual CPUs | 2 |
| Configured memory | 4 GB |
| Datastore | 100 GB; visible in the ESXi Host Client |

## 1. Deploy ESXi02

ESXi02 was created as a virtual machine in VMware Workstation and ESXi was installed.

The VM was configured with 2 virtual CPUs and 4 GB of memory. These values were selected to fit the available lab resources.

## 2. Verify Management Access

The ESXi Host Client opened at:

`https://192.168.1.68/ui`

The Host Client Summary showed ESXi 8.0 Update 3 and the host name `localhost.home`. The host state was shown as Normal.

## 3. Verify Storage

The ESXi Host Client showed a datastore with a capacity of 100 GB.

No new datastore was created during this verification because the datastore was already present.

## 4. Verify Standard Networking

The **Networking → Virtual switches** page showed `vSwitch0` and the `VM Network` port group.

No network settings were changed during this check.

## 5. Verify Licensing

The user checked the Licensing page and reported that its status was the same as ESXi01. No license settings were changed.

## Verification Checklist

- [x] ESXi02 Host Client opened successfully.
- [x] ESXi version shown as 8.0 Update 3.
- [x] Management IP `192.168.1.68` was reachable through the Host Client.
- [x] Host state showed Normal.
- [x] A 100 GB datastore was present.
- [x] `vSwitch0` and `VM Network` were present.
- [x] Configured resources were 2 vCPUs and 4 GB RAM.

## Notes

- The displayed host name is still `localhost.home`; it was not renamed in this task.
- This task verifies that ESXi02 is deployed and its current management, storage, and basic virtual networking are visible. It does not verify host-to-host connectivity, shared storage, vMotion, or vSphere HA.

## Task Status

**Completed.** ESXi02 is accessible, and its version, host state, datastore, and basic virtual networking were checked.

## Next Task

**Task 07 — Configure Networking Between ESXi Hosts**
