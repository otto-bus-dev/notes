1 - generate ssh key

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

follow the instructions  to generate the key. you should have a result like that
![[Pasted image 20250916230234.png]]
N.B: key generated for test and removed

two files should have been created : 
![[Pasted image 20250916230402.png]]

Alternatively you can copy the keys (public an private and public)
you need to check the permisson and limit it :

```
chmod 600 ~/.ssh/{SSH_KEY}
chmod 600 ~/.ssh/{SSH_KEY}.pub
```


2 - start the ssh-agent on the machine 

```
eval "$(ssh-agent -s)"
```

3 - add generated key to agent 

```
ssh-add ~/.ssh/test
```

4 - automate the process on login

```
#
# ~/.bash_profile
#
# start ssh-agent if not running
if ! pgrep -u "$USER" ssh-agent > /dev/null; then
    eval "$(ssh-agent -s)"
fi
ssh-add -l >/dev/null 2>&1 || ssh-add ~/.ssh/test

[[ -f ~/.bashrc ]] && . ~/.bashrc
```
