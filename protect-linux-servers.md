# How to Protect Linux servers
## 1. Update your software Operating system and applications
install unattend upgrade package - to install any security updates automatically.
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
make sure unattended package working 
sudo unattended-upgrade --dry-run --debug

## 2. Do not forget to update your containers - like watchtower.

## 3. Secure your SSH access
ssh its very secure itself most problems with ssh is just of results bad habits people make. When whenever you are using credentionals to authenticate via ssh make sure you are always using strong password. if you want to avoid using passwords compeletely is even better and more comfortable to use private and public ssh keys because you don't have to remember any credentionals and also avoid re-using password accidentally.
create new user.
useradd username -m -s /bin/bash -c "administrative user"
usermod -aG sudo,adm,docker username
passwd username
<enter-strong-password>
you can authenticate username and its password to log in to the system and also you can add private and public keys to get rid of entering a password authentication. to do that from whenever you want to authenticate to your linux server

on the client
``` ssh-keygen -b 4096 -C "comment or describtion" ```
enter your phassphrase: passphrase protects your private key from being used by someone who doesn't know the passphrase. without passphrase anyone who gains access to your computer has the potential to copy your privatekey.
two files will be created public key and private key.
  private should never share with anyone its like your root user password. but the public key you can share it with anyone and you can distribute all your servers and you can use that corresponding private key to authenticate to all the users where you have distributed your public key to.

``` scp id_rsa.pub user@hostname:/home/username/.ssh/authorized_keys
sudo chown -R user:group .ssh ```

configure ssh to accept only authentication via private and public key pass which is good idea. and disable root users login via ssh 

``` sudo vim /etc/ssh/sshd_config ```
PermitRootLogin yes to no
PasswordAuthentication yes to no
sudo systemctl restart ssh

using ssh is great if you're only who is administrating the server but if you want manage shared access in a team and have audit login on whats happening on this server use 2FA and an access proxy.

Network security 
Do not expose unused services

ss -ltpn - you will get all apps listening on network ports

Use firewall
ufw (uncomplicated firewall) doesn't care about what is exposed in a container even you do not allow in ufw that service will be accessible.
technically you can see the reason when the closer look at the iptable chains because ufw operates in default chains and docker creates separate chain for its network that is ignored by ufw.

use a reverse proxy
use VPNs, DMZs and access gateways.	
use an IPS (Intrusion Prevention System) - like fail2ban
sudo apt install fail2ban
sudo systemctl enable fail2ban --now
sudo systemctl status fail2ban
sudo fail2ban-client status
sudo fail2ban-client status sshd

Isolate Applications with Docker

apparmor - in ubuntu its installed by default
sudo apparmor_status
