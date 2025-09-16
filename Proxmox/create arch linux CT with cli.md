---
folder: proxmox
title: create arch linux CT with cli
---
1 - connect to you proxmox machine (as proxmox sever root)

sh

```
ssh root@{PROXMOX_IP OR PROXMOX_HOSTNAME}
```
  
2 - run the following command 

sh
```
pct create 201 {YOUR_STORAGE}:vztmpl/{ARCH_CT_TEMPLATE} \
  --hostname {YOUR_HOSTNAME} \
  --rootfs local-lvm:{DISK_SIZE} \
  --unprivileged 1
  --cores {NB_CORES} \
  --memory {MEMORY_MB} \
  --net0 name={NETWORK_INTERFACE_NAME},bridge={PROXMOX_NETWORK_BRIDGE},ip=dhcp \
  --ssh-public-keys /path/to/public_key.pub
  --password {ROOT_PASSWORD}
```

exemple : 

```
pct create 201 synology-backup:vztmpl/archlinux-base_20240911-1_amd64.tar.zst \
  --hostname test \
  --rootfs local-lvm:8 \
  --unprivileged 1
  --cores  1 \
  --memory 512 \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp \
  --password test_passwd
```

results :

```
root@otto:~# pct create 201 synology-backup:vztmpl/archlinux-base_20240911-1_amd64.tar.zst \
  --hostname test \
  --rootfs local-lvm:8 \
  --unprivileged 1
  --cores  1 \
  --memory 512 \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp \
  --password test_passwd
  WARNING: You have not turned on protection against thin pools running out of space.
  WARNING: Set activation/thin_pool_autoextend_threshold below 100 to trigger automatic extension of thin pools before they get full.
  Logical volume "vm-201-disk-0" created.
  WARNING: Sum of all thin volume sizes (152.00 GiB) exceeds the size of thin pool pve/data and the amount of free space in volume group (16.00 GiB).
Creating filesystem with 2097152 4k blocks and 524288 inodes
Filesystem UUID: 5e029861-8fca-4273-bfd8-7c72f3aaf823
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632
extracting archive '/mnt/pve/synology-backup/template/cache/archlinux-base_20240911-1_amd64.tar.zst'
Total bytes read: 612372480 (585MiB, 365MiB/s)
Detected container architecture: amd64
Creating SSH host key 'ssh_host_ecdsa_key' - this may take some time ...
done: SHA256:jgQlMPotRbzmgJIelZdV5LH0zlLPsGSoC1mnYDyRLWQ root@test
Creating SSH host key 'ssh_host_rsa_key' - this may take some time ...
done: SHA256:isTJT2+SlBQBL56d3JmOJZmamxaII27GRs1HK8zSLxE root@test
Creating SSH host key 'ssh_host_ed25519_key' - this may take some time ...
done: SHA256:JGx8UydJWU2Vsc2JPsPJMlGihUzBvb2ggwBRkG+Lfco root@test
Creating SSH host key 'ssh_host_dsa_key' - this may take some time ...
done: SHA256:Kb1Ok+lne9wNWL5u0tOX3EZxRtIKpUp9y39b5CzXsvU root@test
root@otto:~# 
```

and your new server is available in the proxmox node : 
![[Pasted image 20250916124206.png]]
