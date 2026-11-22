# OPNsense Firewall Rules

OPNsense checks rules **top to bottom** and the first match wins. Anything not allowed is blocked.

An alias called `RFC1918` = `10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16` (all private networks).

## VLAN 10 — Main (trusted)
| # | Action | Source | Destination | Port | Why |
|---|---|---|---|---|---|
| 1 | ✅ Allow | Main net | Any | Any | Trusted devices can reach everything |

## VLAN 20 — IoT (untrusted)
| # | Action | Source | Destination | Port | Why |
|---|---|---|---|---|---|
| 1 | ✅ Allow | IoT net | Pi-hole | 53 (DNS) | DNS only through Pi-hole |
| 2 | ❌ Block | IoT net | Any | 53, 853 | Stop devices using their own DNS (e.g. 8.8.8.8) |
| 3 | ❌ Block | IoT net | This firewall | 80, 443, 22 | No access to the firewall admin page |
| 4 | ❌ Block | IoT net | RFC1918 | Any | No access to Main or Guest |
| 5 | ✅ Allow | IoT net | Any | Any | Internet only |

## VLAN 30 — Guest
| # | Action | Source | Destination | Port | Why |
|---|---|---|---|---|---|
| 1 | ✅ Allow | Guest net | Pi-hole | 53 (DNS) | Filtered DNS |
| 2 | ❌ Block | Guest net | This firewall | 80, 443, 22 | No admin access |
| 3 | ❌ Block | Guest net | RFC1918 | Any | No access to other VLANs |
| 4 | ✅ Allow | Guest net | Any | Any | Internet only |

## WAN
- No inbound port forwards. Everything from the internet is blocked by default.

> Pi-hole is on the Main VLAN, so rule 1 on IoT/Guest must sit **above** the RFC1918 block, or DNS breaks.
