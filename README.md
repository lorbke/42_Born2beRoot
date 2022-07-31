# Born2beRoot
Installation and setup of a simple Debian server.

OS
Debianx64

PASSWORDS
Root: 42Heilbronn
User Login: lorbke
User: Born2beRoot
Encryption: Sichererstr15

COMMANDS
su (login to root user)
42Heilbronn
apt install sudo
su - (move to root directory to be able to use usermod command) ?
usermod -aG sudo lorbke (add user lorbke to sudo group)
sudo groupmod -n lorbke42 lorbke
# Password Policies
sudo nano /etc/pam.d/common-password (open password policy file)
([find] password [success=2 default=ignore] pam_unix.so obscure sha512 [and add] minlen=10)
sudo apt install libpam-pwquality (install password complexity library)
sudo nano /etc/pam.d/common-password (open password policy file)
([find] password        requisite                       pam_pwquality.so retry=3 [and add] ucredit=-1 lcredit=-1 dcredit=-1 difok=7 usercheck=1 enforcing=1 maxrepeat=3) (https://www.networkworld.com/article/2726217/how-to-enforce-password-complexity-on-linux.html)
sudo nano /etc/login.defs
([find] PASS_MAX_DAYS 30 [and] PASS_MIN_DAYS 2 [and] PASS_WARN_AGE 7)
# Sudo Config
sudo visudo /etc/sudoers.d
(Defaults  passwd_tries=3 [and] Defaults  insults [and] Defaults  syslog=local1 Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin" Defaults requiretty)
sudo visudo /etc/rsyslog.conf (visudo because it will do a lot of error checks and create a tmp file to protect the original sudo file from changes, secure_path will restrict the system to only search for executables in specified paths, this is a security measure to make sure the correct executables are used)
sudo nano /etc/rsyslog.conf
([find] auth,authpriv.*;local1.none     /var/log/auth.log [insert above] local1.*                     /var/log/sudo/sudo.log)
sudo systemctl restart rsyslog (restart syslog service for changes to take effect)
# SSH
sudo apt install openssh-server
sudo nano /etc/ssh/sshd_config
([find] #Port 22 [replace] Port4242)
([find] #PermitRootLogin prohibit-password [replace] PermitRootLogin no)
# UFW
sudo apt install ufw
sudo ufw enable
sudo ufw allow 4242


EVALUATION
hostname has to be modified, how to do this?
password policies are stored in /etc/pam.d/common-password file.
hostname capital I vs small i IP difference: https://stackoverflow.com/questions/60615270/hostname-i-vs-hostname-i-in-linux
