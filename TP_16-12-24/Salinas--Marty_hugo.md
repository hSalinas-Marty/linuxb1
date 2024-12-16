# TP linux 


sudo apt install nginx
systemctl status nginx
systemctl enable nginx
apt-get install libpam-pwquality
nano /etc/security/pwquality.conf --> minlen = 12 && minclass = 3
adduser hugo
passwd hugo       Test987!test
adduser hugo sudo
nano /etc/ssh/sshd_config --> PermitRootLogin no
ssh-keygen -t rsa

apt install firewalld
systemctl enable firewalld
sudo systemctl start firewalld

apt install fail2ban
nano /etc/fail2ban/jail.local --> 

[sshd]
enabled = true
port = 2222
logpath = /var/log/secure
maxretry = 3
bantime = 600

touch /var/log/secure
sudo systemctl enable fail2ban
sudo systemctl start fail2ban

sudo apt-get install auditd