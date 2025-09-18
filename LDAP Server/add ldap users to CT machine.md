on the proxmox machine

run : 
```
pct push {id_container} /var/lib/vz/snippets/setup-ldap.sh /root/setup-ldap.sh
pct exec {id_container} -- bash /root/setup-ldap.sh
pct exec {id_container} -- cat /etc/nslcd.conf
```
