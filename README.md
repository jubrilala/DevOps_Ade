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





























