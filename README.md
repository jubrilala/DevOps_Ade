# LEMP Stack Project
## EC2 instance launched and running
<img width="960" alt="launch EC2 instant" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/7ea0d0f8-184a-401d-97c6-3c1ca1773d69">
## We launch Git bash and run below cmd to connect to ec2 public IP
##ssh -i LEMP.pem ubuntu@18.221.143.192
![1_Launch Git and Connect with ssh key](https://github.com/jubrilala/DevOps_Ade/assets/88538653/1c7c4a0b-7f34-48f9-a8c0-18b227ff171a)

## Install Nginx web server
# sudo apt update
# $ sudo apt install nginx -y
![2_Apt update](https://github.com/jubrilala/DevOps_Ade/assets/88538653/56ae16b0-c857-46f3-914c-7657fe85c29f)
![3_Install Nginx](https://github.com/jubrilala/DevOps_Ade/assets/88538653/419a7daf-ea97-4675-b518-49ab187d15be)
# Verify that Nginx is successfully installed
# sudo systemctl status nginx
![4_verify Nginx installed](https://github.com/jubrilala/DevOps_Ade/assets/88538653/d98a30b2-5634-4de8-91d2-3ab9016c9bf1)

## Open inbound rule connection through port 80
![5_open inbound rule on port 80](https://github.com/jubrilala/DevOps_Ade/assets/88538653/97cace44-f5a1-4770-9608-7795769a7939)
## Check if we can access server locally in our ubuntu shell
# $ curl http://localhost:80
![6_Check that server is accessible](https://github.com/jubrilala/DevOps_Ade/assets/88538653/e2c1e43e-9452-4b25-9ee9-cbe3bae113b1)

## Test Nginxserver response to request
# http://18.221.143.192:80
![7_test Nginx response to request](https://github.com/jubrilala/DevOps_Ade/assets/88538653/42c83742-fbcc-4d6e-97ee-f2e45795604c)

## Installing mysql
# $ sudo apt install mysql-server
![8_Installing my sql](https://github.com/jubrilala/DevOps_Ade/assets/88538653/646457d2-28ba-44e9-98bf-f95d0fad63d9)

## login to mysql

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.22-0ubuntu0.20.04.3 (Ubuntu)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
# $ sudo mysql![9_login to mysql](https://github.com/jubrilala/DevOps_Ade/assets/88538653/09dc2c34-9dff-4449-87d9-bff4fb1493a9)

## Set password for root user
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
![10_set password for root user](https://github.com/jubrilala/DevOps_Ade/assets/88538653/71a66adc-1726-414e-93d1-cabc98dc8e7b)

## Run security script that come preinstalled with mysql
Start the iteractive script by running
$ sudo mysql_secure_installation
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary              file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y


Press y|Y for Yes, any other key for No:
![11_Run security script_1](https://github.com/jubrilala/DevOps_Ade/assets/88538653/d5025c47-40f9-45ff-876c-5c89ea1d0adc)

## Test login after running security script
$ sudo mysql -p
mysql> 
![12_test login to mysql after running security script](https://github.com/jubrilala/DevOps_Ade/assets/88538653/8ecd34b3-a9a9-4b3f-a0fd-20e3e7ebac70)
 Note: At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. For that reason, when creating database users for PHP applications on MySQL 8, you’ll need to make sure they’re configured to use mysql_native_password instead. We’ll demonstrate how to do that in Step 6.

## Installing PHP
$ sudo apt install php-fpm php-mysql
Now PHP components are installed , next is to configure Nginx to use them
![13_installing php](https://github.com/jubrilala/DevOps_Ade/assets/88538653/546464ff-500e-4a19-a1a7-4441f2e033ba)

## Configure Nginix to use php processor
$ sudo mkdir /var/www/projectLEMP and assign ownership of the directory with the $USER environment variable
$ sudo chown -R $USER:$USER /var/www/projectLEMP

![13_create root web directort and assign ownership of the directory with the $USER environment variable](https://github.com/jubrilala/DevOps_Ade/assets/88538653/a69c67c4-25a3-440b-a7d3-d7562aaf66b5)

## Open a new file in Nginx (site-available) directory using nano
$ sudo nano /etc/nginx/sites-available/projectLEMP
Paste below in the blanmk file created
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
![14_Open a new config file(sites-available)in Nginx directory using nano](https://github.com/jubrilala/DevOps_Ade/assets/88538653/d8ccb028-c1ee-487c-b47f-4fbc1a879ce0)

## Activate the configuration by linking to the config file from Nginx site-available directory
$ sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
# disable the default Nginx host configured to listen on port 80
sudo unlink /etc/nginx/sites-enabled/default
# reload Nginx
$ sudo systemctl reload nginx
your new website is now active but the web root /var/www.projectLEMP is empty
# Create an index.html file in that location to test that the new server block is working
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
# No go to web browser to test 
http://18.221.143.192:80
![15_Create and index html file in that location so we can test that new server](https://github.com/jubrilala/DevOps_Ade/assets/88538653/f05088a2-b376-4e1d-bde9-ad5297c3e841)

## Test PHP with Nginx
$ nano /var/www/projectLEMP/info.php

![16_creating a test php file](https://github.com/jubrilala/DevOps_Ade/assets/88538653/f49e1f94-7227-4f4b-8917-e0666d9b8039)
![17_accessing the page](https://github.com/jubrilala/DevOps_Ade/assets/88538653/f1d871bc-c4f0-44c1-bbc6-e3cefe608c22)

##  Retrieving data from mysql database with PHP
create a test database with simple "To do list: and configure access to it
$ sudo mysql
create a database named example_database and a user named example_user
$ sudo mysql
mysql> CREATE DATABASE `example_database`;
create a new userusing mysql_native_password as default authentication method and change later
mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
# Give user permision over the example_database
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
mysql> exit
$ mysql -u example_user -p
Confirm that you have access to the database
Output
+--------------------+
| Database           |
+--------------------+
| example_database   |
| information_schema |
+--------------------+
2 rows in set (0.000 sec)
Now create a test table to do list 
CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
# insert a few row of content in the table
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
# confirm that data wsas successfully saved to the table
mysql>  SELECT * FROM example_database.todo_list;
Output
+---------+--------------------------+
| item_id | content                  |
+---------+--------------------------+
|       1 | My first important item  | |
+---------+--------------------------+
1 rows in set (0.000 sec)
![18_Retrieving data from database with php](https://github.com/jubrilala/DevOps_Ade/assets/88538653/7edeb973-150f-4bad-847a-f09e6762b332)
![18_Retrieving data from database with php_2](https://github.com/jubrilala/DevOps_Ade/assets/88538653/bbb97ef5-feba-4630-8f29-5e990481cb0e)

# create a PHP script that will connect to mysql and query for my content
$ nano /var/www/projectLEMP/todo_list.php
![19_a create a new php file](https://github.com/jubrilala/DevOps_Ade/assets/88538653/4a464de1-6175-4ed4-8487-4a545c59b6c3)

The php script connect to the DB and query for the content of todo_listtable
Copy this content into your todo_list.php script
$user = "example_user";
$password = "PassWord.1";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
![19_create php script that will connect to mysql](https://github.com/jubrilala/DevOps_Ade/assets/88538653/d6150792-eb46-4568-9fcd-11ee9737b3f2)

## access the page on browser with public IP address configured for the web server
http://18.221.143.192/todo_list.php
![20_confirm the page works](https://github.com/jubrilala/DevOps_Ade/assets/88538653/1c30a94f-4851-4092-b077-7a0fc03765d3)




























