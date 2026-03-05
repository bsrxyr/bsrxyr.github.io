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
<br>
------06. WEBSRV------
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
------07. LVM,STRATIS------
<pre style='color:#cfcfc2;background-color:#232629;'>
****LVM****

lvs
vgs

lvextend -l +100%FREE
pa  	resize2fs
ili		xfsgrowfs

od diskova physical volume pa volume grupe pa lvm particije


****Stratis****

dnf install stratisd stratis-cli

je Redhat verzija LVM-a,
poolovi su volume grupe

stratis pool create pool1 /dec/vdb /dev/vdc
stratis pool list
stratis blockdev list

stratis fs create --size 10GiB pool1 nameodpoola</pre>

<br>
------08. NFS------
<pre style='color:#cfcfc2;background-color:#232629;'>
NA SERVERA

-dižemo NFS server, svugdi je instaliran

systemctl status nfs-server.service
systemctl enable --now nfs-server.service

/etc/nfs.conf		(ništa pametno)
/etc/exports		(prazan file, pogledaj man example, unutra idu pravila u 2 stupca kako slijedi)

/data/nfs/share1	172.25.250.0/24(rw,no_root_squash)
/data/nfs/share2	172.25.250.11(rw)
/data/nfs/share3	172.25.250.9(rw)
/data/nfs/share4	*(ro)


systemctl reload nfs-server.service

sudo exportfs -rav


NA SERVERB
						ubit firewall
sudo mount -t nfs 172.25.250.10:/data/nfs/share1 /mnt/share1
df -hPT					pokazuje mountane stvari



AUTOFS dio priče

dnf install autofs
dnf install nfs-utils

man automount		vidi example
					knjiga redhat2, poglavlje 9


na klijentu	/etc/auto.master			includa neki file (ovaj doli)
/etc/auto.master.d

pa u taj neki file ide:
/home/student/Desktop	/etc/autofs_desktop.direct

pa u autofs_desktop.direct
/home/student/Desktop/Dijeljenjo	-rw,sync	172.25.250.10:/data/nfs/share1

systemct restart autofs</pre>
<br>
------09. BAZE------
<pre style='color:#cfcfc2;background-color:#232629;'>
dnf install mysql-server
systemctl enable --now mysqld
mysql -uroot -p

show databases;
create database mojabaza;
use mojabaza;
show tables;
create table users (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(50), surname VARCHAR(50));
show tables;
show columns in users;
insert into users (name, surname) values ('Alice', 'Admin'), ('Bob', 'RHCSA');
select name from users;
select * from users;
select name from users where id = 1;

create user 'local_db_admin'@'localhost' identified by 'Password123';
grant all privileges on mojabaza.* to 'local_db_admin'@'localhost';

ili
grant select, insert on mojabaza.users to 'local_db_admin'@'localhost';

ili
grant all pivileges on *.* to 'local_db_admin'@'%';


mysqldump -uroot -p test &gt;&gt; test_db.sql

------------------------------------------

dnf install postgresql-server
sudo /usr/bin/postgresql-setup --initdb
sudo su - postgres
psql

create database rhcsa_db;
\l
\c rhcsa_db;
pg_dump rhcsa_db &gt;&gt; rhcsa_db.sql


----------------------------------------

https://www.tecmint.com/install-phpmyadmin-rhel-centos-fedora-linux/

php imagix paket ne postoji za vježbe, wtf (NE TREBA)</pre>

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
