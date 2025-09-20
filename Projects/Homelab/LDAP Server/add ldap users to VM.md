on the proxmox machine

run : 

```
scp /var/lib/vz/snippets/setup-ldap.sh  root@obsidian:/root/setup-ldap.sh
ssh root@{HOSTNAME_OR_IP_VM}
```

on vm with ssh :

```
./setup-ldap.sh 
```

try to connect with your account

```
ssh {LDAP_ACCOUNT}@{HOSTNAME_OR_IP_VM}
```

if this works you should be able to connect

![[Pasted image 20250917203640.png]]