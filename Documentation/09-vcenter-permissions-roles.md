# Task 09 — Configure vCenter Permissions and Roles

## Objective

Create a dedicated test user, assign the built-in Read-only role to a limited inventory object, and verify that the user can view the assigned host and its virtual machines without administrative access.

## Environment

- vCenter Server: `vcsa01.lab.local`
- vCenter IP: `192.168.1.200`
- SSO domain: `vsphere.local`
- Datacenter: `Lab-Datacenter`
- ESXi01: `192.168.1.41`
- ESXi02: `192.168.1.68`
- Test user: `lab.operator`

## Implementation

### 1. Create the test user

1. Opened **Menu → Administration → Single Sign On → Users and Groups**.
2. Selected the `vsphere.local` domain.
3. Created the user `lab.operator` with a dedicated lab password.
4. Kept the existing system and service accounts unchanged.

### 2. Assign the Read-only role to ESXi01

1. Opened **Hosts and Clusters**.
2. Selected ESXi01 (`192.168.1.41`).
3. Opened **Permissions** and selected **Add**.
4. Added `VSPHERE.LOCAL\lab.operator`.
5. Selected the built-in **Read-only** role.
6. Enabled propagation to child objects.

The resulting permission entry showed:

| Setting | Observed value |
|---|---|
| User | `VSPHERE.LOCAL\lab.operator` |
| Role | `Read-only` |
| Defined In | This object and its children |

This assignment is scoped to ESXi01 and its child objects. It is not a Global Permission.

## Verification

The following checks were completed using the test account:

- [x] Signed in to vSphere Client as `lab.operator@vsphere.local`.
- [x] Viewed ESXi01 and its details.
- [x] Viewed the VM inventory under ESXi01, including `debian-13.7.0`.
- [x] Confirmed that the user did not have a direct permission entry on ESXi02 during the check.
- [x] The user reported that configuration changes were not available with the Read-only role.

The Power On action was not executed. This task record does not claim that a power operation was tested.

## How to Grant Read-only Access to Both Hosts at Once

For a new user who needs Read-only access to both ESXi hosts in this lab, assign the permission once on the parent datacenter instead of adding it separately to each host:

1. Sign in with an account that can manage vCenter permissions.
2. Open **Menu → Hosts and Clusters**.
3. Select `Lab-Datacenter`, not an individual ESXi host.
4. Open **Permissions → Add**.
5. Select domain `vsphere.local` and the required user or group.
6. Select **Read-only**.
7. Enable **Propagate to children**, then save.
8. Test the account and confirm it can view both hosts and their inventory.

A datacenter-level permission is inherited by child objects when propagation is enabled. It grants access across that datacenter's inventory, so use it only when that broader scope is intended. Do not assign a Global Permission for this requirement.

The current lab user has a direct Read-only assignment on ESXi01. If moving to a datacenter-level assignment, first add and verify the datacenter permission, then remove the old direct ESXi01 assignment if it is no longer needed. Avoid removing existing permissions for built-in or service accounts.

## Lessons Learned

- A user account and a role are different: the user is an identity, while the role defines allowed operations.
- A permission binds a user or group to a role on an inventory object.
- Permission propagation applies the assignment to child objects.
- Assigning a role at the datacenter can cover multiple hosts in that datacenter, avoiding repeated host-by-host assignments.
- Use the narrowest inventory scope that meets the access requirement.

## Task Status

**Completed — basic Read-only permission assignment and viewing access verified.** The Power On action was not tested. The lab user currently has a direct Read-only assignment on ESXi01; datacenter-level assignment for both hosts is documented as the recommended method but has not been applied or verified in this task.

## Next Task

**Task 10 — Configure Distributed Switching**
