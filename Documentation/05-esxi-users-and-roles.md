# Task 05 — Manage ESXi Users and Roles

## Objective

Create a separate local user on ESXi, assign a limited role, and verify the user's access. This task focuses on the difference between authentication (proving identity) and authorization (what the user is allowed to do).

## Lab Environment

| Component | Details |
|---|---|
| Hypervisor | Nested VMware ESXi 8.0 Update 2 |
| ESXi Host Client | `https://192.168.1.41/ui` |
| Host | ESXi01 |
| Test user | `labadmin` |
| Assigned role | `Read-only` |

## 1. Review Existing Users

In **Host → Manage → Security & Users → Users**, the visible local user was:

| User name | Description | Shell access |
|---|---|---|
| `root` | Administrator | `true` |

The user `labadmin` was then created for this lab. Shell access was left disabled, as selected during account creation.

## 2. Review Available Roles

The Roles page showed these built-in roles:

- `Administrator` — full access rights.
- `Read-only` — can view object details but cannot make changes.
- `No access` — used to restrict granted access.
- `No cryptography administrator` — full access without cryptographic operations and Trusted Infrastructure privileges.
- `No Trusted Infrastructure administrator` — full access without Trusted Infrastructure privileges.
- `Trusted Infrastructure administrator` — used to set up and monitor Trusted Infrastructure.
- `Anonymous` and `View` were also listed as system roles that cannot be granted.

For this task, `Read-only` was selected to test limited access.

## 3. Assign the Role

In **Assign users and roles for Host**, the existing entries included:

| User | Role |
|---|---|
| `dcui` | Administrator |
| `root` | Administrator |
| `vpxuser` | Administrator |

The user `labadmin` was added with the `Read-only` role at the host level.

The existing roles for `root`, `dcui`, and `vpxuser` were not changed.

## 4. Verify Access

The following checks were completed:

- [x] `labadmin` could log in to the ESXi Host Client.
- [x] The user could view host information.
- [x] An attempt to change host settings was blocked.
- [x] The host permission list still showed `labadmin` as `Read-only`.
- [x] The existing administrative permissions were checked and left unchanged.

These results confirm that the `Read-only` role allowed viewing but blocked the tested change operation.

## 5. Review Other Security Settings

The **Security & Users** page was also reviewed. These settings were observed but not changed:

### Acceptance Level

The current acceptance level shown was `Partner`.

The acceptance level controls which ESXi software packages (VIBs) are accepted for installation. The setting was left unchanged.

### Authentication

The Authentication page showed:

| Setting | Observed value |
|---|---|
| Active Directory enabled | `No` |
| Smart card authentication enabled | `No` |

These settings were only reviewed. Active Directory and smart card authentication were not configured as part of this task.

## Security Notes

- Use a separate account for routine access instead of using `root` for every task.
- Assign only the permissions required for the user's work.
- Keep at least one working administrative access path before changing permissions.
- Do not lower the ESXi acceptance level without a clear need and an understanding of the security impact.
- Do not enable a new authentication method until its configuration and recovery path are understood and tested.

## Task Status

**Completed.** A local user was created, assigned the `Read-only` role, and tested. The user could view host information but could not perform the tested change. Acceptance Level and Authentication settings were reviewed without modification.

## Next Task

**Task 06 — Deploy and Configure ESXi02**
