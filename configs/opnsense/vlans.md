# OPNsense VLAN / Interface Setup

| Interface | VLAN tag | Gateway IP (example) | DHCP range | DNS given out |
|---|---|---|---|---|
| MAIN | 10 | 10.0.10.1/24 | .100 – .200 | Pi-hole (10.0.10.5) |
| IOT | 20 | 10.0.20.1/24 | .100 – .200 | Pi-hole (10.0.10.5) |
| GUEST | 30 | 10.0.30.1/24 | .100 – .200 | Pi-hole (10.0.10.5) |

Static IPs (outside the DHCP range):
| Device | IP |
|---|---|
| Pi-hole | 10.0.10.5 |
| Domain Controller | 10.0.10.10 |

## Backing up the config
**System → Configuration → Backups → Download**.
⚠️ Don't commit the backup XML to GitHub. It contains password hashes and your real IPs.
