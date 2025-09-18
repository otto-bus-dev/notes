
---

## **1. Download/Create Arch Linux LXC Template**

**A. Download Prebuilt Template (if available):**

If your Proxmox storage has an Arch Linux template, you’ll see it in the GUI under  
`local → CT Templates → Templates`.

If not, you can manually download one:

bash

```
# Download Arch Linux LXC template to Proxmox
wget https://mirrors.lxc.org/images/archlinux/current/amd64/default/archlinux-default-20240405_amd64.tar.xz -O /var/lib/vz/template/cache/archlinux-default-latest.tar.xz
```

_(Update the URL to the latest available from [LXC images](https://images.linuxcontainers.org/))_

---

## **2. Create a New Arch Linux Container**

Use the Proxmox GUI or CLI:

bash

```
pct create 201 local:vztmpl/archlinux-default-latest.tar.xz \
  --hostname archldap \
  --rootfs local-lvm:8 \
  --cores 2 \
  --memory 2048 \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp \
  --password yourrootpassword
```

- Adjust `201` and settings as needed.

---

## **3. Start and Enter the Container**

bash

```
pct start 201
pct enter 201
```

---

## **4. Set Up LDAP Client Inside the Container**

Inside the container, run (as root):

bash

```
# Update Arch Linux
pacman -Sy --noconfirm

# Install LDAP and NSS/PAM modules
pacman -S --noconfirm nss-pam-ldapd openldap

# Enable and start nslcd
systemctl enable nslcd
systemctl start nslcd
```

---

## **5. Configure LDAP Client**

Edit `/etc/nslcd.conf`:

conf

```
uid nslcd
gid nslcd

uri ldap://192.168.86.31/
base dc=otto-bus,dc=dev

binddn cn=admin,dc=otto-bus,dc=dev
bindpw Blibmf35@
```

_(Update the URI, base DN, bind DN, and password for your LDAP server)_

Set permissions:

bash

```
chmod 600 /etc/nslcd.conf
```

---

Edit `/etc/nsswitch.conf` to use LDAP:

conf

```
passwd:     files ldap
group:      files ldap
shadow:     files ldap
```

_(Just ensure `ldap` is present for those lines.)_

---

## **6. Configure PAM for LDAP**

Edit `/etc/pam.d/system-auth`:

conf

```
auth      sufficient pam_ldap.so
auth      required   pam_unix.so use_first_pass

account   sufficient pam_ldap.so
account   required   pam_unix.so

password  sufficient pam_ldap.so
password  required   pam_unix.so use_first_pass

session   optional   pam_ldap.so
session   required   pam_unix.so
session   required   pam_mkhomedir.so skel=/etc/skel/ umask=0022
```

And `/etc/pam.d/sshd`:

conf

```
auth       include      system-auth
account    include      system-auth
password   include      system-auth
session    include      system-auth
```

---

## **7. Optional: Set Up SSH Access for LDAP Users**

Make sure `openssh` is installed (usually is) and that `/etc/pam.d/sshd` is correctly configured (see above).

Restart SSHD if you changed any configs:

bash

```
systemctl restart sshd
```

---

## **8. Clean Up & Prepare for Template**

Remove any temporary files, logs, or custom user accounts you don’t want in the template.  
You may wish to:

bash

```
pacman -Scc --noconfirm
rm -rf /tmp/*
history -c
```

---

## **9. Stop the Container**

bash

```
exit   # Leave the container shell
pct stop 201
```

---

## **10. Convert the Container to a Template**

bash

```
pct template 201
```

Now you have an **Arch Linux LDAP-enabled CT template**!

---

## **11. Use the Template for New Containers**

When you create new containers from this template, they will **automatically have LDAP authentication enabled**, ready for SSH and console login with LDAP users.

bash

```
pct create 202 local:vztmpl/archlinux-default-latest.tar.xz \
  --hostname mynewct \
  --rootfs local-lvm:8 \
  --cores 2 --memory 2048 --net0 name=eth0,bridge=vmbr0,ip=dhcp \
  --password yourrootpassword \
  --hookscript local:snippets/ct-ldap-hook.sh
```

_(Assign a hookscript if you want further automation after creation.)_

---

## **Summary Table**

|Step|Action|Command/Location|
|---|---|---|
|1|Download Arch template|`/var/lib/vz/template/cache/archlinux-default-latest.tar.xz`|
|2|Create CT|`pct create ...`|
|3|Enter CT|`pct enter <CTID>`|
|4|Install LDAP client|`pacman -S ...`|
|5|Configure LDAP|`/etc/nslcd.conf`, `/etc/nsswitch.conf`|
|6|Configure PAM for LDAP|`/etc/pam.d/system-auth`, `/etc/pam.d/sshd`|
|7|Set up SSH for LDAP users|`/etc/pam.d/sshd`, `systemctl restart sshd`|
|8|Clean up CT|`pacman -Scc`, `rm -rf /tmp/*`|
|9|Stop CT|`pct stop <CTID>`|
|10|Convert to template|`pct template <CTID>`|
|11|Create new LDAP-ready CT|`pct create ...`|

---

**Let me know if you need a ready-to-run script for the LDAP setup inside Arch, or a sample hookscript for further automation!**


to update password : **ldappasswd -x -D "cn=admin,dc=otto-bus,dc=dev" -W -s '{YOUR_PASSWORD}' "uid=otto,dc=otto-bus,dc=dev"**