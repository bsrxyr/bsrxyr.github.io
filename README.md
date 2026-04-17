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
ISHOD 9		GET	8/22 !!!
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
dnf install cockpit-selinux
<br>
<br>
sudo chattr -i /etc/resolv.conf
<br>
<br>
------APACHE------
<pre style='color:#cfcfc2;background-color:#232629;'>
https://httpd.apache.org/docs/2.4/vhosts/examples.html

//custom port u apacheu:
nano /etc/httpd/conf/httpd.conf
					Listen brojporta
									
//custom home folder:
sudo nano /etc/httpd/conf.d/custom_dir.conf
<VirtualHost *:80>
ServerName severa.lokal.com
Documentroot "/var/custom_dir"
</VirtualHost>

//Ne zaboravit prava pristupa i ownership:
chmod -R 755 /var/custom_dir/
chown -R apache:apache /var/custom_dir

//sa klijenta upucat u /etc/hosts adrese da ne dižeš DNS server

//provjera s klijenta
curl http://servera
curl http://servera:80
</pre>
<br>
------FIREWALL------
<pre style='color:#cfcfc2;background-color:#232629;'>
sudo systemctl start firewalld
firewall-cmd --get-active-zones

//promjene uvik spremit u permanent!
sudo firewall-cmd --runtime-to-permanent

//propusti webserver
sudo firewall-cmd --add-service http --zone public

//propusti webserver kroz port 8080
sudo firewall-cmd --add-port 8080/tcp --zone public

//forwardaj dolazni promet sa port 8080 na port80
sudo firewall-cmd --permanent --add-forward-port=port=8080:proto=tcp:toport=80
</pre>
<br>
------SELINUX------
<pre style='color:#cfcfc2;background-color:#232629;'>
getenforce			provjera stanja selinuxa
setenforce 1		postavlja enforcing stanje

sudo semanage port -a -t http_port_t -p tcp 99	//enable webservera na port 99
sudo semanage port --list | grep 99				//provjera da je dodano u context

//ako je port 69 zauzet, grepaj port naredbom gore i vidi imeservisa koji ga je zauzea, pa:
sudo semanage port -m -t imeservisa -p tcp 69
sudo semanage port -m -t http_port_t -p tcp 69
sudo semanage port -d -t imeservisa -p tcp 69	//pa ga ovo tek pobrise s imeservisa,al moze i ostat na oba,nije kljucno


sudo netstat -tuln							//mrežna provjera da radi na portu 99
curl IPADRESA:port							//može i ovako



//za custom dir webservera:

sudo journalctl -xefu setroubleshoot	//na vrhu napiše grešku i rješenje
sudo ls -laZ	//pokazuje selinux type (target) foldera na razini sistema

sudo semanage fcontext -a -t httpd_sys_content_t "/var/custom_dir(/.*)?"
sudo restorecon -R -v "/var/custom_dir"	//uvik ovo nakon promjene fcontexta!
</pre>
<br>
------PROCESI------
<pre style='color:#cfcfc2;background-color:#232629;'>
Limiti se učitavaju prilikom logina! (odradi logout pa login)

//Show status of PROCESSNAME
ps -aux | grep PROCESSNAME


//Show all PROCESSNAME of user root sortirano
ps -u root | sort


//Set PROCESSNAME priority to lowest
sudo ps -aux | grep sshd | awk '{print $2}' | head -1	//gets PID
sudo renice -n 19 -p PID


//highest priority je -20


//Show priority(niceness)
sudo ps -aux | grep PROCESSNAME



//Set max number of files "student" can open to 69
sudo nano /etc/security/limits.conf
		student    hard    nofile    69

//provjera: su - student -c "ulimit -n"

//za dokazat da je postavljen neki limit: prlimit

//make sure /etc/pam.d/system-auth or /etc/pam.d/password-auth contains: session required pam_limits.so



//Set priority of all "student" run process to 9
sudo nano /etc/security/limits.conf
		student	-	priority	-9
</pre>
<br>
------REGEX------
<pre style='color:#cfcfc2;background-color:#232629;'>
ps aux | grep '^root\b'		//ps aux printa sve procese, grep traži tekst koji slijedi, ^root je regex da root mora biti na početku, \b je kraj riječi(može se stavit i space)

//pa se može dalje rezultat filtrirat sa | awk '{print $2 $11}'		ispisuje stupac 2 i stupac 11
//pa se može minjat rezultat sa | sed 's/\[//g'		s znači zamijeni, / je delimiter pa unutar njih stoji znak uglate zagrade koju želimo zaminit, a kako je ona specijalni znak onda je treba escapeat sa \], a g je global, tj napravi to ne samo za prvi takav znak nego za sve


//counta broj rezultata
grep 'blabla' -c

//ispisuje i broj linije uz rezultat
grep 'blabla' -n

//not case sensitive
grep 'BlaBla' -i

//redak sa ekskluzivno blabla (ne redak blabla blablo)
grep 'blabla' -x

//linija počinje s blabla
grep '^blabla'

//linija završava s blabla
grep 'blabla$'

//print linija 10char long
grep "^.\{10\}$" /usr/share/dict/words

//traženo počinje slovom t, pa samoglasnik, pa sh
grep 't[aeiou]sh'

//red počinje slovom t, pa samoglasnik, pa sh
grep '^t[aeiou]sh'

//grepaj linije s brojevima od 80-99
grep -E "\b(8[0-9]|9[0-9])\b"

//Izdvajanje IP adresa iz datoteka.txt
grep -E "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" datoteka.txt

//brojanje jedinstvenih IP adresa u logovima (-o je bez cijelog retka, samo IP)
grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" access.log | sort | uniq -c | sort -nr

//sudo cat /var/log/secure | grep 'Accepted publickey for' | cut -d " " -f11 | sort | uniq -c	 ispisat samo ip adrese (tj 11. blok odvojen sa space) i pobrojat



targeti obično:
/var/log/secure
/usr/share/dict/words


//Find all files in /etc ending in ".config"
find /etc -name "*.config"

//Search only files (exclude directories):
find / -type f -name "*.config"

//Not case sensitive match
find / -type f -iname "*.config"

//find u /etc sve .conf fajlove koji sadrže "password"
sudo find /etc -type f -name "*.conf" -exec grep -l "password" {} +
</pre>
