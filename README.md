# mysql5.7_configuration

## Step 1: Download mysql-apt-config package
```
wget https://dev.mysql.com/get/mysql-apt-config_0.8.12-1_all.deb
```
## Step 2: Install mysql-apt-config package
```
sudo dpkg -i mysql-apt-config_0.8.12-1_all.deb
```
## When prompted, select Ubuntu Bionic.
## Step 3: Select MySQL Server & Cluster option
# When prompted, select the MySQL Server & Cluster option. Then, select mysql-5.7 and finally select Ok.

## Step 4: Update apt
```
sudo apt update
```
## Step 5: Add the MySQL repository GPG key
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 467B942D3A79BD29
```
## Step 6: Update apt again

```
sudo apt update
```
## Step 7: Check the installation candidate
```
sudo apt-cache policy mysql-server
```
## Step 8: Install MySQL
```
sudo apt install -f mysql-client=5.7* mysql-community-server=5.7* mysql-server=5.7*
```
## Step 9: Secure the MySQL installation
```
sudo mysql_secure_installation
```
## Step 10: Login to MySQL
```
mysql -u root -p
```
## Step 11: Verify the version of MySQL
```
SELECT VERSION();
```
## Step 12: Create a new MySQL user
```
CREATE USER 'username'@'localhost' IDENTIFIED BY 'user_password';
```
## Step 13: Grant privileges to the MySQL user
```
GRANT CREATE, SELECT ON *.* TO 'username'@'localhost';
```
## Step 14: List all MySQL users
```
SELECT user FROM mysql.user;
```
## Step 15: Check the MySQL service status
```
sudo systemctl status mysql
```
## Step 16: Quit MySQL
```
mysql> quit
```
## Step 17: Create a new MySQL user and grant all privileges
```
mysql> CREATE USER '[newuser]'@'localhost' IDENTIFIED BY '[password]';
mysql> GRANT ALL PRIVILEGES ON * . * TO '[newuser]'@'localhost';
mysql> FLUSH PRIVILEGES;
```
## Step 18: Manage permissions for a MySQL user
```
mysql> GRANT [type of permission] ON [database name].[table name] TO '[username]'@'localhost';
mysql> REVOKE [type of permission] ON [database name].[table name] FROM '[username]'@'localhost';
mysql> DROP USER '[username]'@'localhost';
```
#### Step 19: Allow remote connections to MySQL
## Open /etc/mysql/my.cnf. (Or /etc/mysql/mysql.conf.d/mysqld.cnf if later than 5.7) Comment this following line: bind-address = 127.0.0.1. Run command sudo service ## mysql restart. Enter mysql shell, execute the following command: GRANT ALL PRIVILEGES ON . TO '[username]'@'[ip]' IDENTIFIED BY '[password]' WITH GRANT OPTION. If ## all ip addresses are allowed, replace [ip] to %.
## Step 20: Fix "MySQL ERROR 1819 (HY000): Your password does not satisfy the current policy requirements"
```
mysql> SHOW VARIABLES LIKE 'validate_password%';
mysql> create user 'ostechnix'@'localhost' identified by 'Password123#@!';
mysql> SET GLOBAL validate_password.policy = 0;
OR
mysql> SET GLOBAL validate_password.policy=LOW;
mysql> SHOW VARIABLES LIKE 'validate_password%';
If the password policy doesn't change, exit from the mysql prompt and restart mysql service from your Terminal window:
sudo systemctl restart mysql
```
## Step 21:Configure a firewall: Use a firewall to restrict access to MySQL from outside your network. You can use the ufw firewall on Ubuntu by running the following commands:
```
sudo ufw allow mysql
sudo ufw enable
```
## Step 22:Create a new user for application access: Instead of using the root user for all database access, create a new user with limited privileges for each ##application that needs database access. This will improve security and limit potential damage in case of a security breach.
##To create a new user with limited privileges, connect to the MySQL server as root and run the following commands:
```
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT SELECT, INSERT, UPDATE, DELETE ON database_name.* TO 'newuser'@'localhost';
FLUSH PRIVILEGES;
```
## Step 23:Backup your data: Regularly backup your MySQL data to ensure that you can recover from data loss or corruption. You can use the mysqldump utility to backup your MySQL databases. For example, to backup the database named 'database_name', run the following command:
##Export:
```
mysqldump -u username -p database_name > backup.sql
```
##Import
```
mysql -u username -p database_name < backup.sql
```
