# Task 04 — Manage Snapshots and Clones

## Objective

Create and test a VM snapshot, restore the VM to the saved state, and create an independent Debian VM by exporting and importing the VM files.

## Lab Environment

| Component | Details |
|---|---|
| Hypervisor | Nested VMware ESXi 8.0 Update 2 |
| ESXi Host Client | `https://192.168.1.41/ui` |
| Original VM | `debian-13.7.0` |
| Clone VM | `debian-13.7.0-clone` |
| Guest OS | Debian GNU/Linux 13 |
| Datastore type | VMFS6 |
| Datastore capacity shown | 99.75 GB |
| Free space shown before the task | 27.32 GB |

## 1. Create a Snapshot

In the ESXi Host Client, open the original VM and use **Actions → Snapshots → Take snapshot**.

The snapshot was created with these details:

| Setting | Value |
|---|---|
| Name | `Debian-Before-Changes` |
| Description | `Baseline snapshot before testing snapshot and clone operations` |

The snapshot appeared in the snapshot manager.
<img width="989" height="605" alt="image" src="https://github.com/user-attachments/assets/58fa0dfb-d356-427c-b79a-5628cd97fa8d" />

## 2. Test Snapshot Revert

A test directory and file were created inside Debian after the snapshot. The VM was then reverted to `Debian-Before-Changes`.

**Observed result:** the test directory disappeared after the revert. This confirmed that the VM returned to the saved snapshot state.

A snapshot is a point-in-time state of a VM, not an independent backup. Changes made after the snapshot can be lost when reverting.  
<img width="1012" height="662" alt="image" src="https://github.com/user-attachments/assets/78c72e58-945b-489f-b41d-c3eba69bad35" />
<img width="1001" height="665" alt="image" src="https://github.com/user-attachments/assets/f9849b80-4fb8-4c45-a967-7e3dcde9112f" />



## 3. Export the Original VM

The ESXi Host Client's **Export** action was used. The download list showed these files:

- `debian-13.7.0.ovf`
- `debian-13.7.0.mf`
- `debian-13.7.0-1.vmdk`

The NVRAM file was not selected. The ISO was not included in the selected export files. The files were downloaded to the Windows host.

## 4. Import a Clone

In **Virtual Machines → Create / Register VM**, the option **Deploy a virtual machine from an OVF or OVA file** was selected.

The imported VM was named:

```text
debian-13.7.0-clone
```

Deployment settings observed:

- Network mapping: `VM Network`
- Disk provisioning: `Thin` was selected
- Power on automatically: left unchecked

The import completed, and the new VM booted into Debian with the same user login as the original VM.
<img width="1305" height="608" alt="image" src="https://github.com/user-attachments/assets/9814b9b8-76a4-47bb-a568-51671163c6ad" />

## 5. Verify the Clone

The following results were observed:

| Check | Original VM | Clone VM |
|---|---|---|
| VM name in ESXi | `debian-13.7.0` | `debian-13.7.0-clone` |
| IPv4 address shown in Debian | `192.168.1.252` | `192.168.1.43` |
| Default gateway | `192.168.1.1` | `192.168.1.1` |
| Initial hostname | `debian` | `debian` |
| Initial `/etc/machine-id` | `fc86a4bdd24b42cb8f1f6b95c5618ec6` | Same value as original |

The clone initially had the same hostname and machine ID as the original. The hostname was changed on the clone, and its `/etc/machine-id` was cleared and regenerated. The terminal then reported:

```text
Initializing machine ID from random generator.
```

A new machine ID was displayed afterward. The original VM's machine ID was not changed. The final hostname was reported by the user as changed, but the final hostname command output was not captured in the available evidence.

The clone booted successfully and had a different IPv4 address from the original. This verifies successful import and basic operation; it does not prove every VM setting or service is independent.
<img width="1533" height="685" alt="image" src="https://github.com/user-attachments/assets/85c592cd-cca2-4cd0-b43d-c878057aa595" />

## 6. Notes and Limitations

- The clone was created by exporting the original VM and deploying the OVF/VMDK files as a new VM.
- The exported files were downloaded to Windows; they were not stored as a reusable image on the ESXi datastore.
- The datastore had 27.32 GB free before this task. The final free space after export/import was not recorded.
- The final hostname output and the final saved disk provisioning type were not independently verified.

## What This Task Practiced

- Creating and managing a VM snapshot.
- Reverting a VM to a saved state and verifying the result.
- Exporting a VM to OVF/VMDK files.
- Deploying a new VM from exported files.
- Checking IP address, default gateway, hostname, and machine ID.
- Understanding why a cloned guest may need a unique hostname and machine ID.

## Task Status

**Completed for the documented scope.** Snapshot creation and revert were tested, and the Debian clone was imported and booted. Template work is not included in this task.

## Next Task

Continue with **Task 05 — Manage ESXi Users, Roles and Security**.
