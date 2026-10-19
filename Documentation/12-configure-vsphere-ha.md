# Task 12 — Configure and Validate vSphere HA

## Objective
Enable vSphere HA, verify agent status, review Admission Control, and record what is and is not proven by the lab.

## Environment
- vCenter: `vcsa01.lab.local` (`192.168.1.200`)
- Datacenter: `Lab-Datacenter`
- Cluster: `Cluster-HA-Image`
- ESXi01: `192.168.1.41`
- ESXi02: `192.168.1.68`
- ESXi02 version observed: ESXi 8.0 Update 3, build 24677879
- No shared datastore has been established. The hosts' existing VMFS datastores were observed with different names and UUIDs.
- Resource warnings have included high ESXi01 memory usage and low-memory warnings on VCSA.

## Configuration and Observations

### HA setup
- HA enabled on `Cluster-HA-Image`; DRS and vSAN remain disabled.
- Host Monitoring enabled.
- Host Failure Response set to `Restart VMs`.
- Initial HA component configuration attempts failed with messages including `Installing HA components failed on the cluster` and `Applying HA VIBs on the cluster encountered a failure`.
- DNS resolution and HTTP access to the vCenter micro-depot XML on port 9084 were later verified from both hosts. This confirms access to that URL only; it does not prove the initial component installation issue was resolved.

### HA summary provided after the latest Admission Control change
- Primary host: `192.168.1.41`
- Hosts connected to primary: 1; hosts not connected: 0
- HA agent unreachable: 0; HA agent configuration error: 0
- Hosts failed: 0; network isolated: 0; network partitioned: 0
- Protected VMs: 2; unprotected VMs: 1

These values show the summary state at the time reported. They do not prove a successful VM failover.

### Admission Control
- Host failures cluster tolerates: `1`
- Policy: `Cluster resource Percentage`
- Override calculated failover capacity: enabled
- Reserved failover CPU: `50%`
- Reserved failover Memory: changed from `67%` to `50%`
- Performance degradation VMs tolerate: `100%`

After the change, the summary reported total cluster memory `13.86 GB`, failover memory capacity `50% (6.93 GB)`, total cluster CPU `44.93 GHz`, failover CPU capacity `50% (22.46 GHz)`, and failover resource auto-computation `Off`.

The user later reported that the `debian-13.7.0-clone vSphere HA virtual machine failover failed` alarm disappeared. No evidence was provided that the VM restarted on the alternate host as part of a successful host-failure test.

### Outstanding alarms and limitations
- Heartbeat datastore count: `0`, less than the required `2`.
- `Insufficient resources to satisfy vSphere HA failover level` was observed earlier.
- Both hosts reported no management network redundancy.
- ESXi01 reported high memory usage.
- A failover status previously reported one VM waiting for a retry.

Because no shared storage has been configured and the local VMFS datastores have different UUIDs, there is no evidence that both hosts can access the same VM files. A host-failure failover test is therefore not verified.

## Verification Checklist
- [x] HA enabled on the cluster.
- [x] HA summary reported both hosts connected to the primary at the time of observation.
- [x] HA summary reported zero unreachable agents, zero agent configuration errors, and zero failed hosts at the time of observation.
- [x] CPU and memory failover capacity set to 50%.
- [x] User reported that the VM failover-failed alarm later disappeared.
- [ ] Check current heartbeat datastore alarm.
- [ ] Investigate the precise cause of the initial HA component installation failure using logs.
- [ ] Review current resource and memory alarms.
- [ ] Verify VM restart on the surviving host in a controlled host-failure test. This has not been demonstrated.

## Safety Notes
- Do not power off either host or disconnect management networking to simulate failure while vCenter depends on local storage on ESXi01 and VM files have not been shown to be accessible from ESXi02.
- A VM marked HA-protected does not by itself prove it can restart on the other host.
- The lack of shared heartbeat datastores explains the heartbeat warning, but does not independently explain the original HA VIB installation failure.
- Do not disable Admission Control merely to hide a resource warning.
- No VMs or virtual disks were added for this task.

## Official Broadcom References
- [KB 392502 — HA heartbeat datastore count is 0, less than required 2](https://knowledge.broadcom.com/external/article/392502/alert-the-number-of-vsphere-ha-heartbeat.html)
- [KB 318871 — HA heartbeat datastore count below required](https://knowledge.broadcom.com/external/article/318871/ha-error-the-number-of-heartbeat-datasto.html)
- [KB 396016 — Insufficient HA failover resources](https://knowledge.broadcom.com/external/article/396016)
- [KB 424373 — Insufficient configured resources for HA failover level](https://knowledge.broadcom.com/external/article/424373/error-insufficient-configured-resources.html)

## Task Status
**In Progress** — HA is enabled and the cluster summary reported connected agents, but successful VM failover has not been verified. Shared heartbeat storage is absent and resource/heartbeat alarms require follow-up.
