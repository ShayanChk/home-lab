# Pi-hole Setup (Raspberry Pi 3 Model B)

## Install
```bash
curl -sSL https://install.pi-hole.net | bash
```
Give it a static IP (e.g. `10.0.10.5`) during setup.

## Settings I use
- **Upstream DNS:** Cloudflare / Quad9 (or the DC forwards to Pi-hole, see setup.md)
- **Conditional forwarding:** `home.lan` → DC `10.0.10.10`
- **Blocklists:** see [`adlists.txt`](adlists.txt)

## Proof it works
- Dashboard → **% of queries blocked**
- Query Log → see which IoT devices "phone home" the most
