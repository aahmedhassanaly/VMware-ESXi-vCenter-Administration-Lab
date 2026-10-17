# Task 10 — Configure Distributed Switching

## Objective

Create a vSphere Distributed Switch (vDS), attach both ESXi hosts, and verify guest network connectivity through a distributed port group without moving the existing management VMkernel adapter.

## Environment

- vCenter: `vcsa01.lab.local` (`192.168.1.200`)
- Datacenter: `Lab-Datacenter`
- ESXi01: `192.168.1.41`
- ESXi02: `192.168.1.68`
- VMware Workstation network mode for the additional nested ESXi adapter: Bridged
- Distributed switch: `DSwitch-Lab`
- Distributed port group used for the guest test: `DPortGroup`

## Implementation

### 1. Create the Distributed Switch

Created `DSwitch-Lab` in `Lab-Datacenter`.

- Distributed switch version: `8.0.0`
- Number of uplinks: `1`
- Network I/O Control: Enabled
- MTU: `1500` bytes
- Multicast filtering mode: IGMP/MLD snooping
- Discovery protocol: Cisco Discovery Protocol, Listen

### 2. Add the ESXi Hosts and Uplinks

Added ESXi01 (`192.168.1.41`) and ESXi02 (`192.168.1.68`) to `DSwitch-Lab`.

A second virtual network adapter was available to each nested ESXi host as `vmnic1`. It was assigned to `Uplink 1`; the existing `vmnic0` and management VMkernel adapter `vmk0` were left on the standard switch.

The vCenter Distributed Switch port view showed both uplinks with `Link Up`:

- `192.168.1.41 - vmnic1`
- `192.168.1.68 - vmnic1`
<img width="1528" height="520" alt="image" src="https://github.com/user-attachments/assets/26a89d3e-f58a-4f22-b688-81cee490a228" />

### 3. Configure and Use a Distributed Port Group

The distributed port group `DPortGroup` was present with VLAN access ID `0`. The Debian test clone, `debian-13.7.0-clone`, was connected to this port group for verification.

The guest reported:

- Interface: `ens33`
- IPv4 address: `192.168.1.43/24`
- Default gateway: `192.168.1.1`
<img width="1536" height="528" alt="image" src="https://github.com/user-attachments/assets/09e18256-9a54-44fe-b0a5-99d8c6dfb3eb" />

### 4. Verify Connectivity

The user observed successful ping responses from the Debian clone to both ESXi management IPs:

- `192.168.1.41`
- `192.168.1.68`

The initial ping to `192.168.1.1` failed, then the user confirmed that the gateway ping worked after subsequent troubleshooting. The final successful gateway output was not captured in the conversation, so the result is recorded as user-confirmed rather than screenshot-verified.
<img width="922" height="484" alt="image" src="https://github.com/user-attachments/assets/ac28a1a2-a4bd-46a5-aed5-8f2c2646b201" />

## Verification

- [x] `DSwitch-Lab` exists with version `8.0.0`.
- [x] `DPortGroup` exists.
- [x] ESXi01 and ESXi02 are listed on the distributed switch.
- [x] `vmnic1` is assigned to `Uplink 1` on both hosts and shown as `Link Up`.
- [x] Existing `vmnic0` and management `vmk0` were not migrated.
- [x] Debian clone was connected to `DPortGroup`.
- [x] Ping to both ESXi management IPs succeeded.
- [x] User confirmed the gateway ping subsequently succeeded.

## Notes and Follow-up

- `Link Up` confirms the adapter link state; it does not by itself prove end-to-end connectivity.
- VLAN ID `0` means the port group does not add an 802.1Q VLAN tag. It does not by itself guarantee network isolation.
- The successful guest connectivity tests demonstrate that the test VM could reach the listed addresses; they do not validate every possible distributed-switch feature.
- ESXi01 showed high memory usage during the task. Continue monitoring host and VCSA resource alarms.
- vMotion, vSphere HA, and DRS have not been configured as part of this task.

## Task Status

**Completed** — The distributed switch and port group were created, both ESXi hosts were attached with an uplink shown as Link Up, and guest connectivity was tested.
