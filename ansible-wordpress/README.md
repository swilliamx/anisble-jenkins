Set Up WordPress Using Ansible

Step 1: Create a Directory Structure
mkdir ~/wordpress
cd ~/wordpress
mkdir roles vars files
cd roles
mkdir -p apache/tasks php/tasks mysql/tasks wordpress/tasks firewall/tasks

Step 2: Create an Inventory File
vim ~/wordpress/inventory.txt
[target]
md-student-01.example.com

Where:
ansible_host is the IP address of the Target host.
ansible_user is the root user of the Target host.
ansible_ssh_pass is the password of the root user on the Target host.

Step 3: Create an Ansible Variable
vim ~/wordpress/vars/default.yml

Where:
php_modules is the list of all PHP modules required for WordPress.
mysql_root_password is the MySQL root password that you want to set.
mysql_db is the database name of WordPress that you want to create.
mysql_user is the database user name of WordPress that you want to create.
mysql_password is the password of the MySQL user that you want to set.
http_host is the FQDN name of your WordPress website.
http_conf is the name of the WordPress configuration file.
http_port is the port of the Apache webserver.

Step 4: Create an Apache Virtual Host Template File
vim ~/wordpress/files/httpd.conf

In this lesson, we will learn how to install and set up the WordPress website using Ansible on CentOS 8.

Prerequisites
A valid domain name pointed with your server IP. In this tutorial, we will use wp.example.com.
Step 1: Create a Directory Structure
First, you will need to create a directory structure for the Ansible where all of your configurations will be stored.

Ansible directory structure
You can run the following commands to create a directory structure:
mkdir ~/wordpress
cd ~/wordpress
mkdir roles vars files
cd roles
mkdir -p apache/tasks php/tasks mysql/tasks wordpress/tasks firewall/tasks
After running the above commands. Your directory structure should look like the following:

tree ~/wordpress/
You should see the tree view of your directory structure.
/root/wordpress/
├── files
├── roles
│   ├── apache
│   │   └── tasks
│   ├── firewall
│   │   └── tasks
│   ├── mysql
│   │   └── tasks
│   ├── php
│   │   └── tasks
│   └── wordpress
│       └── tasks
└── vars

Step 2: Create an Inventory File
Next, you will need to create an inventory file to define your Target host. You can create it with the following command:

vim ~/wordpress/inventory.txt
Add the following lines:

[student]
md-student-01.example.com

Step 3: Create an Ansible Variable
Next, you will need to define a variable to store the information about common things including, MySQL user, Apache host, Port, Password, PHP extensions, etc. You can define the variables inside the vars file as shown below:

vim ~/wordpress/vars/default.yml
Add the following lines:

#PHP Settings
php_modules: [ 'php', 'php-curl', 'php-gd', 'php-mbstring', 'php-xml', 'php-xmlrpc', 'php-soap', 'php-intl', 'php-zip' ]
 
#MySQL Settings
mysql_root_password: "your-root-password"
mysql_db: "wpdb"
mysql_user: "wpuser"
mysql_password: "password"
 
#HTTP Settings
http_host: "wp.example.com"
http_conf: "wp.example.com.conf"
http_port: "80"
Save and close the file when you are finished.

Where:

php_modules is the list of all PHP modules required for WordPress.
mysql_root_password is the MySQL root password that you want to set.
mysql_db is the database name of WordPress that you want to create.
mysql_user is the database user name of WordPress that you want to create.
mysql_password is the password of the MySQL user that you want to set.
http_host is the FQDN name of your WordPress website.
http_conf is the name of the WordPress configuration file.
http_port is the port of the Apache webserver.

Step 4: Create an Apache Virtual Host Template File
vim ~/wordpress/files/httpd.conf

Step 5: Create a Playbook for Apache Roles
Install the Apache package.
Start the Apache service and enable it to start at boot.
Create an Apache web root directory.
Copy the Apache virtual host configuration template file from the Ansible control machine to the Ansible Target host.

vim ~/wordpress/roles/apache/tasks/main.yml

Step 6: Create a Playbook for PHP Role
Install PHP Remi repository.
Reset the default PHP repository and enable the Remi repository.
Install PHP and required modules.

vim ~/wordpress/roles/php/tasks/main.yml

Step 7: Create a Playbook for MySQL Role
Install MySQL and other packages.
Start the MySQL service and enable it to start at boot.
Set the MySQL root password.
Create a database for WordPress.
Create a database user for WordPress.

vim ~/wordpress/roles/mysql/tasks/main.yml

Step 8: Create a Playbook for WordPress Role
Download and extract WordPress to the Apache web root directory.
Set ownership for WordPress.
Set permissions for directories.
Set permissions for files.
Rename WordPress sample configuration file.
Define database settings in the WordPress configuration file.
Restart the Apache service.

vim ~/wordpress/roles/wordpress/tasks/main.yml

Step 9: Create a Playbook for Firewall Role
Disable the SELinux.
Disable the SELinux on the fly.
Configure the firewall to allow Apache service.
Reload the firewall rules.
You can create a playbook for the firewall with the following command:

vim ~/wordpress/roles/firewall/tasks/main.yml

Step 10: Create a Main Playbook
vim ~/wordpress/playbook.yml

Step 11: Run the Ansible Playbook
cd ~/wordpress
ansible-playbook playbook.yml -i inventory.txt

Step 12: Access WordPress Installation Wizard

