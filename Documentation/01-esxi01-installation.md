# Task 01 — Install and Configure ESXi01

## Objective

Install VMware ESXi as a nested hypervisor in VMware Workstation and confirm that the host boots to the Direct Console User Interface (DCUI).

## Scenario

You are preparing the first ESXi host for a home virtualization lab. The host will be used for later storage, networking, virtual machine, and vCenter tasks.

## Planned Lab Configuration

| Setting | Value |
|---|---|
| Virtual machine name | `ESXi01` |
| Hypervisor | VMware ESXi |
| Platform | VMware Workstation on Windows |
| vCPU | 4 |
| RAM | 8 GB |
| Virtual disk | 100 GB |
| Network adapter | NAT for initial setup |
| Nested virtualization | Enabled, if supported by the host and Workstation configuration |

These are the planned values. Record any changes made during the actual deployment.

## Requirements

- A compatible ESXi installer ISO obtained from an authorized source.
- VMware Workstation installed and working.
- CPU virtualization support enabled in the physical computer firmware.
- Enough free disk space for the 100 GB virtual disk and future lab files.
- A working backup of important files before changing host virtualization settings.

## Implementation

### Step 1 — Create the Virtual Machine

1. Open VMware Workstation.
2. Select **Create a New Virtual Machine**.
3. Select the ESXi installer ISO when prompted.
4. Set the virtual machine name to `ESXi01`.
5. Configure 4 virtual CPUs and 8 GB of memory.
6. Create a 100 GB virtual disk.
7. Select a network adapter in NAT mode for initial setup.
8. Finish creating the virtual machine.

### Step 2 — Enable Nested Virtualization

1. Power off the `ESXi01` virtual machine.
2. Open **VM Settings → Processors**.
3. Enable **Virtualize Intel VT-x/EPT or AMD-V/RVI** if the option is available.
4. Save the settings.

If the option is unavailable or the VM cannot power on, stop and troubleshoot the physical host's virtualization settings and Workstation compatibility before continuing. Do not disable Windows security features without checking the actual error and understanding the impact.

### Step 3 — Install ESXi

1. Power on `ESXi01` and boot from the installer ISO.
2. Follow the ESXi installer prompts.
3. Select the virtual disk created for this VM.
4. Confirm the installation target carefully. This must be the virtual disk for `ESXi01`, not a physical disk containing personal data.
5. Complete the installation.
6. Reboot when prompted and disconnect the installer ISO if needed so that the VM boots from its virtual disk.

### Step 4 — Check the DCUI

After reboot, confirm that the ESXi Direct Console User Interface (DCUI) appears. Record the version shown on the screen and the current management network status.

Do not claim that installation succeeded until this check is complete.

## Verification

Check the following items and record the actual result:

| Check | Expected Result | Actual Result |
|---|---|---|
| VM powers on | No Workstation power-on error | Pending |
| ESXi installer completes | Installer reports completion | Pending |
| ESXi boots from disk | DCUI appears after reboot | Pending |
| ESXi version | Version is visible on DCUI | Pending |
| Management network | Current status/address is visible | Pending |

Take a screenshot of the DCUI after a successful boot. Do not include passwords, product keys, or other secrets in screenshots.

## Troubleshooting

Use the following method for any issue:

**Problem → Evidence → Hypothesis → Test → Fix → Verify**

### Problem: Nested virtualization option is missing

- **Evidence:** Record the Workstation version, processor model, and available processor options.
- **Hypothesis:** CPU support, firmware configuration, or the current virtualization platform may limit the option.
- **Test:** Check whether hardware virtualization is enabled on the physical computer and review the exact Workstation warning.
- **Fix:** Correct the confirmed cause only.
- **Verify:** Power on the VM and confirm that no nested-virtualization error appears.

### Problem: VM fails to power on

- **Evidence:** Save the complete error message from VMware Workstation.
- **Hypothesis:** Possible causes include nested virtualization conflicts, insufficient host resources, or an incompatible VM configuration.
- **Test:** Check the error details and Workstation logs before changing settings.
- **Fix:** Apply a change that matches the evidence.
- **Verify:** Power on the VM again and record the result.

### Problem: ESXi does not boot after installation

- **Evidence:** Record the console message and whether the VM boots back into the installer.
- **Hypothesis:** The VM may still boot from the ISO, or the installation may not have completed correctly.
- **Test:** Check the virtual CD/DVD connection and boot sequence.
- **Fix:** Disconnect the ISO if the installation has completed; otherwise, review the installer failure.
- **Verify:** Confirm that the VM boots from its virtual disk to the DCUI.

### Problem: No management IP is shown

- **Evidence:** Record the network adapter type and DCUI network status.
- **Hypothesis:** The management interface may not have received an address or the virtual network may not be connected.
- **Test:** Check the Workstation network adapter and the ESXi management network settings.
- **Fix:** Correct the identified connection or address configuration.
- **Verify:** Confirm the management address before moving to Task 02.

## Result

**Status: Not started.**

Complete the installation first. Then replace the pending verification entries with actual results, attach the relevant evidence, and update this section before marking Task 01 as complete.

## Lessons Learned

Complete this section after the installation and troubleshooting have been verified. Record what worked, what failed, and the reason for any configuration changes.
