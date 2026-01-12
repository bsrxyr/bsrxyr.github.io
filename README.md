<html>
<body style="background-color:darkgrey;">

<div># bsrxyr.github.io</div>
<p><strong>Linux sysadmin quick reference &#128366;</strong></p>
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

<br>
<br>
<strong>------06. WEBSRV------</strong>
<br>
<br>
Apache (httpd)
<br>
/etc/httpd/conf.d
<br>
<br>
<VirtualHost *:80>
<br>
ServerName www.apache.local
<br>
DocumentRoot /var/www/web_apache_local
<br>
</VirtualHost>
<br>
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
<br>	
sudo setenforce 0		//obavezno za nginx
<br>	
<br>
curl http://web.apache.local	//brzi dokaz websitea
<br>
</body>
</html> 
