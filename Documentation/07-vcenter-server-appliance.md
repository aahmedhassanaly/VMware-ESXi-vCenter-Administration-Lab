# Task 07 — Deploy vCenter Server Appliance

## Objective

Deploy VMware vCenter Server Appliance (VCSA), configure DNS name resolution, and verify that the appliance can resolve its own fully qualified domain name.

## Lab Environment

| Component | Details |
|---|---|
| Physical host | Windows 11 Pro |
| DNS software | Technitium DNS Server |
| DNS server IP | `192.168.1.144` |
| VCSA version | `8.0.3.00400` |
| Build number | `24322831` |
| Hostname | `vcsa01.lab.local` |
| IP address | `192.168.1.200` |
| Network | `192.168.1.0/24` |
| Default gateway | `192.168.1.1` |
| SSO domain | `vsphere.local` |

## 1. Deploy VCSA

VCSA was installed with the hostname `vcsa01.lab.local` and IP address `192.168.1.200`.

The VAMI management interface is available at:

`https://192.168.1.200:5480`

The vSphere Client URL to verify is:

`https://vcsa01.lab.local/ui`

## 2. Configure DNS on the Windows Host

Technitium DNS Server was installed on the physical Windows 11 Pro host. Its local administration interface is:

`http://localhost:5380`

A Primary Zone named `lab.local` was created with this A record:

| Record type | Name | Value |
|---|---|---|
| A | `vcsa01` | `192.168.1.200` |

A reverse zone named `1.168.192.in-addr.arpa` was created with this PTR record:

| Record type | Name | Value |
|---|---|---|
| PTR | `200` | `vcsa01.lab.local` |

The Technitium Windows service is named `DnsService`. It was observed as Running with startup mode set to Automatic.

The Windows 11 Wi-Fi adapter uses DHCP reservation `192.168.1.144` on the router. The Wi-Fi network profile is Public. A Windows Firewall rule was configured to allow DNS over UDP port 53 from `192.168.1.0/24` on the Public profile.

TCP port 53 has not yet been independently verified.

## 3. Verify Forward and Reverse DNS

On the physical Windows 11 host, both commands completed successfully:

```powershell
Resolve-DnsName vcsa01.lab.local -Server 192.168.1.144
Resolve-DnsName 192.168.1.200 -Type PTR -Server 192.168.1.144
```

The results mapped `vcsa01.lab.local` to `192.168.1.200` and the reverse lookup to `vcsa01.lab.local`.

The same forward and reverse lookups succeeded from VCSA:

```bash
nslookup vcsa01.lab.local 192.168.1.144
nslookup 192.168.1.200 192.168.1.144
```

These tests verify the tested DNS queries over the path used by the commands. They do not independently verify DNS over TCP.

## 4. Configure VCSA DNS

DNS settings were changed through VAMI at `https://192.168.1.200:5480`. After the change, `/etc/resolv.conf` showed:

```text
nameserver 127.0.0.1
nameserver 192.168.1.144
search .
```

The file is managed by `systemd-resolved`; it was not edited manually.

## 5. Review Appliance Health

The VAMI Summary page showed:

| Health item | Observed status |
|---|---|
| CPU | Good |
| Memory | Alert — appliance running low on memory |
| Database | Good |
| Storage | Good |
| Swap | Good |
| Single Sign-On | Running for `vsphere.local` |

After installation, the configured VCSA memory was reduced from 12 GB to 4 GB. The Memory Alert was visible after this change. This is an acknowledged lab resource constraint, not proof that every vCenter service is healthy.

## Verification Checklist

- [x] VCSA deployed with version `8.0.3.00400`.
- [x] Hostname and static IP recorded.
- [x] Technitium DNS Primary Zone and A record created.
- [x] Reverse zone and PTR record created.
- [x] Forward and reverse lookups succeeded from Windows 11.
- [x] Forward and reverse lookups succeeded from VCSA.
- [x] VCSA DNS settings changed through VAMI.
- [x] VAMI health status reviewed and low-memory alert recorded.
- [ ] Verify DNS over TCP port 53 separately.
- [ ] Restart Windows and confirm Technitium starts automatically and records remain available.
- [ ] Verify vSphere Client access using the FQDN.
- [ ] Continue monitoring VCSA services under the current 4 GB memory allocation.

## Security and Operations Notes

- Do not edit `/etc/resolv.conf` manually because it is managed by `systemd-resolved`.
- A successful UDP DNS lookup does not prove that TCP port 53 is allowed.
- The DNS service is running automatically, but persistence after a Windows restart still needs a test.
- The low-memory alert must remain visible in the project record until it is resolved or accepted as a lab limitation.

## Task Status

**Deployed; DNS forward and reverse lookups verified.** The VCSA Memory Alert is open, and the remaining verification items are listed above.

## Next Task

**Task 08 — Add Hosts to vCenter and Organize Inventory**
