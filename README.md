# implemeting wordpress website with LVM storage mgt
 # prepare a web server
 Launch ec2 instance that will serve as web server
<img width="920" alt="1_launch ec2 redhat" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ebfa5c2e-5cee-4932-bb92-53c53c01cd69">

 # create and attach 3 volumes in the same AZ as your web server, each of 10G
<img width="920" alt="1_create 3 EBS volume and attached to ec2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/1978cb83-fc13-4d16-b8a8-48b894634891">
 

#open up linux for configuration
 lsblk to inspect the devices attached to the server
<img width="567" alt="3_inspect devices attached to the ec2 server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/97165078-48ef-4c23-a2fa-92699d65967a">

# create single partition on each o the 3 disk
sudo gdisk /dev/xvdb
sudo gdisk /dev/xvdc
sudo gdisk /dev/xvdd
<img width="644" alt="4_create a single partition on each of the 3 disks" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/7496178e-4ece-43db-ad12-e75a7099dfdb">

# install lvm2 and check 
<img width="512" alt="6_install lvm2_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/15039843-9756-485b-99ea-dba15ccc451a">
<img width="514" alt="5_install lvm2_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b9d29cb6-4cb0-4c83-8110-d3f01999d498">

# check avaiable partition lvmsiskscan
 sudo lvmdiskscan
 <img width="506" alt="7_check available partition with lvmdiskscan and create physical vol" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/d81a2850-3405-4b9a-8cf4-48c3e6404f6d">

# mark each of the 3 disk as pv to be used by LVM
sudo pvcreate /dev/xvdb1
sudo pvcreate /dev/xvdc1
sudo pvcreate /dev/xvdd1
<img width="511" alt="8_sudo pvs" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/d68f9bd8-58d3-480c-9b30-80289f3e9bd5">

# create vol group
sudo vgcreate webdata-vg /dev/xvdd1 /dev/xvdc1 /dev/xvdb1

# create logical vol
sudo lvcreate -n apps-lv -L 14G webdata-vg    ***db-lv
sudo lvcreate -n logs-lv -L 14G webdata-vg
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
<img width="519" alt="9_create logical vol" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/41428427-c5b1-4b85-80a9-768b9b558f0c">

# Format the vol with ext4 file system
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
<img width="502" alt="10_format the vol" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ecb71562-f9c0-4ab9-baec-f66fb1e3b6e1">

# create directories
mount apps-lv on /var/www/html
sudo mount /dev/webdata-vg/apps-lv /var/www/html/  *** /db
<img width="494" alt="11_create var_ directory and mount_18" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/a096cbf7-5313-45cd-93e2-dc0473874944">

# use rsync utility to backup file in the log directory /var/log intp /home/recovery
sudo rsync -av /var/log/. /home/recovery/logs/
<img width="509" alt="12_ rysync_ 20" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/f6c952b1-c46e-491a-98f1-e54ac4b78d18">

# mount /var/log pm lofs-lv logical volume
sudo mount /dev/webdata-vg/logs-lv /var/log
<img width="494" alt="11_create var_ directory and mount_18" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/adb07e89-7ee2-40e4-84a3-00b4e57d1976">

# restore log files back into /var/log directory
sudo rsync -av /home/recovery/logs/log/. /var/log
!from sudo blkid
/dev/mapper/webdata--vg-apps--lv: UUID="1ace8b64-b492-4e0a-8652-b40ac2d44dbe" BLOCK_SIZE="4096" TYPE="ext4"
/dev/mapper/webdata--vg-logs--lv: UUID="e3ad5c7f-6692-41b5-9ddc-4495c74d7363" BLOCK_SIZE="4096" TYPE="ext4"

# update /etc/fstab file so that config will persist after restart
/etc/fstab
!from sudo blkid
/dev/mapper/webdata--vg-apps--lv: UUID="1ace8b64-b492-4e0a-8652-b40ac2d44dbe" BLOCK_SIZE="4096" TYPE="ext4"
/dev/mapper/webdata--vg-logs--lv: UUID="e3ad5c7f-6692-41b5-9ddc-4495c74d7363" BLOCK_SIZE="4096" TYPE="ext4"
 sudo vi /etc/fstab
 <img width="925" alt="13_sudo blkid" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/fc9a87c0-7631-4584-adda-f3f2142175e1">
 <img width="855" alt="14_edit etc_fstab" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2ec42526-0523-4370-a443-a081ed99fdf7">
<img width="941" alt="14_a_sudo vi etc_fstab" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/3ce73cd8-af6c-4c1e-a477-f8cb336aeab9">


# persist the mounting and test config
sudo mount -a
sudo systemctl daemon-reload
<img width="919" alt="15_persist the mounting" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/6a9d6f73-ae51-4a97-9f75-a204672bcb30">

# install wordpress and configuration to use mysql database
#launch an ec2 as DB server 
<img width="917" alt="a_launch ec2 redhat for DB server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/0a0c5796-966f-499f-bccf-52fd59368523">

