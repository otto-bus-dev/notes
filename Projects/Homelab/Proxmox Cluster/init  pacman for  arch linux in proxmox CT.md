---
tags:
  - proxmox
  - arch
---

### **1 - run init commands

```
pacman-key --init
pacman-key --populate archlinux
pacman-key --refresh-keys
pacman -Sy archlinux-keyring
```