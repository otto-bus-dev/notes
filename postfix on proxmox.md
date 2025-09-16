if restart fails with "postfix already running"

rm -rf /var/spool/postfix/private/*
rm -rf /var/spool/postfix/public/*
rm -f /var/lib/postfix/master.lock
rm -f /var/spool/postfix/pid/*
systemctl start postfix

what to install on proxmox 

- postfix
- libsasl2-modules


you also to generate an app pasword in gmail
https://myaccount.google.com/apppasswords


then update file : 
sshfs otto@192.168.86.250:/ mount/proxmox/
nvim mount/proxmox/lib/systemd/system/postfix.service 


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

