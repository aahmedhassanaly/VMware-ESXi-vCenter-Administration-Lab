# Task 07 — Deploy vCenter Server Appliance (VCSA)

## Objective

Deploy VMware vCenter Server Appliance and prepare DNS name resolution for the lab.

## Lab Details

| Component | Verified details |
|---|---|
| Appliance | VMware vCenter Server Appliance |
| Version | 8.0.3.00400 |
| Build | 24322831 |
| Hostname | `vcsa01.lab.local` |
| IP address | `192.168.1.200` |
| Network | `192.168.1.0/24` |
| Default gateway | `192.168.1.1` |
| SSO domain | `vsphere.local` |
| DNS server | Technitium DNS on the physical Windows 11 Pro host |
| DNS server IP | `192.168.1.144` |

## DNS Configuration

Technitium DNS Server was installed on the physical Windows 11 Pro host. The management interface is available locally at `http://localhost:5380`.

The following records were created in the `lab.local` zone:

| Record type | Name | Value |
|---|---|---|
| A | `vcsa01` | `192.168.1.200` |
| PTR | `200` in `1.168.192.in-addr.arpa` | `vcsa01.lab.local` |

The DNS service is named `DnsService`, has startup mode set to Automatic, and was observed in the Running state. The physical Windows 11 host has a DHCP reservation for `192.168.1.144` using its Wi-Fi MAC address.
<img width="1495" height="696" alt="image" src="https://github.com/user-attachments/assets/93ed089d-0692-45fc-aeb9-76b16e5d1394" />

## Verification Performed

Forward and reverse lookups were successful from the physical Windows 11 host:

```powershell
Resolve-DnsName vcsa01.lab.local -Server 192.168.1.144
Resolve-DnsName 192.168.1.200 -Type PTR -Server 192.168.1.144
```

Forward and reverse lookups were also successful from VCSA:

```bash
nslookup vcsa01.lab.local 192.168.1.144
nslookup 192.168.1.200 192.168.1.144
```

The VCSA resolver configuration was changed through VAMI at `https://192.168.1.200:5480`. The resulting `/etc/resolv.conf` showed:

```text
nameserver 127.0.0.1
nameserver 192.168.1.144
search .
```

This file is managed by `systemd-resolved`; it was not edited manually.

## Resource Alert

After installation, the configured VCSA memory was reduced from 12 GB to 4 GB. The VAMI Summary page then showed a Memory Alert stating that the appliance was running low on memory. CPU, Database, Storage, and Swap were shown as Good in the captured screen. Single Sign-On showed the `vsphere.local` domain with status Running.

This alert is documented as an open resource constraint. It does not by itself prove that every vCenter service is healthy. The lab is continuing with this limitation acknowledged.

## Open Follow-up Items

- Verify DNS over TCP port 53 separately; UDP DNS queries have been tested successfully.
- After a Windows restart, verify that Technitium starts automatically and that the DNS records remain available.
- Monitor VCSA service health and memory pressure while continuing the lab.
- Verify access to the vSphere Client using `https://vcsa01.lab.local/ui`.

## Result

VCSA was deployed and its hostname resolved successfully using the Technitium DNS server. Forward and reverse DNS lookups were verified from both the Windows host and VCSA. The appliance currently reports a low-memory alert after its configured memory was reduced to 4 GB.

