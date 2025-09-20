---
tags:
  - proxmox
  - mail
---

### **1 - packages to install on proxmox

connect to your proxmox machine as root (ssh or directly on the machine)

```
apt update
apt install postfix libsasl2-modules
```

> N.B : add sudo if sudoer instead of root for every command in this doc


### **2 - configure postfix for gmail SMTP**

on the proxmox machine, edit the postfix config file /etc/postfix/main.cf

```
myhostname={YOUR_HOSTNAME}
smtpd_banner = $myhostname ESMTP $mail_name (Debian/GNU)
biff = no
append_dot_mydomain = no
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
mydestination = $myhostname, localhost.$mydomain, localhost
relayhost = [smtp.gmail.com]:587
mynetworks = 127.0.0.0/8
inet_interfaces = all
recipient_delimiter = +
compatibility_level = 3.6
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
```


### **3 - setup gmail username and password**

##### **3.1 - generate gmail app password**

go to : https://myaccount.google.com/apppasswords

name your app

![[Pasted image 20250920225335.png]]

click create and copy the generated password 

![[Pasted image 20250920225455.png]]

>Password and app removed

##### **3.2 -  set gmail authent**

update file /etc/postfix/sasl_passwd : 

```
[smtp.gmail.com]:587    yourgmailaddress@gmail.com:{PASSWORD_GENERATED}
```

and activate your credentials : 

```
postmap /etc/postfix/sasl_passwd
chown root:root /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
chmod 600 /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
```

**3.3 -  start service**

check the service file, it should look like this : 

```
[Unit]
Description=Postfix Mail Transport Agent
Documentation=man:postfix(1)
Conflicts=sendmail.service exim4.service
ConditionPathExists=/etc/postfix/main.cf

[Service]
Type=forking
ExecStart=/usr/sbin/postfix start
ExecStop=/usr/sbin/postfix stop
ExecReload=/usr/sbin/postfix reload

[Install]
WantedBy=multi-user.target
```

then start the service and check the status 

 ```
systemctl start postfix
systemctl status postfix
 ```

if you have an error when starting the service check the logs with : 

```
journalctl -xeu postfix.service
```

if restart fails with "postfix already running"

![[Pasted image 20250920232346.png]]

the try this : 

```
rm -rf /var/spool/postfix/private/*
rm -rf /var/spool/postfix/public/*
rm -f /var/lib/postfix/master.lock
rm -f /var/spool/postfix/pid/*
systemctl enable postfix
systemctl start postfix
```

### **4 - test mail**

```
echo "Test body" | mail -s "Test subject" recipient@example.com
```

> no configuration is required on the proxmox web ui
