1 - create VM
- [[create arch linux VM with Web UI]]

3 - connect to the VM 
![[Pasted image 20250917143100.png]]


add your ssh public key to the file /root/.ssh/authorized_keys

```
pacman -Sy vi
vi /root/.ssh/authorized_keys
```


4 - start ssh
```
pacman -Sy openssh
systemctl status sshd
systemctl enable sshd
systemctl start sshd
systemctl status sshd
```

you should have the following result

![[Pasted image 20250916163619.png]]

5 - check firewall 

7 - add other user : 
- [[add ldap users to VM]]
- add local user

8 - [[add user to sudoer]]

9 - iystall pacman packages

```
sudo pacman -Syu
sudo pacman -S git nodejs npm xorg-server-xvfb base-devel
```

10 - [[install yay on arch linux]]

11 - install hyprland-git

```
yay -S hyprland-git
```
N.B: i would i love to avoid this but it is needed to run obsidian "html server" plugin

11 - install obsidian

```
yay -S obsidian
```

12 - [[add ssh key to server on arch machine to access repo]]

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

if repo already exists :

```
git clone git@github.com:YOUR_GITHUB_USERNAME/YOUR_REPO_NAME.git
```

14 - add cron job to pull changes

on the obsidian server 

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
*/15 * * * * cd {YOUR_REPO_PATH} && git pull
```

then save and you should have the following result : 
![[Pasted image 20250916234913.png]]

15 - start obsidian and open the folder with the git repo 

![[Pasted image 20250917233913.png]]

![[Pasted image 20250917233958.png]]

![[Pasted image 20250917234022.png]]

check that the html server is up :

```
ss -tuln
```

you should see a line with ":8080" with a state "LISTEN"

![[Pasted image 20250917234307.png]]