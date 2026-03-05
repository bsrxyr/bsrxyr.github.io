<div># bsrxyr.github.io</div>
<p><strong>Linux sysadmin quick reference &#128366;</strong></p>
<br>
bcerav i
https://student.algebra.hr
<br>
X.X.Va62yvpJ}'x#
<br>
bseravi c
https://rha.ole.redhat.com/rha/app/
<br>
%ZtrF6Ykh]X=hQV
<br>
<br>
ISHOD 5		EMAIL					5/7
<br>
ISHOD 6		WEBSERVER				3/7
<br>
ISHOD 7	L07,08 LVM,STRATIS,NFS		3/7
<br>
ISHOD 8	L09,L10	DBs, APPS			3/7
<br>
ISHOD 9	L11,L12	SELINUX,FW,PROCESS,REGEX	8/22
<br>
<br>
workstation 172.25.250.9
<br>
servera 172.25.250.10
<br>
serverb 172.25.250.11
<br>
<br>
root:redhat
<br>
student:student
<br>
<br>
sudo systemctl start cockpit.socket
<br>
https://server-ip:9090
<br>
<br> 
dnf install cockpit-storaged
<br>
dnf install stratisd stratis-cli
systemctl start stratisd
<br>
<br>
------05. MAIL------
<br>
<pre style='color:#cfcfc2;background-color:#232629;'>

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
cat /var/spool/mail/mailer</pre>


*
<br>
DNS
<br>
nano /etc/dnsmasq.conf
<br>
interfaces=eth0 (ifconfig)
<br>
domain=domena.rhcsa
<br>
mx-host=domena.rhcsa,mail.domena.rhcsa,50
<br>
<br>
*
<br>
POSTFIX
<br>
sudo dnf install postfix -y
<br>
nano /etc/postfix/main.cf
<br>
myhostname = mail.domena.rhcsa
<br>
mydomain = domena.rhcsa
<br>
myorigin = $mydomain
<br>
mydestination =svedefaultno, $mydomain
<br>
mynetworks = 172.25.250/24, 127.0.0.0/8
<br>
mail_spool_directory = /var/spool/mail
<br>
<br>
*
<br>
na workstationu
<br>
nslookup -type=mx domena.rhcsa
<br>
nslookup mail.domena.rhcsa
<br>
namistit /etc/resolv.conf (search domena.rhcsa
<br>
nameserver 172.25.250.10)
<br>
<br>
telnet mail.domena.rhcsa 25
<br>
MAIL FROM: student@domena.rhcsa
<br>
RCPT TO: mailer@domena.rhcsa
<br>
DATA
<br>
Tekst novog maila
<br>
.
<br>
quit
<br>
<br>
*
<br>
na mail serveru provjera
<br>
cat /var/spool/mail/mailer
<br>
<br>
<br>

------06. WEBSRV------
<br>
<pre style='color:#cfcfc2;background-color:#232629;'>
sudo dnf install httpd -y


# Create Document Root Directories

sudo mkdir -p /var/www/site1/public_html
sudo mkdir -p /var/www/site2/public_html

# Set ownership
sudo chown -R apache:apache /var/www/site1
sudo chown -R apache:apache /var/www/site2

# Set permissions
sudo chmod -R 755 /var/www/site1
sudo chmod -R 755 /var/www/site2


# Create Test Index Pages
echo &quot;&lt;h1&gt;Site 1 is working&lt;/h1&gt;&quot; | sudo tee /var/www/site1/public_html/index.html
echo &quot;&lt;h1&gt;Site 2 is working&lt;/h1&gt;&quot; | sudo tee /var/www/site2/public_html/index.html


# Create Virtual Host Config Files
Create /etc/httpd/conf.d/site1.conf:

apache&lt;VirtualHost *:80&gt;
    ServerName   site1.example.com
    ServerAlias  www.site1.example.com
    DocumentRoot /var/www/site1/public_html

    ErrorLog  /var/log/httpd/site1_error.log
    CustomLog /var/log/httpd/site1_access.log combined

    &lt;Directory /var/www/site1/public_html&gt;
        AllowOverride All
        Require all granted
    &lt;/Directory&gt;
&lt;/VirtualHost&gt;



# Create /etc/httpd/conf.d/site2.conf:

apache&lt;VirtualHost *:80&gt;
    ServerName   site2.example.com
    ServerAlias  www.site2.example.com
    DocumentRoot /var/www/site2/public_html

    ErrorLog  /var/log/httpd/site2_error.log
    CustomLog /var/log/httpd/site2_access.log combined

    &lt;Directory /var/www/site2/public_html&gt;
        AllowOverride All
        Require all granted
    &lt;/Directory&gt;
&lt;/VirtualHost&gt;



# Reload Apache
sudo systemctl reload httpd
</pre>

<br>
<br>


Nginx
<br>
/etc/nginx/nginx.conf
<br>
server {
<br>
root /var/www/www_nginx_local/;
<br>
server name www.nginx.local;
<br>
}
<br>
<br>

sudo setenforce 0 //obavezno za nginx
<br>

curl http://web.apache.local //brzi dokaz websitea 
<br>
<br>
<br>
<br>




-------------------------------------------------------------------------
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
