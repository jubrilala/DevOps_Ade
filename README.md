# implemetn wordpress website with LVM storage mgt
 # prepare a web server
 Launch ec2 instance that will serve as web server
<img width="920" alt="1_launch ec2 redhat" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ebfa5c2e-5cee-4932-bb92-53c53c01cd69">

 # create and attach 3 volumes in the same AZ as your web server, each of 10G
<img width="920" alt="1_create 3 EBS volume and attached to ec2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/1978cb83-fc13-4d16-b8a8-48b894634891">
 
 # inspect 
!open up linux for configuration
 lsblk to inspect the devices attached to the server
df -h


sudo gdisk /dev/xvdb
sudo gdisk /dev/xvdc
sudo gdisk /dev/xvdd

