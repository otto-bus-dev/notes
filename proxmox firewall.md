# Guide: Configuring Firewall for Proxmox Reverse Proxy

## 1. **Understand Your Setup**

- **Proxmox VE:** Manages your virtual machines/containers.
- **Reverse Proxy (e.g., Nginx, Traefik, Apache):** Typically runs in a VM or container on Proxmox, forwarding traffic to internal services.

## 2. **Determine Required Ports**

- **Proxmox Web UI:** TCP 8006
- **SSH:** TCP 22 (if you need remote SSH access)
- **Reverse Proxy HTTP/HTTPS:** TCP 80/443
- **Other services:** (e.g., VNC, custom ports) as needed

## 3. **Enable Proxmox Firewall**

- Go to **Datacenter** > **Firewall** in the Proxmox UI.
- Enable the firewall globally, on the host node, and on the VM/container running the reverse proxy.

## 4. **Best Practices**

- **Default Policy:** Set to **DROP** for Input and Output unless otherwise required.
- **Allow Only What Is Needed:** Permit only trusted IPs/networks for management ports (e.g., 8006, 22).
- **Limit Reverse Proxy Exposure:** Only allow ports 80/443 from the internet to your reverse proxy VM/container.

## 5. **Example Firewall Rules**

### **Datacenter Level (Global Rules)**
```text
# Allow SSH from trusted IP
IN ACCEPT -source <your-trusted-ip> -dest port 22

# Allow Proxmox Web UI from trusted IP
IN ACCEPT -source <your-trusted-ip> -dest port 8006
```

### **Node Level (Proxmox Host)**
```text
# Drop all inbound traffic except specific ports from trusted IPs
IN DROP
IN ACCEPT -source <your-trusted-ip> -dest port 22
IN ACCEPT -source <your-trusted-ip> -dest port 8006
```

### **VM/Container Level (Reverse Proxy)**
```text
# Allow HTTP/HTTPS from anywhere
IN ACCEPT -dest port 80
IN ACCEPT -dest port 443

# Optionally, restrict management access (e.g., SSH) to trusted IPs
IN ACCEPT -source <your-trusted-ip> -dest port 22

# Drop all other inbound traffic
IN DROP
```

## 6. **Apply and Test**

- Apply rules in the Proxmox UI.
- Test from external and internal networks:
  - Confirm only allowed services are reachable.
  - Use tools like `nmap` or online port scanners.

## 7. **Advanced Tips**

- **Geo-blocking:** Use reverse proxy features to restrict access by country.
- **Rate Limiting:** Mitigate brute-force attacks.
- **Fail2ban:** Consider installing for SSH and web services.
- **Logging:** Enable firewall logs for auditing.

## 8. **Troubleshooting**

- If locked out, use Proxmox console or direct access to reset firewall.
- Always have an “emergency access” plan (e.g., console, physical access).

---

**References:**
- [Proxmox Firewall Documentation](https://pve.proxmox.com/wiki/Firewall)
- [Nginx Reverse Proxy Guide](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)
- [Securing Proxmox](https://pve.proxmox.com/wiki/Securing_Proxmox_VE)
