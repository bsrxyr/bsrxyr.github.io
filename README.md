# bsrxyr.github.io
Quick reference

01.repos,users

*repo dump
dnf list installed > /root/dnf_list.txt

*epel
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm


*users,groups

useradd bob
groupadd bobs

useradd -m -d /shared/bobs_homedir bob		//dodjeljivanje home dira
usermod -g bobs bob							//dodavanje u grupu
sudo usermod -aG wheel bob					//wheel je sudoers na RH

*ownership

chown bob /shared/foldername

chgrp groupname file.txt	//grupa je owner


*permissions
  	owner group all
r=4
w=2
x=1

chmod 755 filename.py

chmod -R permissions directory		//rekurzivno


*ssh
/etc/ssh/sshd_config
Match User bob
PasswordAuthentication yes

ssh bob@serverb
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no bob@serverb 	//forsiran pass login
