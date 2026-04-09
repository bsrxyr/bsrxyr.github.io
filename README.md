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
ISHOD 5		EMAIL						5/7
<br>
ISHOD 9		SELINUX,FW,PROCESS,REGEX	8/22
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
sudo systemctl stop firewalld<br>
sudo chattr -i /etc/resolv.conf
<br>
<br>
------05. MAIL------
<pre style='color:#cfcfc2;background-color:#232629;'>
1. slaganje DNS-a na mail serveru

	sudo nano /etc/hosts

172.25.250.10 servera servera.domena.local
172.25.250.11 serverb serverb.domena.local mail mail.domena.local	//mail exchanger DNS zapis
172.25.250.9 workstation workstation.domena.local


	sudo nano /etc/dnsmasq.conf

interface=eth0
bind-interfaces
domain=domena.local
mx-host=domena.local,mail.domena.local,50


2. slaganje postfixa na mail serveru

		dnf install postfix
		sudo nano /etc/postfix/main.cf

myhostname = mail.domena.local
mydomain = domena.local
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain	//otkomentiramo 2.liniju
mynetworks_style = subnet
mynetworks = 172.25.250.0/24, 127.0.0.0/8
mail_spool_directory = /var/spool/mail
smtpd_banner = $myhostname ESMTP $mail_name RHCSA Mikrokvalifikacija	//opcionalno


3. na workstationu

		sudo nano /etc/resolv.conf

search domena.local
nameserver 172.25.250.10


//pa provjeriti mail exchanger zapis

	nslookup -type=mx domena.local

Name: mail.domena.local
Address: 172.25.250.11


//pa telnetom poslati mail

	telnet mail.domena.local 25
	MAIL FROM: student@domena.local
	RCPT TO: mailer@domena.local
	DATA
	Evo sadrzaj novog maila
	.
	quit


4. na mail serveru provjera

		sudo cat /var/spool/mail
		sudo cat /var/spool/mail/mailer
</pre>
<br>
<br>
------11. SELINUX, FW------
<pre style='color:#cfcfc2;background-color:#232629;'>
SELinux i Firewall

getenforce			provjera stanja selinuxa
setenforce			postavlja stanje selinuxa

/etc/selinux/congif	config od selinuxa, not much, za on boot ponašanje

firewall-cmd --reload
firewall-cmd --set-target=%%REJECT%%
firewall-cmd --add-service http --zone public
firewall-cmd --runtime-to-permanent


sudo nano /etc/httpd (u confu namistit listen port na neki drugi od 80, npr 123 i folder custom direktorija -kopirat dva bloka koja su već za /var/www pa prominit)

sudo semanage port -l
semanage port -a -t http_port_t -p tcp 123
sudo semanage port -l |grep http_port_t
sudo firewall-cmd --add-port 123/tcp --zone public


sudo nano /etc/httpd/conf.d/custom_dir.conf
&lt;VirtualHost *:123&gt;
	ServerName severa
	Documentroot /app/custom_dir
&lt;/VirtualHost&gt;

provjera s klijenta
curl http://servera
curl http://servera:123

sudo ls -laZ /var/www/
sudo ls -laZ /app

sudo semanage fcontext -a -t httpd_sys_content_t &quot;/app(/.*)?&quot; 
sudo restorecon -Rv /app</pre>
<br>
<br>
------12. Procesi, REGEX------
<pre style='color:#cfcfc2;background-color:#232629;'>
procesi
-------

sudo systemctl enable --now mysqld

faux
pidof mysqld

/etc/security/limits.conf
prlimit			za dokazat da je postavljen neki limit



regex
-----
https://regex101.com/
https://regex-generator.olafneumann.org
	
grep 'Accepted publickey' /var/log/secure
grep -i 'Accepted publickey' /var/log/secure 			case not sensitive
sudo cat /var/log/secure | grep 'Accepted publickey for' | cut -d &quot; &quot; -f11 | sort | uniq -c	 ispisat samo ip adrese (tj 11. blok odvojen razmakom) i pobrojat</pre>
<br>
<br>
<br>
<br>
------04. DHCP,DNS------

DHCP
/etc/dnsmasq.conf
user=
group=
interface=eth0 (ifconfig)
dhcp-range=x.x.x.x,x.x.x.y,24h
dhcp-option=3,x.x.x.x adresa routera

<br>
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