# for DB server, create logical vol
sudo lvcreate -n db-lv -L 28G webdata-vg    

! Format the vol
sudo mkfs -t ext4 /dev/webdata-vg/db-lv

! mount apps-lv on /var/www/html
sudo mount /dev/webdata-vg/apps-lv /var/www/html/  *** /db
sudo mount /dev/webdata-vg/db-lv /db
<img width="513" alt="16_for DB_create volume,directory  and mount" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/833916bf-46f8-4444-9710-4d869aefe611">

# update /etc/fstab file so that config will persist after restart
from sudo blkid
UUID="86fafeee-9b27-4d33-919a-edfa0b753d14" BLOCK_SIZE="4096" TYPE="ext4"
!edit the
/etc/fstab
# persist the mounting 
sudo mount -a
sudo systemctl daemon-reload

<img width="947" alt="17_sudo blkid for db server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/13c7a161-8783-4a37-b6a3-85871bd98c78">

<img width="504" alt="18_edit the etc_fstab persit mounting and reload" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/321a36c7-29ab-40f2-a974-70dd3af466e3">
<img width="634" alt="19_edit etc_fstab_for DB server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/76e0d3e3-61e4-4fc1-9af5-81cfb51d7f81">

# !Install wordpress on web server

sudo systemctl enable httpd
sudo systemctl start httpd
<img width="493" alt="21_sudo yum install wget httpd" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/05dbfe48-390b-4a1c-8ad7-a4e6253d9117">
<img width="891" alt="20_sudo yum update on ec2 server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/f546415f-10b2-464e-932c-da8e2ef7ebed">
<img width="742" alt="22_enable appache " src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/33713034-51a8-413c-8341-40ab793dcca8">

# install php and dependencies
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
<img width="956" alt="23_install php_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/cc399768-db58-4843-a8d2-6159c6352db7">
<img width="812" alt="23_install php_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/1f15f4d1-bb11-4089-a300-813db3762c79">
<img width="899" alt="23_install php_3" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/3ce5db20-298e-4d8f-b993-10ba91f263b5">
<img width="935" alt="23_install php_5" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/abf47f39-469f-42dd-b5db-7b7d40b9c035">
<img width="886" alt="23_install php_4" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ca3dbea4-2e67-44bd-9952-6ede754b1f71">
<img width="906" alt="23_install php_6" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/0fa66a35-3dc2-44c9-9796-067d9c5a9b0b">
<img width="878" alt="23_install php_7" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/fcf7f29d-84c7-4b5d-b25c-4c2993ced631">

# restart apache2
 sudo systemctl restart httpd

# download wordpress to /var/www/html
mkdir wordpress
cd   wordpress
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
cp wordpress/wp-config-sample.php wordpress/wp-config.php
cp -R wordpress /var/www/html/
<img width="496" alt="24_download wordpress to var_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/a8fc6504-2023-4ca5-91c3-e589b6637bce">
<img width="512" alt="24_download wordpress to var_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/06f3a1f2-cc09-472e-80b0-2a25bbf3b89f">
<img width="487" alt="24_download wordpress to var_3" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b5443fad-3a1a-4437-b913-74b7f9e5dce7">

! configure SElinux, permisiion for web server to work with php and connect to db
 sudo chown -R apache:apache /var/www/html/wordpress
 sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
 sudo setsebool -P httpd_can_network_connect=1
 
<img width="502" alt="25_configure SElinux" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/48fa7b6c-581e-43ec-8122-41049a66b864">

# !install mysql on db server ec2
sudo yum update
sudo yum install mysql-server
<img width="504" alt="26_install mysql on DB server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/6f67008f-5875-4a05-964b-e95743a2c02c">

# start mysql
sudo systemctl restart mysqld
sudo systemctl enable mysqld
<img width="493" alt="27_restart msq and enable" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/c6ce8da0-dada-4b75-90c0-a70f02a719cd">

# configure DB to work with wordpress
sudo mysql
CREATE DATABASE wordpress;
CREATE USER `myuser`@18.222.87.145 IDENTIFIED BY 'mypass';
GRANT ALL ON wordpress.* TO 'myuser'@'18.222.87.145';
FLUSH PRIVILEGES;
SHOW DATABASES;<img width="494" alt="29_configure db to work with wordpress_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/76820b1f-58db-40b3-9e2a-d4c75c669606">
exit


# configure wordpress to connect to remote database
<img width="877" alt="b_configure wordpress to connect to DB server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/c00d43b0-9da9-43d1-8082-2d74d762f646">

# on web server , install mysql client
sudo yum install mysql
sudo mysql -u myuser -p -h 172.31.39.75<img width="919" alt="30_wordpress page" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/a58b1a2d-4a6c-43bb-9687-2cfeaa4449ea">


<img width="494" alt="29_configure db to work with wordpress_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/a6693932-6688-4da0-8c9d-1b24d9ef9fe2">

# trying accessing from browser 
<img width="919" alt="30_wordpress page" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2149b6af-b0bc-4863-a8d6-5f764faa404c">
