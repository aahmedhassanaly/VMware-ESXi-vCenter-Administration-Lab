# Task 03 — Create and Manage a Virtual Machine

## Objective
Create a virtual machine on the nested ESXi host, install a guest operating system, connect its virtual network adapter to the intended port group, and verify basic operation.

## Environment
- Hypervisor: VMware ESXi 8.0 Update 2 (nested in VMware Workstation)
- VM name: debian-13.7.0
- Guest operating system installed: Debian GNU/Linux 13 (64-bit)
- Guest OS configured in VM settings: Debian GNU/Linux 4 (64-bit) at the time of inspection; ESXi displayed a mismatch warning
- Virtual CPU: 1 vCPU
- Memory: 2 GB configured (approximately 1.95 GB shown in VM details)
- Virtual disk: 15 GB configured after increasing it from the initial 8 GB
- Disk provisioning type selected: Thick provisioned, lazily zeroed initially; user later selected Thin provisioning during the disk settings discussion, but final saved provisioning type was not independently verified
- Network adapter: VM Network (Connected)
- Guest IP address observed: 192.168.1.252
- Installation media: debian-13.7.0-amd64-netinst.iso stored on the ESXi datastore

## Implementation
1. Created a new VM in ESXi Host Client.
2. Attached the Debian 13 netinst ISO from the datastore to the virtual CD/DVD drive.
3. Connected the VM network adapter to the VM Network port group and enabled connection at power-on.
4. Installed Debian 13 using the graphical installer.
5. Started the installed system and opened the web browser. The user confirmed internet access was working.
6. Increased the virtual disk size from 8 GB to 15 GB in the VM settings.

## Verification
- Debian 13 boots and is usable after installation.
- A browser opens inside the guest.
- The user confirmed internet access from the guest.
- ESXi VM summary showed the network adapter as VM Network (Connected).
- Guest IP address shown in ESXi: 192.168.1.252.

## Notes / Follow-up
- ESXi reported that the configured guest OS type (Debian GNU/Linux 4 64-bit) did not match the running guest (Debian GNU/Linux 13 64-bit). Update the VM's Guest OS setting to Debian GNU/Linux 13 (64-bit) when available, then verify the warning is gone.
- The guest disk was increased to 15 GB. A virtual disk capacity change does not necessarily expand the guest partition or filesystem; inspect guest disk layout before relying on the added capacity.
- The final disk provisioning type after the settings discussion was not independently verified.
- The datastore storage figure displayed in the VM summary was not investigated, as the user chose to leave it for now.

## Status
Completed — VM creation, Debian installation, boot, and basic network access were verified by the user. Follow-up notes above remain open.
