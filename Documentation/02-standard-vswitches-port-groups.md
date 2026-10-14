# Task 02 — Configure Standard vSwitches and Port Groups

## Objective

Review the existing standard virtual switch and create a port group for virtual machine networking while keeping ESXi management access available.

## Environment

- ESXi host: ESXi01
- Management IP: 192.168.1.41
- VMware Workstation network mode: Bridged
- Standard switch: `vSwitch0`
- Uplink adapter: `vmnic0` (`nvmxnet3`)

## Implementation

### 1. Review the Standard Switch

Opened **Networking → Virtual switches** and reviewed the existing networking configuration.

- Standard switch: `vSwitch0`
- Uplink: `vmnic0`
- Management VMkernel adapter: `vmk0`
- Management IP: `192.168.1.41`

### 2. Review Management Network

The existing `Management Network` port group was present and accessible.

- VLAN ID: `0`
- Active ports shown: `1`
- VMkernel adapter: `vmk0`
- Enabled service: Management
- Promiscuous mode: Reject
- Forged transmits: Reject
- MAC address changes: Reject
- Notify switches: Yes
- Failback: Yes
- Load-balancing policy: Route based on originating port ID

The management network was left unchanged because access to the ESXi Host Client was working.

### 3. Create VM Network

Opened **Networking → Port groups** and created a port group named `VM Network`.

- Port group name: `VM Network`
- Standard switch: `vSwitch0`
- VLAN ID: `0`

The port group is available for connecting virtual machine network adapters in later tasks.
<img width="1536" height="694" alt="image" src="https://github.com/user-attachments/assets/3f57bf25-179b-4bdf-95b7-3aee147985d5" />

## Verification

- [x] `vSwitch0` is present.
- [x] `vmnic0` is shown as the ESXi virtual uplink adapter.
- [x] `Management Network` remains accessible.
- [x] `vmk0` uses management IP `192.168.1.41`.
- [x] Both `Management Network` and `VM Network` appear in the port group list.
- [x] VMware Workstation is configured to use Bridged networking for ESXi01.

## Notes

- `vmnic0` is the network adapter presented to ESXi by VMware Workstation; it is not a second physical NIC in the computer.
- `vSwitch0` is the virtual switch inside ESXi.
- A port group provides network settings that can be selected by a VM or VMkernel interface.
- `vmk0` is the ESXi VMkernel management interface. It is not the same as a VM's virtual NIC.
- VLAN ID `0` means the port group does not add an 802.1Q VLAN tag. It does not by itself guarantee network isolation.
- The `Management Network` and `VM Network` port groups share `vSwitch0`; they are not automatically isolated from each other.

## Task Status

**Completed** — The existing standard switch and management network were reviewed, and `VM Network` was created for future virtual machines.
