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


## Connecting to ec2 server
<img width="876" alt="1_Ubuntu server login on AWS EC2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/6355e02c-b637-425c-8053-adcde9573650">

## Edit inbound rule
<img width="751" alt="Edit inbound rule" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/e6a47245-9e4a-47c8-a7c8-b513872cd877">

## Installing apache server
<img width="563" alt="5_check apache server is accessible locally_3" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/25982c2c-fadd-4849-8732-0a0377edde2c">
<img width="613" alt="5_check apache server is accessible locally_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b3148ff2-2bfa-4934-b16e-c5526938b686">
<img width="572" alt="4_verify apache2 running as a service" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/89d4cd0b-e242-4193-b652-ea4bc38437f6">
<img width="617" alt="3_Run apache2 package installation_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/40d78824-7f71-45f7-b670-f8b2ab15ce38">
<img width="669" alt="3_Run apache2 package installation_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ec0e0566-0a35-429e-a362-d4269a44b93d">
<img width="764" alt="2_Installing Apache_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2567df42-f50d-4c90-b65b-9c3a2f2253fb">
<img width="764" alt="2_Installing apache_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/72675ca7-23b3-4a0b-a35a-d49a142ac3d7">


## Installing mysql server
<img width="602" alt="7_my sql installatn completed" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/66afe1b7-429a-4bd4-93ac-bd15eb9423c8">
<img width="770" alt="6_installing mysql_server_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/f82e123f-9c88-4832-b992-0943915bc485">
<img width="769" alt="6_installing mysql_server_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/9ce1c63f-8d10-422e-8e94-ec03629ec860">
<img width="573" alt="8_set password for root user" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/26a6be86-11a5-4c8a-a1d3-03e47101c93d">


## Installing php
<img width="769" alt="8_installing php_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/926be819-b13d-464c-8a84-03c294f66557">
<img width="764" alt="8_installing php_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/504893ce-da32-4637-bfa1-7652222edd63">

## Enable php on the web site
<img width="664" alt="Enable php on the website_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/4dd4d6c1-5e15-44da-a07b-8c54114e07e3">
<img width="572" alt="Enable php on the website_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b827df41-b9b3-45ce-8328-3b7a053afa35">


## creating a virtual host for web site
To create a virtual host for my website files using apache, we create a directory “projectlamp” Then change ownership :

<img width="481" alt="creating a virtual host for web site_3" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/83360390-b6a8-4b9e-9649-3707ad413e2e">
<img width="616" alt="creating a virtual host for web site_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/34923825-3ae8-410b-b819-d5e495620b32">
<img width="637" alt="creating a virtual host for web site_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/cd5dc0f5-130b-480a-8312-22c264e45c24">
<img width="769" alt="creating a virtual host for web site_4" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b7ce754f-585e-42ef-b23c-0bc352bce158">

## Created a new configuration file by running the command
 sudo vi /etc/apache2/sites-available/projectlamp.conf
 We place the following in the file we created

<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
 <img width="611" alt="enable new virtual host" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/fe9b9b86-6354-438b-b512-47f27b28bfd2">

## enable the new virtual host
sudo a2ensite projectlamp
## disable the default website that comes with apache.
sudo a2dissite 000-default
### Test config file
sudo apache2ctl configtest 
## reload the apache webserver
sudo systemctl reload apache2
<img width="611" alt="enable new virtual host" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/a8df2989-e150-48c1-8b6b-de039f8df2a1">

## enable PHP on the website, we open this file and edit it:
sudo vim /etc/apache2/mods-enabled/dir.conf
<img width="756" alt="Enable php on the website" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/8124b767-8ce3-47af-a67e-0ee6e1b97e20">

## Create and open another file in “projectlamp” named “index.php”
vim /var/www/projectlamp/index.php
Add this to the file

<?php
phpinfo();
<img width="892" alt="create and open another file in project lamp" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/7446d2af-fe53-47ed-9325-aee25cee0415">
















