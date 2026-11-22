# Setup — Step by Step

Subnets and IPs are examples. Change them to match your network.

## 1. OPNsense firewall (VM)
1. Create a VM with 2 network adapters: **WAN** (to the ISP modem) and **LAN** (to the switch, as a VLAN trunk).
2. Install OPNsense and finish the setup wizard.
3. **Interfaces → Other Types → VLAN**: create VLAN 10, 20 and 30 on the LAN adapter.
4. Assign each VLAN as an interface, give it a gateway IP (e.g. `10.0.10.1/24`) and turn on DHCP.
5. Set the DHCP DNS server to **Pi-hole** for every VLAN.
6. Add the firewall rules from [`firewall-rules.md`](firewall-rules.md).

## 2. Switch / Access Point
- Set the port going to OPNsense as a **trunk** (tagged VLAN 10, 20, 30).
- Put each Wi-Fi network (SSID) on its own VLAN: Main → 10, IoT → 20, Guest → 30.

## 3. Domain Controller (Windows Server)
1. Give it a static IP on VLAN 10 (e.g. `10.0.10.10`).
2. Install **AD DS** and promote it to a new forest (e.g. `home.lan`).
3. DNS on the DC: forward outside lookups to **Pi-hole**, so domain names resolve on the DC and everything else gets filtered.
4. Create user accounts, OUs and basic Group Policies (password policy, screen lock, Windows Update).

## 4. Pi-hole (Raspberry Pi 3)
- Static IP on VLAN 10 (e.g. `10.0.10.5`). See [`configs/pihole/`](../configs/pihole/).
- Conditional forwarding: send `home.lan` lookups to the DC.

## 5. Join the computers to AD
See [`ad-domain-join.md`](ad-domain-join.md).

## 6. Test it
- From an IoT device: try to ping the DC → **should fail**
- From a guest device: try to open the OPNsense web page → **should fail**
- From the Surface: log in with a domain account → **should work**
- Visit a known ad domain → **blocked by Pi-hole**
