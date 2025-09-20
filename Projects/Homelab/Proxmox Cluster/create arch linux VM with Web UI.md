---
tags:
  - proxmox
  - arch
---

### **1 - create VM**

click the "Create VM" button 

![[Pasted image 20250920233610.png]]

follow the wizard and select resources depending on your needs and capacity

![[Pasted image 20250917135908.png]]

![[Pasted image 20250917135934.png]]

![[Pasted image 20250917135949.png]]

![[Pasted image 20250917140005.png]]

![[Pasted image 20250917140030.png]]

![[Pasted image 20250917140053.png]]

![[Pasted image 20250917140114.png]]

![[Pasted image 20250917140132.png]]

>click "start after created"  or click on start manually once it's created

### **2 - install arch**

#### **2.1 - launch install**

open the console view of the new VM

![[Pasted image 20250920233952.png]]

during boot you should see

![[Pasted image 20250917140212.png]]

when the boot is over you should see : 

![[Pasted image 20250917140512.png]]


use loadkeys {YOUR_KEYBOARD} to set your keyboard setup (for me fr)

```
loadkeys fr
```

and launch archinstall

```
archinstall
```


![[Pasted image 20250917140627.png]]


#### **2.1 - configure install**

you should arrive to the install config screen 

![[Pasted image 20250917140711.png]]

choose language (i kept it in english)

![[Pasted image 20250917140753.png]]

for locales i changer the keyboard layout to fr and use default english for the rest : ![[Pasted image 20250917140855.png]]
for the mirror i chose france as the region and use default for the rest

![[Pasted image 20250917141026.png]]

for partitionning i use the default value as show here : 

![[Pasted image 20250917141048.png]]
![[Pasted image 20250917141103.png]]
![[Pasted image 20250917141118.png]]
![[Pasted image 20250917141129.png]]
![[Pasted image 20250917141141.png]]
![[Pasted image 20250917141151.png]]
![[Pasted image 20250917141206.png]]

![[Pasted image 20250917141222.png]]

for the swap config i leave it as it is by default

for the bootloader  same thing 

>the default value is grub

fot the hostname i chose the hostname you want

![[Pasted image 20250917141411.png]]

for authentication i on add the root password  

>you can add other users if you want and provide their password as well

![[Pasted image 20250917141501.png]]

![[Pasted image 20250917141537.png]]

![[Pasted image 20250917141556.png]]

![[Pasted image 20250917141616.png]]

![[Pasted image 20250917141632.png]]


for profile  i chose the minimal profile to keep it as small as possible  :
![[Pasted image 20250917141719.png]]

![[Pasted image 20250917141734.png]]

![[Pasted image 20250917141756.png]]

![[Pasted image 20250917141810.png]]

for application i did nothing and left it empty
for the kernel i leave the default value "linux"
![[Pasted image 20250917141910.png]]

for network configuration i use manual ( this is for ethernet)
![[Pasted image 20250917141945.png]]

![[Pasted image 20250917141959.png]]

![[Pasted image 20250917142022.png]]

![[Pasted image 20250917142036.png]]

![[Pasted image 20250917142105.png]]

![[Pasted image 20250917142128.png]]

you can leave additional packages empty we will add them later depending on the use case for the machine

for timezone i chose france : 
![[Pasted image 20250917142244.png]]
![[Pasted image 20250917142303.png]]
![[Pasted image 20250917142317.png]]

i left Automatic time sync to default value 
![[Pasted image 20250917142352.png]]

and then lauchned the install 
![[Pasted image 20250917142418.png]]
and validated the config file 
![[Pasted image 20250917142453.png]]

![[Pasted image 20250917142507.png]]

#### **3 - after install**

when the install is over you should have something like this

![[Pasted image 20250917142809.png]]

you can exit archinstall or directly reboot the system 
after reboot you can login with the root account

![[Pasted image 20250917143010.png]]