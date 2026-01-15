# bsrxyr.github.io

Linux sysadmin quick reference 🕮

------01. REPOS, USERS------
*repo dump
dnf list installed > /root/dnf_list.txt

*epel add
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
dnf history info //zadnji instalirani paket

*
USERS,GROUPS
useradd bob
groupadd bobs
useradd -m -d /shared/bobs_homedir bob //dodjeljivanje home dira
usermod -g bobs bob //dodavanje u grupu
sudo usermod -aG wheel bob //wheel je sudoers na RH
cat /etc/group //pokazat grupe
*
OWNERSHIP
chown bob /shared/foldername
chgrp groupname file.txt //grupa je owner

*
PERMISSIONS
owner group all
r=4
w=2
x=1

chmod 755 filename.py
chmod -R permissions directory //rekurzivno

*
SSH
/etc/ssh/sshd_config
Match User bob
PasswordAuthentication yes
ssh bob@serverb
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no bob@serverb //forsiran pass login
ssh-copy-id student@serverb //sa servera kopiranje keya

------02. DISKS------

fdisk (w piše promjene)
lsblk --fs (show filesystem)

mkfs.xfs
mkfs.ext4

mkswap
swapon

UUID=881e856c-37b1-41e3-b009-ad526e46d987 /archive xfs defaults 0 0 //fstab
swap swap defaults 0 0 //za swap

------03. TIME, LOG------
timedatectl set-timezone Europe/Zagreb
vidi lab03

EDITOR=nano crontb -u student -e //nanom otvoren studentov crontab
journalctl --since "17:00" --until "5 minutes ago"

------04. DHCP,DNS------

DHCP
/etc/dnsmasq.conf
user=
group=
interface=eth0 (ifconfig)
dhcp-range=x.x.x.x,x.x.x.y,24h
dhcp-option=3,x.x.x.x adresa routera


DNS
domain needed domain=mojadomena.local

(sve napucat u /etc/hosts file)
(na klijentu za koju domenu je koji name server odgovoran /etc/resolv.conf)

nslookup serverb.domena.local
dig www.algebra.hr


route -n (defaultna ruta)
ip ro (defaultna ruta)

/etc/hostname
/etc/hosts

timedatectl set-ntp false //ugasit ntp

------05. MAIL------
*
DNS
nano /etc/dnsmasq.conf
interfaces=eth0 (ifconfig)
domain=domena.rhcsa
mx-host=domena.rhcsa,mail.domena.rhcsa,50
*
POSTFIX
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
namistit /etc/resolv.conf (search domena.rhcsa
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


------06. WEBSRV------

Apache (httpd)
/etc/httpd/conf.d


ServerName www.apache.local
DocumentRoot /var/www/web_apache_local



Nginx
/etc/nginx/nginx.conf
server {
root /var/www/www_nginx_local/;
server name www.nginx.local;
}


sudo setenforce 0 //obavezno za nginx

curl http://web.apache.local //brzi dokaz websitea 
