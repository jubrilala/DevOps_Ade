# automate deployment of server with nginx
## launch server1 , 2 and nginx
<img width="907" alt="0_launch server1, 2 and nginx" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/56f4fd02-b0ba-4b1b-a802-991edd99f324">

## server 1 and 2
Open a file with below cmd and paste the file below
    sudo vi install.sh

    #!/bin/bash

####################################################################################################################
##### This automates the installation and configuring of apache webserver to listen on port 8000
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
######## ./install_configure_apache.sh 127.0.0.1
####################################################################################################################

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure

PUBLIC_IP=$1

[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

sudo apt update -y &&  sudo apt install apache2 -y

sudo systemctl status apache2

if [[ $? -eq 0 ]]; then
    sudo chmod 777 /etc/apache2/ports.conf
    echo "Listen 8000" >> /etc/apache2/ports.conf
    sudo chmod 777 -R /etc/apache2/

    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

fi
sudo chmod 777 -R /var/www/
echo "<!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html
        
sudo systemctl restart apache2
<img width="752" alt="1_ sudo vi install" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/845fceb1-02cb-4624-87e9-a54bcfb6fe75">

## change file permission
    sudo chmod +x install.sh

## run the script on server 1 and 2
    ./install.sh 3.12.147.187
    ./install.sh 52.14.222.39
<img width="752" alt="2_install_server1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/d04a9a3a-df03-4c13-872c-f62aeaff44a0">
<img width="775" alt="3_install_server2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/13e7650b-a877-47cd-a15f-eec8ae041300">

## on nginx
Open a file with below cmd and paste the file below
    sudo vi nginx.sh

#!/bin/bash

######################################################################################################################
##### This automates the configuration of Nginx to act as a load balancer
##### Usage: The script is called with 3 command line arguments. The public IP of the EC2 instance where Nginx is installed
##### the webserver urls for which the load balancer distributes traffic. An example of how to call the script is shown below:
##### ./configure_nginx_loadbalancer.sh PUBLIC_IP Webserver-1 Webserver-2
#####  ./configure_nginx_loadbalancer.sh 127.0.0.1 192.2.4.6:8000  192.32.5.8:8000
############################################################################################################# 

PUBLIC_IP=$1
firstWebserver=$2
secondWebserver=$3

[ -z "${PUBLIC_IP}" ] && echo "Please pass the Public IP of your EC2 instance as the argument to the script" && exit 1

[ -z "${firstWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the second argument to the script" && exit 1

[ -z "${secondWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the third argument to the script" && exit 1

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure


sudo apt update -y && sudo apt install nginx -y
sudo systemctl status nginx

if [[ $? -eq 0 ]]; then
    sudo touch /etc/nginx/conf.d/loadbalancer.conf

    sudo chmod 777 /etc/nginx/conf.d/loadbalancer.conf
    sudo chmod 777 -R /etc/nginx/

    
    echo " upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server  "${firstWebserver}"; # public IP and port for webserser 1
            server "${secondWebserver}"; # public IP and port for webserver 2

            }

           server {
            listen 80;
            server_name "${PUBLIC_IP}";

            location / {
                proxy_pass http://backend_servers;   
            }
    } " > /etc/nginx/conf.d/loadbalancer.conf
fi

sudo nginx -t

sudo systemctl restart nginx
!<img width="757" alt="4_ sudo vi install_nginx" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/1715a9a8-c179-4056-9bd6-6af3bbf8f7fe">
<img width="727" alt="5_  install_nginx" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b8d83e2d-0b2d-41cb-b457-8460edc4589f">


## ## change file permission

    sudo chmod +x nginx.sh
## run the script on for nginx
./nginx.sh 3.12.147.187 52.14.222.39 :8000

<img width="616" alt="6_Screenshot for web server1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/3f672b12-f588-4ea3-bfdf-5fd2aa9eb518">
<img width="741" alt="6_Screenshot for web server2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/efc09ec0-46fe-4cf4-93e7-5a874d83cdb0">









    



























