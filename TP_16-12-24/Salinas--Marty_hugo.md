# TP linux 

### installation nginx
```powershell
sudo apt install nginx
```
```powershell
systemctl status nginx
```
### Lancement nginx
```powershell
systemctl enable nginx
```
### Paramêtre des mots de passe
```powershell
apt-get install libpam-pwquality
```
```powershell
nano /etc/security/pwquality.conf
```
 --> minlen = 12 && minclass = 3

### Creation d'un utilisateur et de son mot de passe 
```powershell
adduser hugo
```
```powershell
passwd hugo
```
### On autorise les sudo pour le nouvel utilisateur
```powershell
adduser hugo sudo
```
### Interdire les connections ssh en root
```powershell
nano /etc/ssh/sshd_config
```
--> PermitRootLogin no
### Téléchargement et lancement de firewalld
```powershell
apt install firewalld
```
```powershell
systemctl enable firewalld
```
```powershell
sudo systemctl start firewalld
```
### Téléchargement et lancement de fail2ban
```powershell
apt install fail2ban
```
```powershell
nano /etc/fail2ban/jail.local
```
--> 

[sshd]
enabled = true
port = 2222
logpath = /var/log/secure
maxretry = 3
bantime = 600

```powershell
touch /var/log/secure
```
```powershell
sudo systemctl enable fail2ban
```
```powershell
sudo systemctl start fail2ban
```
```powershell
sudo apt-get install auditd
```




