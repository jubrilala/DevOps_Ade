# Nginx as a load balancer Project
## Launch 2 ubuntu server 
<img width="905" alt="1_launch 2 ubuntu server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/498c76ff-8fd4-42c1-a1aa-f0341cd624e9">

##  Install apache2
sudo apt update -y &&  sudo apt install apache2 -y
<img width="908" alt="3_apt install apache2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/3b67f464-bb64-4368-90bf-b26baefe2c79">
<img width="869" alt="2_apt update" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2608a4bd-7b28-4a4d-8d36-5f38ab00fad8">
## verify that apache2 is running
sudo systemctl status apache2
<img width="586" alt="4_verify apache2 running" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ce914b6d-c7c4-42a7-b439-6b66ec5e12f5">

## configure apache to serve content on port 8000
sudo vi /etc/apache2/ports.conf 
sudo vi /etc/apache2/sites-available/000-default.conf
<img width="605" alt="6_change virtual port from 80 to 8000" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/5959b469-8bc2-4085-ab69-1f3bd81c2e6b">
<img width="509" alt="5_add listen port 8000" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/f24a7b33-09fc-41d6-a255-1aacb1b39937">

##  restart appache to load new config
sudo systemctl restart apache2
<img width="481" alt="7_restart apache2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/51ac36fb-d35d-40fe-ae16-020772c44007">

## create a new html file
sudo vi index.html

        <!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: 3.139.108.200</p>
        </body>
        </html>
<img width="494" alt="8_edit the html file with ec2 public IP" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/16f882d3-5e49-45a2-a945-e2a3d69e8ae5">

## change file ownership of index.html file
sudo chown www-data:www-data ./index.html
replace html file with new htlm file
sudo cp -f ./index.html /var/www/html/index.html
<img width="558" alt="9_chnage html file ownership, replace with new file and restart apache2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/67ca679a-936f-4a66-9f25-225cadc22595">
## restart appache2
sudo systemctl restart apache2

##  verify that the apache2 is serving
<img width="512" alt="10_check that your page work" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/75bf42eb-2411-4980-a76b-ebd07f7e6332">

## launch ubuntu server for nignix
<img width="895" alt="11_ubuntu Nginix server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/c264f9cc-5ecf-471a-b117-b9138ebaf241">

## install nginix
sudo apt update -y && sudo apt install nginx -y
<img width="724" alt="12_update and install nginix" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/6c636968-0651-471c-a06d-e3d72f95ea8e">

## verify nginix is installed
sudo systemctl status nginx
<img width="805" alt="13_verify nginix is installed" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/42354d2e-261f-42ea-a1ed-28a951278fe4">

## open nginix config with cmd below
sudo vi /etc/nginx/conf.d/loadbalancer.conf
edit config file with below and insert the IPs
        
        upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server 127.0.0.1:8000; # public IP and port for webserser 1
            server 127.0.0.1:8000; # public IP and port for webserver 2

        }

        server {
            listen 80;
            server_name <your load balancer's public IP addres>; # provide your load balancers public IP address

            location / {
                proxy_pass http://backend_servers;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }

<img width="494" alt="14_edit load balancer config file" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/e1b1ae90-3f51-4883-baad-32f9d2ed8226">

##  test your configuration with cmd below
sudo nginx -t
## restart nginix
sudo systemctl restart nginx

<img width="494" alt="15_test configuration and restart nginix" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/769801e0-8bf0-4049-b1bb-25c6ecc99213">

# check that the page is serve
<img width="512" alt="10_check that your page work" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2b93aca6-f750-422c-a4e9-610300c440aa">























