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
(Defaults  passwd_tries=3 [and] Defaults  insults [and] Defaults  syslog=local1 Defaults          secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin" Defaults requiretty)  
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
# Cron
crontab -e (choose editor)  
([write] */10 * * * * //usr/local/bin/monitoring.sh)  

# Bonus
sudo apt install lighttpd  
sudo apt install mariadb-server  
sudo apt install php-cgi php-mysql  
sudo ufw allow 80 (allow connections on port 80)  
sudo mysql_secure_installation (start script to remove insecure default mysql settings)  
sudo mariadb (log into MariaDB)  
CREATE DATABASE bonus; (creates SQL database)  
GRANT ALL ON bonus.* TO 'lorbke'@localhost IDENTIFIED BY 'Born2beRoot' WITH GRANT OPTION; (creates a new user with full permissions)  
FLUSH PRIVILEGES; (reloads the tables of the database so changes take effect)  
sudo wget http://wordpress.org/latest.tar.gz -P /var/www/html (get newest wordpress .tar)  
sudo tar -xzvf /var/www/html/latest.tar.gz (extract)  
sudo rm /var/www/html/latest.tar.gz  
sudo cp -r /var/www/html/wordpress/* /var/www/html  (copy wordpress files)  
sudo rm -rf /var/www/html/wordpress  
sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php  
sudo nano /var/www/html/wp-config.php  
(insert database name, user name and password)  
sudo lighty-enable-mod fastcgi  
sudo lighty-enable-mod fastcgi-php  
sudo service lighttpd force-reload (these services are needed for a functioning webserver)   
(open browser, connect to 127.0.0.1:80 | make sure port 4242 and 80 are allowed in VirtualBox settings)  
# Bonus Service (FTP)
sudo apt install vsftpd  
sudo ufw allow 20 (data port in active mode session)    
sudo ufw allow 21 (command port in active mode session)  
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.template  
sudo nano /etc/vsftpd.conf  
(listen=YES listen_ipv6=NO pasv_enable=YES pasv_min_port=40000 pasv_max_port=45000)  
sudo nano /etc/vsftp.userlist  
([write] lorbke)  
sudo systemctl restart vsftpd  
sudo systemctl enable vsftpd  

# EVALUATION
hostname has to be modified, how to do this?  
password policies are stored in /etc/pam.d/common-password file.  
hostname capital I vs small i IP difference: https://stackoverflow.com/questions/60615270/hostname-i-vs-hostname-i-in-linux  
FTP active vs passive: in active mode FTP server initializes the data connection, often prompting a block from the firewall of the client. in passive mode both connections are initialized by the client, therefore not causing any firewall problems  
sudo lsof -P -i | grep LISTEN
ssh lorbke@localhost -p 4242
