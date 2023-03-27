## Documentation for Project 2
## LEMP Stack Implementation  - aws virtual server

## STEP 1 — INSTALLING THE NGINX WEB SERVER

-- Starting by ssh into my server labelled 'LEMP Nginx-Web Server' from my vscode terminal in windows machine 

`sudo apt update`--(running command to update my list of packages in package manager)
![Nginx-update](./Image-2/Nginx-Update-1.PNG) --screenshot shows update completed 

`sudo apt upgrade`--(To upgrade Ubuntu)
![Nginx-install](./Image-2/Nginx-Install-2.PNG)

`sudo systemctl status nginx`--(To verify that nginx is running as a Service in our Ubuntu OS)
![Nginx-Test](./Image-2/Nginx-Test-3.PNG)

`curl http://localhost:80` --(Using curling command with DNS Name option - to check how we can access Nginx server locally within ubuntu shell. Objective - Use curl command to request our Nginx Server on port 80.
![Nginx-AccessServerLocal](./Image-2/Nginx-AccessServerLocal-4.PNG)

`http://52.207.136.248` --(Opened a new browser and ran command to test if we can view our Nginx server via the internet. Webpage displayed validating Nginx web server is operational, after installation on ubuntu system.
![Nginx-AccessServerInternet](./Image-2/Nginx-AccessServerinternet-5.PNG)

`curl -s http://169.254.169.254/latest/meta-data/public-ipv4` --(Retrieving my Public IP address from my terminal.Testing how to retrieve files content in my metadata directory.
![Nginx-Retrieve-PublicIPAdd](./Image-2/Nginx-Retrieve-PublicIPAdd-6.PNG)

## STEP 2 — INSTALLING MYSQL

`sudo apt install mysql-server`--(Mysql database software installed to my apache server)
![Mysql-install](./Image-2/Mysql-install-1.PNG)

`sudo mysql` --(successful login into the MySQL console, connected as administrative database user root)
![Mysql-login](./Image-2/Mysql-Login-2.PNG)

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<password>';`--(Setting up a default authenticate password for mysql,confirming mysql query for password)
![Mysql-set-Password](./Image-2/Mysql-set-Password-3.PNG)

`sudo mysql_secure_installation`--(running interactive security script that comes pre-installed with MySQL to secure my password, password plugin validated).
![Mysql-security-install](./Image-2/Mysql-securrity-installed-4.PNG)

`mysql -u root -p`--(Test:1. successful login back to MySQL console)
![Mysql-password-test](./Image-2/Mysql-Password-Test-5.PNG)

## STEP 3 — INSTALLING PHP

`sudo apt install php-fpm php-mysql`--(Installing all 2 packages to PHP component;1. PHP fastCGI Manager, 2. PHP Module).
![PHP-install-all-2-pckages-1](./Image-2/PHP-Install-all-2-Packages-1.PNG)

## STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR

`sudo mkdir /var/www/evezidomainLEMP`--(Creating a domain for my root web directory in my Nginx web server).
![ConfigNginx-create-Domain](./Image-2/ConfigNginx-create-Domain-1.PNG)

`sudo chown -R $USER:$USER /var/www/evezidomainLEMP`--(Assign ownership of my new directory with the $USER environment variable which will reference your current system user).
![ConfigNginx-AssignOwner-](./Image-2/ConfigNginx-AssignOwner-2.PNG)

`sudo namo etc/nginx/sites-available/evezidomainLEMP`--(crete and open a new configuration file in Nginx sites-available directory using nano text editor inserted snippet script).
![ConfigNginx-CreateOpen-ConfigFile](./Image-2/ConfigNginx-CreateOpen-ConfigFile-3.PNG)

`sudo ln -s /etc/nginx/sites-available/evezidomainLEMP /etc/nginx/sites-enabled/`--(Activate my configuration by linking to the config file from Nginx’s sites-enabled directory informing Nginx to use the configuration next time it is reloaded).
![ConfigNginx-Activate-Config](./Image-2/ConfigNginx-Activate-Config-4.PNG)

`sudo nginx -t` --(Testing my configuration file syntax is error free and 2. Apache webserver is reloaded and website is now active).
![ConfigNginx-NoSyntaxError](./Image-2/ConfigNginx-NoSyntaxError-5.PNG)

`sudo unlink /etc/nginx/sites-enabled/default` --(Disabling default Nginx host that is currently configured to listen on port 80).
![ConfigNginx-Disable-DefaultNginxHost](./Image-2/ConfigNginx-Disable-DefaultNginxHost-6.PNG)

`sudo systemctl reload nginx` --(reloading Nginx to apply changes).
![ConfigNginx-Reload-Nginx](./Image-2/ConfigNginx-Reload-Nginx-7.PNG)

`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/evezidomainLEMP/index.html`--(Creating an index.html file in my web root enable test of new server block works as expected - Using Public IP address)
![ConfigNginx-Website-Using-PublicIPAd](./Image-2/ConfigNginx-Website-Using-PublicIPAd-8.PNG)

--(Using DNS Name to launch website - achieving same result Echoing Public DNS name and Public IP address)
![ConfigNginx-Website-Using-DNSName](./Image-2/ConfigNginx-Website-Using-DNSName-9.PNG)

## STEP 5 — Testing PHP with Nginx

 `sudo nano /var/www/evezidomainLEMP/info.php`--(creating a new test PHP file called (info.php) in my document root).
![PHPNginx-Creat-PHPTestFile](./Image-2/PHPNginx-Creat-PHPTestFile-1.PNG)

`<?php info.php();`--(Edited new file in my document root with valid PHP code in the nano line text editor).
![PHPNginx-PHPCode](./Image-2/PHPNginx-PHPCode-2.PNG)

`<?php info.php();`--(Access my webpage through web browser by using public IP address I set up in your Nginx configuration file, validating that Nginx can correctly hand .php files off to my PHP processor.)
![PHPNginx-Test-PublicIpAd](./Image-2/PHPNginx-Test-PublicIpAd-3.PNG)

`sudo rm /var/www/evezidomainLEMP/info.php`--(Removed PHP test file created from PHP server as it contains sensitive information of my PHP environment and my Ubuntu server).
![PHPNginx-Remove-Webpage](./Image-2/PHPNginx-Remove-Webpage-4.PNG)

## STEP 6 — RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)
`mysql -u root -p` --(successful login into the MySQL console, connected as administrative database user root)
![Mysql-login](./Image-2/Msql-Login-2-1.PNG)

`CREATE DATABASE `rain_database`;` --(successfully creating a new database in MySQL console)
![Msql-Create-Database](./Image-2/Msql-Create-Database-2-2.PNG)

`CREATE USER 'rain_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Friendly1';` --(successfully creating a new user for the newly created database and grant user full privileges on the database just created)
![Msql-CreateUser-DefaultAuth](./Image-2/Msql-CreateUser-DefaultAuth-2-3.PNG)

`GRANT ALL ON rain_database.* TO 'rain_user'@'%'` --(Give this user permission over this rain_database)
![Msql-Grant-Permission](./Image-2/Msql-Grant-Permission-2-4.PNG)

`mysql -u rain_user -p` --(Relogging into mySQL to test assigned permissions works and confirm that user has access  - "rain_database")
![Msql-Test-Permission](./Image-2/Msql-Test-Permission-2-5.PNG)

`SHOW DATABASES;` --(Showing rain_database in tabular formate "3 rows")
![Msql-Show-Database](./Image-2/Msql-Show-Database-2-6.PNG)

`CREATE TABLE rain_database.todo_list (item_id INT AUTO_INCREMENT, content VARCHAR(255), PRIMARY KEY(item_id));` --(creating a test table named todo_list from the MySQL console)
![Msql-TestTable-TodoList](./Image-2/Msql-TestTable-TodoList-2-7.PNG)

`INSERT INTO rain_database.todo_list (content) VALUES ("My first important item");` --(Insert a few rows of content in the test table. Also repeated command lines a few times, using different VALUES)
![Msql-InsertContent-TestTable](./Image-2/Msql-InsertContent-TestTable-2-8.PNG); 

`SELECT * FROM rain_database.todo_list;` --(To confirm that the data was successfully saved to my test table)
![Msql-ConfirmData-TestTable](./Image-2/Msql-ConfirmData-TestTable-2-9.PNG);

`nano /var/www/evezidomainLEMP/todo_list.php` --(Create a PHP test script that will connect to MySQL and query for my content. Create a new PHP file in your custom web root directory -  using nano editor. using snippet content into my todo_list.php scrip)
![Msql-PHP-TestScript](./Image-2/Msql-PHP-TestScript-2-10.PNG);

`http://3.86.42.242/todo_list.php` --(Access this page in my web browser by visiting public IP address configured for your website out of var/www/evezidomainLEMP/todo_list.php directory,now my PHP environment is ready to connect and interact with MySQL server.)
![Msql-AccessWebpage-PublicIpAd](./Image-2/Msql-AccessWebpage-PublicIpAd-2-11.PNG;

