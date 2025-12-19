<html>
<body style="background-color:darkgrey;">

<div># bsrxyr.github.io</div>
<p><strong>Linux sysadmin quick reference &#128366;</strong></p>
<br>
bceravi
https://student.algebra.hr
<br>
X.X.Va62yvpJ}'x#
<br>
bseravic
https://rha.ole.redhat.com/rha/app/
<br>
%ZtrF6Ykh]X=hQV
<br>

<br>
<strong>------01. REPOS, USERS------</strong>
<br>
*repo dump
<br>
dnf list installed > /root/dnf_list.txt
<br>
<br>
*epel add
<br>
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm		
<br>
dnf history info  //zadnji instalirani paket
<br>

*<br>
USERS,GROUPS
<br>
useradd bob
<br>
groupadd bobs
<br>
useradd -m -d /shared/bobs_homedir bob		//dodjeljivanje home dira
<br>
usermod -g bobs bob							//dodavanje u grupu
<br>
sudo usermod -aG wheel bob					//wheel je sudoers na RH
<br>

*<br>
OWNERSHIP
<br>
chown bob /shared/foldername
<br>
chgrp groupname file.txt	//grupa je owner
<br>

*<br>
PERMISSIONS
<br>
owner group all
<br>
r=4<br>
w=2<br>
x=1<br>

chmod 755 filename.py
<br>
chmod -R permissions directory		//rekurzivno
<br>

*<br>
SSH
<br>
/etc/ssh/sshd_config
<br>
Match User bob
<br>
PasswordAuthentication yes
<br>
ssh bob@serverb
<br>
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no bob@serverb 	//forsiran pass login
<br>
<br>

<strong>------02. DISKS------</strong>
<br>
<br>
fdisk (w piše promjene)
<br>
lsblk --fs (show filesystem)
<br>
<br>
mkfs.xfs
<br>
mkfs.ext4
<br>
<br>
mkswap
<br>
swapon

<br>
<strong>------04. DHCP,DNS------</strong>
<br>
<br>
DHCP
<br>
/etc/dnsmasq.conf 
<br>
user=
<br>
group=
<br>
interface=eth0 (ifconfig)
<br>
dhcp-range=x.x.x.x,x.x.x.y,24h
<br>
dhcp-option=3,x.x.x.x			adresa routera
<br>
<br>
<br>DNS
<br>
domain needed
domain=mojadomena.local
<br>

(sve napucat u /etc/hosts file)
<br>
(na klijentu za koju domenu je koji name server odgovoran /etc/resolv.conf)
<br>
<br>
nslookup serverb.domena.local
<br>
dig www.algebra.hr
<br>
<br>

<strong>------05. MAIL------</strong>
<br>
*<br>
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
*<br>
POSTFIX
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
*<br>
na workstationu
<br>
nslookup -type=mx domena.rhcsa
<br>
nslookup mail.domena.rhcsa
<br>
namistit /etc/resolv.conf  (search domena.rhcsa <br>
							nameserver 172.25.250.10)<br>


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
<br>na mail serveru provjera
<br>cat /var/spool/mail/mailer


</body>
</html> 
