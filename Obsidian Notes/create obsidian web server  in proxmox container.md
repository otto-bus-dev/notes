1 - create CT
- [[create arch linux CT with Web UI]]
- [[create arch linux CT with cli]]

CLI exemple : 

```
pct create 201 synology-backup:vztmpl/archlinux-base_20240911-1_amd64.tar.zst \
  --hostname obsidian \
  --unprivileged 1
  --rootfs local-lvm:8 \
  --cores  1 \
  --memory 512 \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp \
  --ssh-public-keys /path/to/public_key.pub
  --password {ROOT_PASSWORD}
```

2 - [[start proxmox container]]

3 - connect to the container 
![[Pasted image 20250916163223.png]]

4 - start ssh
```
systemctl status sshd
systemctl enable sshd
systemctl start sshd
systemctl status sshd
```

you should have the following result

![[Pasted image 20250916163619.png]]

5 - check firewall 

6 - [[update pacman in new arch linux machine]]

7 - add other user : 
- add ldap
- add local user

8 - [[add user to sudoer]]

9 - install pacman packages

```
sudo pacman -Syu
sudo pacman -S git nodejs npm xorg-server-xvfb base-devel
```

10 - [[install yay on arch linux]]


11 - install obsidian

```
yay -S obsidian
```

12 - [[add ssh key to user on arch machine]]

13 - create a repo for obsidian notes

```
git init
git remote add origin git@github.com:YOUR_GITHUB_USERNAME/YOUR_REPO_NAME.git
echo '.obsidian/workspace
.obsidian/cache
.DS_Store
Thumbs.db
*.tmp
' > .gitignore
git add .gitignore
git commit -m "Add .gitignore"
git add .
git commit -m "Initial commit"
git branch -M main
git push -u origin main
```

14 - add cron job to pull changes


bash

```
nano ~/obsidian-pull.sh
```

Paste this content:

bash

```
#!/bin/bash
cd ~/ObsidianVault || exit 1
git fetch origin
git reset --hard origin/main
```

- This script fetches changes and resets the local branch to match the remote, discarding any local edits.

Make it executable:

bash

```
chmod +x ~/obsidian-pull.sh
```

you can test your script by changing the repo from github  and running this script : 

```
~/obsidian-pull.sh
```

you should have something like this
![[Pasted image 20250916233944.png]]
we can see that the readme file has been updated

14.1 - install cron

```
yay -Sy cronie
```

14.2 - add cron task 
```
crontab -e
```

set your task to execute every 15 minutes
```
*/15 * * * * /home/{YOUR_USER_NAME}/obsidian-pull.sh
```

then save and you should have the following result : 
![[Pasted image 20250916234913.png]]

15 - start obsidian
