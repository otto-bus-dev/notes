---
folder: proxmox
title: create arch linux CT with Web UI
---
1 - connect to your proxmox web ui (https://proxmox.ludovic-delaunay.fr)
![[Pasted image 20250916104714.png]]
2 - click on "Create CT"
![[Pasted image 20250916104857.png]]
2.1 - fill "General" tab
![[Pasted image 20250916115205.png]]
- Node : should be preselected, you can change it to another node if you want
- CT ID : should be pre filled but you can change it as well (check for existing id)
- Hostname : {YOUR_HOSTNAME}
- Unprivileged : true
- Nesting : true
- Ressource pool : ""
- Password : {YOUR_PASSWORD}
- Confirm password : {YOUR_PASSWORD}
- SSH public key(s) : {YOUR_SSH}

2.2 - fill "Template" tab
![[Pasted image 20250916115316.png]]
- Storage : {YOUR_STORAGE_CONTAINING_CT_TEMPLATE}
- Template : {YOUR_ARCH_LINUX_CT_TEMPLATE}

2.3 - fill "Disks" tab
![[Pasted image 20250916115736.png]]
- Storage : select the storage where your container will be created
- Disk size : select the disk size you need
N.B.: if you need to add another disk you can do it here

2.4 - fill "CPU" tab
![[Pasted image 20250916120041.png]]
- Cores : select the number of core you need

2.5 - fill "Memory" tab
![[Pasted image 20250916120214.png]]
- Memory (Mib): select the memory you need
- Swap (Mib): select the swap size you need

2.6 - fill "Network" tab
![[Pasted image 20250916120417.png]]
- Name : name of the network interface
- MAC address : auto
- Bridge : select the proxmox network bridge you want
- VLAN Tag : No VLAN
- Firewall : True (to filter request to server)
- IPv4 : DHCP (no static ip)
- IPv6 : DHCP (no static ip)

2.7 - fill "DNS" tab
![[Pasted image 20250916120858.png]]
- DNS domain : use host settings
- DNS servers : use host settings

2.8 - fill "Confirm" tab
![[Pasted image 20250916121020.png]]
- check "Start after created"
- and click "Finish" to create the proxmox Container

2.9 - CT creation output 
![[Pasted image 20250916121330.png]]
if everything is ok the last line should be "TASK OK"
- close the window and check that the CT is available and started in you datacenter node : 
![[Pasted image 20250916121512.png]]

