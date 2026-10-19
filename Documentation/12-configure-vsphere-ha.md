# Task 12 — Configure and Test vSphere HA

## Objective

Enable vSphere HA on the cluster, review Admission Control, and verify the current HA status using the vSphere Client.

## Environment

- vCenter: `vcsa01.lab.local` (`192.168.1.200`)
- Datacenter: `Lab-Datacenter`
- Cluster: `Cluster-HA-Image`
- ESXi01 management IP: `192.168.1.41`
- ESXi02 management IP: `192.168.1.68`

## Implementation

### 1. Configure the HA Cluster

Created and used the cluster `Cluster-HA-Image` with image management enabled. vSphere HA was enabled. DRS and vSAN remained disabled.

The HA configuration included:

- Host Monitoring: Enabled
- Host Failure Response: `Restart VMs`
- Host failures cluster tolerates: `1`
- Define host failover capacity by: `Cluster resource Percentage`
- Override calculated failover capacity: Enabled
- Reserved failover CPU capacity: `50%`
- Reserved failover Memory capacity: changed from `67%` to `50%`
- Performance degradation VMs tolerate: `100%`
<img width="1530" height="545" alt="image" src="https://github.com/user-attachments/assets/bee2ed18-b80a-4bca-a588-ed75ca099f33" />

### 2. Verify the HA Summary

After the memory failover capacity was changed to 50%, the vSphere HA summary provided by the user showed:

| Item | Observed value |
|---|---:|
| Primary host | `192.168.1.41` |
| Hosts connected to primary | 1 |
| Hosts not connected to primary | 0 |
| HA agent not reachable | 0 |
| HA agent configuration error | 0 |
| Hosts failed | 0 |
| Network isolated | 0 |
| Network partitioned | 0 |
| Protected VMs | 2 |
| Unprotected VMs | 1 |
| Cluster total memory | 13.86 GB |
| Failover memory capacity | 50% (6.93 GB) |
| Cluster total CPU | 44.93 GHz |
| Failover CPU capacity | 50% (22.46 GHz) |
| Failover resource auto-computation | Off |

These are the values reported in the summary at that time.
<img width="1529" height="627" alt="image" src="https://github.com/user-attachments/assets/2b6c7ca6-6be0-45b9-8aec-fb0d3cb6b8a5" />

### 3. Review the Reported Alarms

The user reported these HA-related alarms during the task:

- `Insufficient resources to satisfy vSphere HA failover level`
- `The number of vSphere HA heartbeat datastores ... is 0, which is less than required: 2`
- `debian-13.7.0-clone vSphere HA virtual machine failover failed`
- A failover operation status reporting one VM waiting for a retry
- No management network redundancy on both hosts

The user later reported that the VM failover-failed alarm disappeared. No successful restart of the VM on the alternate host was confirmed.

The user confirmed there is no shared datastore in the lab. The existing VMFS datastores on the two hosts were observed with different names and UUIDs.

## Verification

- [x] vSphere HA enabled on `Cluster-HA-Image`.
- [x] Host Monitoring enabled and Host Failure Response set to `Restart VMs`.
- [x] The HA summary reported both hosts connected to the primary at the time of observation.
- [x] The HA summary reported zero unreachable HA agents, zero HA agent configuration errors, and zero failed hosts at the time of observation.
- [x] Reserved failover CPU capacity set to `50%`.
- [x] Reserved failover Memory capacity changed to `50%`.
- [ ] Verify that a VM successfully restarts on the surviving host after a host failure (not tested).
- [ ] Recheck the current heartbeat datastore and resource alarms (still open).

## Notes and Follow-up

- The HA summary and the disappearance of an alarm do not prove that host-failure recovery succeeded.
- No shared datastore is available in the current lab, and access to VM files from both hosts has not been established.
- The initial HA component installation attempts reported `Installing HA components failed on the cluster` and `Applying HA VIBs on the cluster encountered a failure`. DNS resolution and access to the vCenter micro-depot XML over HTTP port 9084 were later verified from both hosts, but the exact cause of the initial installation failure was not confirmed.
- ESXi01 has reported high memory usage. The VCSA previously reported low memory after its allocation was reduced to 4 GB.
- This task is marked complete for HA configuration and status review. It does not represent a successful host-failure recovery test; VM restart on the alternate host remains unverified.


## Task Status

**Completed** — HA configuration and status review are complete. A successful VM failover has not been verified; heartbeat datastore, resource capacity, and management network redundancy warnings remain follow-up items.
