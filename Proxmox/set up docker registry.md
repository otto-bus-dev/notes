
## 1. Create a VM in Proxmox

- Install Arch Linux on the VM ([Arch Wiki Installation Guide](https://wiki.archlinux.org/title/installation_guide)).
- Assign a static IP (e.g., `192.168.1.50`).

---

## 2. Install Docker on Arch Linux VM

bash

```
sudo pacman -Syu docker
sudo systemctl enable --now docker
```

---

## 3. Run the Private Docker Registry Container

bash

```
sudo mkdir -p /opt/registry/data
sudo docker run -d \
  --name registry \
  -p 5000:5000 \
  --restart=always \
  -v /opt/registry/data:/var/lib/registry \
  registry:2
```

---

## 4. Network Configuration

- Make sure port 5000 is open on your VM (`sudo ufw allow 5000/tcp` if you use `ufw`).
- Ensure your Proxmox/host network routes allow access from your client machine.

---

## 5. (Optional but recommended) Configure HTTPS

- For testing, you can use HTTP (see next step).
- For production, generate SSL certificates and configure the registry for HTTPS ([Docker docs](https://docs.docker.com/registry/deploying/)).

---

## 6. Allow Docker to use Insecure Registry (HTTP, for testing)

On your **Arch Linux client machine** (the one you’ll push images from):

1. Edit or create `/etc/docker/daemon.json`:

JSON

```
{
  "insecure-registries": ["192.168.1.50:5000"]
}
```

2. Restart Docker:

bash

```
sudo systemctl restart docker
```

---

## 7. Build, Tag & Push Images

On your client machine, build your image:

bash

```
sudo docker build -t myapp:latest .
```

Tag it for your registry:

bash

```
sudo docker tag myapp:latest 192.168.1.50:5000/myapp:latest
```

Push to your registry:

bash

```
sudo docker push 192.168.1.50:5000/myapp:latest
```

---

## 8. Pull Images from Registry

On any machine configured to use your registry:

bash

```
sudo docker pull 192.168.1.50:5000/myapp:latest
```

---

## 9. Security Recommendations

- For production, use HTTPS and authentication.
- Backup `/opt/registry/data` regularly.
- Restrict access to the registry port if needed (firewall).

---

### Summary of changes for Arch Linux:

- Use `pacman` for Docker installation.
- Docker commands and config paths are the same.
- Arch does not include Docker in base install; install from official repos.



## Basic Authentication Setup

**Why?**  
Authentication restricts who can push/pull images, making your registry private.

### Install `htpasswd` Tool

bash

```
sudo pacman -Syu apache-tools
```

### Create Registry User

bash

```
sudo mkdir -p /opt/registry/auth
sudo htpasswd -Bbn myuser mypassword > /opt/registry/auth/htpasswd
```

### Start Registry with Auth

bash

```
sudo docker run -d \
  --name registry \
  -p 5000:5000 \
  --restart=always \
  -v /opt/registry/data:/var/lib/registry \
  -v /opt/registry/certs:/certs \
  -v /opt/registry/auth:/auth \
  -e REGISTRY_AUTH=htpasswd \
  -e REGISTRY_AUTH_HTPASSWD_REALM="Registry Realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  registry:2
```