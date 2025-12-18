# bsrxyr.github.io
RHCSA Quick reference

----01. REPOS, USERS----

*repo dump
dnf list installed > /root/dnf_list.txt

*epel add
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm		


*
users,groups

useradd bob

groupadd bobs

useradd -m -d /shared/bobs_homedir bob		//dodjeljivanje home dira

usermod -g bobs bob							//dodavanje u grupu

sudo usermod -aG wheel bob					//wheel je sudoers na RH


*
ownership

chown bob /shared/foldername

chgrp groupname file.txt	//grupa je owner


*
permissions

	owner group all
r=4
w=2
x=1

chmod 755 filename.py

chmod -R permissions directory		//rekurzivno


*
SSH

/etc/ssh/sshd_config

Match User bob

PasswordAuthentication yes

ssh bob@serverb
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no bob@serverb 	//forsiran pass login


----05. MAIL----

*
DNS

nano /etc/dnsmasq.conf

interfaces=eth0 (ifconfig)

domain=domena.rhcsa

mx-host=domena.rhcsa,mail.domena.rhcsa,50


*
postfix

nano /etc/postfix/main.cf

myhostname = mail.domena.rhcsa

mydomain = domena.rhcsa

myorigin = $mydomain

mydestination =svedefaultno, $mydomain

mynetworks = 172.25.250/24, 127.0.0.0/8

mail_spool_directory = /var/spool/mail

*
na workstationu

nslookup -type=mx domena.rhcsa

nslookup mail.domena.rhcsa

namistit /etc/resolv.conf  (search domena.rhcsa
							nameserver 172.25.250.10)

telnet mail.domena.rhcsa 25

MAIL FROM: student@domena.rhcsa

RCPT TO: mailer@domena.rhcsa

DATA

Tekst novog maila

.

quit

*
na mail serveru provjera
cat /var/spool/mail/mailer
