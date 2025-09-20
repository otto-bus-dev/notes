1 - **Generate a key without passphrase**

```
ssh-keygen -t ed25519 -f ~/.ssh/{SSH_KEY_NAME} -N ""
```

follow the instructions  to generate the key. you should have a result like that
![[Pasted image 20250918174239.png]]

N.B: key generated for test and removed

two files should have been created : 
![[Pasted image 20250918174327.png]]

2 - read the public key 


```
cat ~/.ssh/{SSH_KEY}.pub
```

![[Pasted image 20250918174456.png]]


3 - add your key to the repo

in https://github.com go to "Settings"
![[Pasted image 20250918174709.png]]

then in settings go to "SSH and GPG keys"
![[Pasted image 20250918174752.png]]

then click on the button new ssh key
![[Pasted image 20250918174809.png]]

Name your key and add the public key copied from your repo machine
![[Pasted image 20250918174927.png]]


3 - add ~/.ssh/config file

create the file with the following content 

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/{SSH_KEY_NAME}
```

4 - test config : 

```
cd {REPO_PATH}
git pull
```
if your repo is already up to date  you should have somlething like that 
![[Pasted image 20250918175625.png]]