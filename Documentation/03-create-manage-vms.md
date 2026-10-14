# Task 03 — Create and Manage Virtual Machines

## Objective

Create a virtual machine on ESXi01, install a guest operating system, connect it to the configured VM network, and verify that the guest boots and can access the network.

The practical scope completed in this task:

- Create a VM in the ESXi Host Client.
- Attach installation media from the datastore.
- Configure CPU, memory, virtual disk, and network adapter.
- Install Debian 13.
- Verify guest startup and basic network access.

Snapshots, clones, and templates are not included in this task; they are planned for Task 04.

---

## Lab Environment

| Component | Configuration |
|---|---|
| Physical host | Windows |
| Virtualization platform | VMware Workstation |
| Hypervisor | Nested VMware ESXi 8.0 Update 2 |
| ESXi management address | `192.168.1.41` |
| VM name | `debian-13.7.0` |
| Installed guest OS | Debian GNU/Linux 13 (64-bit) |
| vCPU | 1 |
| Memory | 2 GB configured (about 1.95 GB shown in VM details) |
| Virtual disk | 15 GB configured; originally 8 GB |
| Network port group | `VM Network` |
| Guest IP shown by ESXi | `192.168.1.252` |
| Installation ISO | `debian-13.7.0-amd64-netinst.iso` |

---

## 1. Create the Virtual Machine

In the ESXi Host Client, **Virtual Machines → Create / Register VM** was used to create a new virtual machine.

The VM was named:

```text
debian-13.7.0
```

The compatibility shown by ESXi was:

```text
ESXi 8.0 U2 virtual machine
```

The VM was created on the available datastore.

---

## 2. Attach the Installation ISO

The CD/DVD drive was configured to use the Debian installation ISO stored on the ESXi datastore:

```text
debian-13.7.0-amd64-netinst.iso
```

The virtual CD/DVD drive was connected for installation. The ISO was selected from the datastore rather than a physical host CD/DVD device.

---

## 3. Configure Virtual Hardware

The VM hardware details observed in ESXi were:

| Setting | Value |
|---|---|
| CPU | 1 vCPU |
| Memory | 2 GB |
| Hard disk | 15 GB |
| Network adapter | `VM Network` |
| Network connection | Connected |
| USB controller | USB 2.0 |
| SCSI controller | VMware Paravirtual was shown during setup |

The disk was initially displayed as **Thick provisioned, lazily zeroed**. Thin provisioning was later selected during the settings discussion, but the final saved provisioning type was not independently verified.

The virtual disk capacity was increased from 8 GB to 15 GB. Increasing the virtual disk capacity does not automatically confirm that the guest partition and filesystem have expanded.

---<img width="1516" height="687" alt="image" src="https://github.com/user-attachments/assets/806bbd14-03d4-42db-9501-0ffd33adf70e" />


## 4. Configure the VM Network

The virtual network adapter was connected to:

```text
VM Network
```

The **Connect at power on** option was enabled.

This connects the VM's virtual NIC to the selected ESXi port group. Internet access still depends on the full network path and its configuration; selecting a port group alone does not guarantee Internet access.

---

## 5. Install Debian 13

The VM was powered on and the Debian graphical installer was opened from the attached ISO.

Debian 13 was installed on the VM's virtual disk. After installation, the guest booted and the user opened the web browser.

---

## 6. Verification

The following results were confirmed during the lab:

- [x] The VM was created in ESXi.
- [x] Debian 13 installed and booted.
- [x] The guest desktop/graphical environment opened.
- [x] The web browser opened inside Debian.
- [x] The user confirmed that Internet access worked.
- [x] ESXi showed the VM network adapter as `VM Network (Connected)`.
- [x] ESXi displayed guest IP address `192.168.1.252`.

These checks verify basic VM operation and network access. They do not constitute a full performance, security, or resilience test.

---

## 7. Follow-up Items

### Guest OS type mismatch

ESXi displayed this mismatch:

- Configured guest OS: `Debian GNU/Linux 4 (64-bit)`
- Running guest OS: `Debian GNU/Linux 13 (64-bit)`

Update the VM's Guest OS setting to the appropriate Debian 13 option if available, then verify that the warning disappears. This follow-up was not confirmed as completed.

### Virtual disk provisioning

The final saved provisioning type was not verified after Thin provisioning was selected in the settings discussion. Do not treat Thin provisioning as confirmed until it is checked in the VM settings.

### Guest partition and filesystem

The virtual disk was increased to 15 GB. Check the partition and filesystem from inside Debian before assuming the entire additional capacity is available to the guest.

### Datastore usage display

The VM summary showed a storage figure of approximately 17.08 GB while the configured virtual disk was 15 GB. The difference was not investigated because the user chose to leave it for now.

---

## 8. What This Task Practiced

- Creating a VM in ESXi Host Client.
- Selecting a datastore and attaching an ISO.
- Assigning virtual CPU, memory, disk, and network resources.
- Understanding the role of an ESXi port group.
- Installing a Linux guest OS.
- Verifying boot, browser access, guest IP, and network connectivity.
- Distinguishing virtual disk capacity from guest partition/filesystem capacity.

---

## Task Status

**Completed —** Debian 13 was installed, booted, and used successfully, and basic network access was confirmed. The follow-up items above remain open and are not claimed as completed.

## Next Task

**Task 04 — Manage Snapshots, Clones and Templates**
