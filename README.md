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
login
lorbke
Born2beRoot
groups (also the group is not lorbke42 as stated in subject, ask Wendelin)
sudo nano /etc/pam.d/common-password (open password policy file)
([find] password [success=2 default=ignore] pam_unix.so obscure sha512 [and add] minlen=10)
sudo apt install libpam-pwquality (install password complexity library)
y
sudo nano /etc/pam.d/common-password (open password policy file)
[find] password        requisite                       pam_pwquality.so retry=3 [and add] ucredit=-1 lcredit=-1 dcredit=-1 difok=7 usercheck=1 enforcing=1 maxrepeat=3) (https://www.networkworld.com/article/2726217/how-to-enforce-password-complexity-on-linux.html)




EVALUATION
hostname has to be modified, how to do this?
password policies are stored in /etc/pam.d/common-password file.
