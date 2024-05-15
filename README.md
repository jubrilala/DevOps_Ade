# ANSIBLE REFACTOR PROJECT
# create this new directory on jenkins-ansible server to store all artifacts after build
sudo mkdir /home/ubuntu/ansible-config-artifact

# Change permission on the directory
chmod 777

change ownership on the file
chown -R 777 /home/ubuntu/ansible-config-artifact
<img width="923" alt="6_change ownership of the file" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/6b6f53d9-2d03-4b0f-b57e-7f96e93d4905">

<img width="891" alt="2_mkdir and change permission" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/1eeb5951-cc08-4457-8f11-7242a98f9ec6">

# install copy artifact plugin

<img width="946" alt="3_installing copt artefact plugin" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/0eed4fc6-0ad0-48c2-a299-cea5dc625024">

# create new Freestyle project and name it save_artifact 
 it will be triggered with existing AdeFreestyle project
 configure save_artifacts project accordingly with required no of build and other values
<img width="929" alt="3_create a new freestlyle project called save_artifacts" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/cf314435-cf7a-4c5a-8166-d56ae9d4c34d">
<img width="925" alt="4_create the build for save_artifacts" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/d6f9035e-5059-4937-ad5f-387d38735c61">


# Test the set up by making changes in the README.MD file in the local repo ansible-config-mgt
#confirmed that the build was trigered and the files were copied from the first job
<img width="947" alt="5_make changes in readme file and add, and commit and push" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/8fdcec3b-5093-4110-8a6f-6e05222ca71d">
<img width="951" alt="7_check that the 2 build was trigered" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b86f2806-8e63-4cdd-b47a-d70e42936c43">
<img width="752" alt="8_confirmed that the files are copied from the first job" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/e2f8db0b-5e2f-4f77-918c-67a012244655">


# Refactor ansible code by importing playbooks into site.yml
 on the local repo create site.yml within the playbook 
create other folders etc
---
- hosts: all
- import_playbook: ../static-assignments/common.yml

check the file structure
├── static-assignments
│   └── common.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
└── playbooks
    └── site.yml


create common-del.yml and paste below

---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    yum:
      name: wireshark
      state: removed

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    apt:
      name: wireshark-qt
      state: absent
      autoremove: yes
      purge: yes
      autoclean: yes
<img width="959" alt="9_create files and folders_site yml etc" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/47cc254e-8262-49fc-b826-451a60aac0a3">
<img width="783" alt="10_create common-del yml" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ff44373c-a019-4e18-b26e-3ebc1a985d86">
<img width="761" alt="11_update site yml" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/6d73ee82-f67c-4690-a6e6-fa1c62b4b195">

# run ansible-playbook command against dev environment
cd /home/ubuntu/ansible-config-mgt/
ansible-playbook -i inventory/dev.yml playbooks/site.yml
for the ssh agent
open git bash terminal and locate download folder

connect with  ssh agent
ssh -A ubuntu@18.217.175.55

<img width="934" alt="12_ansible playbook error due to no ssh agent" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/43eea319-7733-46ca-9a99-d5f9acbbaeb9">
<img width="940" alt="13_connect with ssh agent_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/c3701e1f-7b13-4901-b1cd-54994b28f90c">
<img width="821" alt="13_connect with ssh agent_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/77132433-3345-4f94-81b0-a703f7e0f1c7">
<img width="955" alt="14_playbook run successful with 1 error" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/e9c8f0b5-4eae-4915-877d-57dff943ea5b">

# configure UAT webserver with role webserver

launch 2 RED 8 web server
create role 
create directory called roles relative to the playbook in /etc/ansible

Create the directory/files structure manually to look like below

└── webserver
    ├── README.md
    ├── defaults
    │   └── main.yml
    ├── handlers
    │   └── main.yml
    ├── meta
    │   └── main.yml
    ├── tasks
    │   └── main.yml
    └── templates
<img width="955" alt="15_create the role structure" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/5e071a22-80b7-4312-926e-c9fa5d46fb1d">

# update your inventory
#update the /etc/ansible/ansible.cfg
#clone tooling website from github
[uat-webservers]
<Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

<img width="911" alt="16_web_uat servers_a" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/c4105253-a073-4b9d-a7c9-ca8efc2c8702">
<img width="950" alt="16_update the uat webservers IP address" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/a6d8dd6d-ebf7-4c46-85fb-594be9bc1196">
<img width="826" alt="17_sudo vi ansible_config file" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2cdd682e-3fb6-43d8-a6f2-289b83870e3a">

# within static assignment create uat-webservers.yml and paste below
---
- hosts: uat-webservers
  roles:
     - webserver

on the playbook, paste below in site.yml

---
- hosts: all
- import_playbook: ../static-assignments/common.yml

- hosts: uat-webservers
- import_playbook: ../static-assignments/uat-webservers.yml

<img width="826" alt="17_sudo vi ansible_config file" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/23d234d8-8437-4e71-950a-981ce7d1a2a1">
<img width="944" alt="18_paste into mail ym for task and change to the forked for github" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2e4d511c-2591-486e-9860-1ad7c00c677d">
<img width="902" alt="19_edit site yml" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/39e3da10-108c-491a-a60b-87ac491212c2">


# Run the playbook
cd /home/ubuntu/ansible-config-mgt

ansible-playbook -i /inventory/uat.yml playbooks/site.yaml
<img width="950" alt="20_connect with ssh agent" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/eddaf423-36fa-4558-a3fd-67d644074111">
<img width="945" alt="21_run playbook" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/0ec2616e-492e-48bb-a593-81782d1f91a8">




















 
 


