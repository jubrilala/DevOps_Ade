# Client-Server Architecture Project
## Run below on ubuntu server
<img width="904" alt="4_linux based virtual server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/d6597ba5-35ba-45aa-bb85-19ea350424d2">

$curl -Iv www.propitixhomes.com
<img width="493" alt="3_curl -Iv" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/eff52da1-366b-4edf-9e29-51902d0bd494">
## implement client server architecture using mysql DBMS
launch linux server and client
install mysql-server<img width="506" alt="1_sudo apt update" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b8c1bb49-3835-4263-95a9-9d98095515d8">

sudo apt install mysql-server
<img width="481" alt="1_sudo apt install mysql-server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ca10f5bf-5b85-4bb8-870c-504f5e4a235a">

## install my sql client
sudo apt install mysql-client <img width="506" alt="1_sudo apt update" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/cd5edbd3-9a43-491c-bdc3-1e83505d4157">
<img width="488" alt="4_sudo apt install mysql-client" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/778e34f7-51af-4cd9-b01a-14d20e448d88">

## Allow client access to server security group
<img width="911" alt="6_Allow client access to server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/90396a30-7301-4581-a989-dcb3cdaf6370">

## configure mysql-server to allow connection from remote host
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf<img width="485" alt="5_edit mysql conf on server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/5376807c-a81d-4f8b-9212-d1f872cef6f2">

## On the mysql-server create user on the DB
CREATE USER 'testuser'@'%' IDENTIFIED BY '123456';
 
GRANT ALL PRIVILEGES ON *.* TO 'name'@'%' WITH GRANT OPTION;
 
FLUSH PRIVILEGES;
sudo mysql

mysql> select user,host from mysql.user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| name             | %         |
| debian-sys-maint | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
<img width="566" alt="7_create user on mysql server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2bfc26d1-b27e-41e1-9dd1-b135f5a3999f">

## login to the mysql server from mysql client
sudo mysql -h 172.31.0.4 -u testuser -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; 

## Check that you successfully login to remote mysql server and can perform sql queries
# Show databases;
<img width="601" alt="8_login to mysql server from the mysql client" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/0c72b9e1-982f-40f8-ab84-75722d3bf2d7">























