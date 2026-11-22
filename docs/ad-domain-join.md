# Joining Computers to Active Directory

Domain example: `home.lan` · DC: `10.0.10.10`

**Before you start:** the computer's DNS must point to the **DC**, or it won't find the domain.

## Windows (Surface Laptop 7)
1. Settings → System → About → **Domain or workgroup** → Change
2. Choose **Domain**, type `home.lan`, and enter a domain admin login.
3. Restart and log in as `HOME\username`.

Or in PowerShell (as admin):
```powershell
Add-Computer -DomainName "home.lan" -Credential HOME\Administrator -Restart
```

## Ubuntu (server + lab PC)
```bash
# 1. Install the tools
sudo apt install realmd sssd sssd-tools adcli krb5-user packagekit samba-common-bin -y

# 2. Find the domain
realm discover home.lan

# 3. Join
sudo realm join -U Administrator home.lan

# 4. Create home folders automatically on first login
sudo pam-auth-update --enable mkhomedir

# 5. Check it worked
realm list
id administrator@home.lan
```

## Check on the DC
Open **Active Directory Users and Computers → Computers**. All 3 machines should be listed.
