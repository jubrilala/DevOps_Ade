# Asible Automate project
## update jenkins ec2 name Jenkins-Ansible 
<img width="914" alt="1_jenkins-ansible ec2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/d0176f8a-2be3-407e-bd9b-0a5d2c4654b1">

 ## create a repository Ansible-Config-Mgt on github
 <img width="957" alt="1a_create ansible-config-magt repo on github" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/24d248c9-39aa-4d24-9a45-5e81749de6ec">

## Install ansible on Jenkins-Ansible ec2
sudo apt update
sudo apt install ansible -y
<img width="705" alt="3_check ansible version" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/6a2902d0-d2cc-43dc-a337-272456799da8">
<img width="796" alt="2_install ansible on ec2 server" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/8f7f8abb-0a3f-45be-a55c-8c5281f7707a">

## Configure jenkin build job to archive your repository content
create a freestyle project in Jenkins and point to ansible-config-mgt repo
<img width="957" alt="4_create new freestyle" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/062f90ad-692a-4f87-aa6d-9484e80978f9">

Configure webhook in github and point it to ansible-config-mgt repo
<img width="938" alt="5_configure webhook" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/7005abee-376a-4736-804d-1c56073f9600">
configure a post-build job to save all ** files 
<img width="940" alt="6_configure a post build job" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/0306f941-bf4d-47e4-a974-08eceb2c75e1">

## test set up by making changes in READ.ME file
ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
/var/lib/jenkins/jobs/AdeFreestyle/builds/2/archive
cat /var/lib/jenkins/jobs/AdeFreestyle/builds/2/archive/README.md
ansible-config-mgt
my first repo cloning
<img width="941" alt="7_make changes on readme" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/d3e2ae34-188d-4197-a7cf-1c59992e943f">
<img width="940" alt="8_check changes on github" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/6f4cdb67-e708-4bb3-bd85-f9f8982c93fb">
<img width="918" alt="9_check build on jenkins" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/90499eb4-271f-4f04-895f-1fedd73cb282">

## Begin ansible development 

git clone <ansible-config-mgt repo link>
in ansible-config-mgt create branch and name feature
<img width="944" alt="10_create a new branch_feature-ansible" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2c468d41-c5e6-4226-84c6-e9a586bafa19">
checkout the feature branch and start building code in directory structure
create a directory and name it playbook
create a directory and name it inventory
within playbook create common.yml folder
with inventory create dev,staging,prod and uat inventory files
<img width="944" alt="13_create common playbook_common yml" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/e5401ccd-2bcf-40f3-9f3d-2fcbc9a491bb">
<img width="955" alt="12_create ansible_inventory_update_inventory_dev yml" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2b6805a6-12cb-4e19-86e4-09f81e297ccc">
<img width="888" alt="11_create directories" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ba61d09e-67d1-47fc-bf6b-16f211f98ffa">
<img width="938" alt="14_check git status after config of files in ansible_config_mgt" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/a9c0459c-4df8-4daa-84f9-7e00d2e88dc8">

## set up ansible inventory
update /inventory.yml 
 create ansible inventory 
[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user=ec2-user  172.31.12.198

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user=ec2-user  172.31.13.176
<Web-Server2-Private-IP-Address> ansible_ssh_user=ec2-user  172.31.7.246

[db]
<Database-Private-IP-Address> ansible_ssh_user=ec2-user 172.31.10.247

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user=ubuntu   172.31.15.156

<img width="955" alt="12_create ansible_inventory_update_inventory_dev yml" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/e652432d-6f97-420a-8f9d-84818741f875">

## create common playbook in common.yml file

---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  become: yes
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest
   

- name: update LB server
  hosts: lb
  become: yes
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest

<img width="944" alt="13_create common playbook_common yml" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/9b6bc2f2-8954-4b47-9fbc-ed6737d0a6c1">

# update your code main branch-make change in new branch-push to github, merge to main branch,checkout out from main branch-do git push
<img width="938" alt="14_check git status after config of files in ansible_config_mgt" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ffec156d-4575-4722-a743-da12ce2279ec">
<img width="941" alt="15_git commit" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/aea75271-566f-48ae-bf02-3f75293a73c9">
<img width="941" alt="16_git push" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/5387e123-5f9a-4917-a7ad-61099f21c6d2">

## Push on the github to merge the main and the branch
<img width="936" alt="17_push_merge request" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/c41b5cc5-2a19-4e9b-8883-00a4d9c07d0b">
<img width="957" alt="18_push_merge request_successful" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b494dac4-1465-436b-be74-50df0cb15153">
<img width="943" alt="19_main_feauture branch merged" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/12032283-850e-4a51-a2b6-ee4e6d7ec575">

## check on the jenkins that the build is triger
<img width="957" alt="20_successful buid job on jenkins" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2f1d6018-4855-4851-b35d-6aded5325b00">

## Update on the local repo the config 
run git pull
<img width="947" alt="21_checking branch on local machine not updated" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/2297ede3-1aa7-44b0-bf31-e41e416fb6a2">
<img width="953" alt="22_git pull to update files on local repo" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/e935147b-d5e3-4067-b581-afd097f8fbb1">

## run ansible cmd with ssh agent
ansible-playbook -i inventory/dev.yml playbooks/common.yml
ssh agent set up
eval `ssh-agent -s`
ssh-add <path-to-private-key>

!! ssh using ssh agent
ssh -A ubuntu@public-ip
ssh -A ubuntu@13.59.10.120

<img width="959" alt="23_run ansible playbook" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/3697a347-23d0-457a-84b5-bd06d4ca1857">
<img width="952" alt="24_check ssh key" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/35083808-557e-49d3-b3e8-15d994b1e6ef">
<img width="954" alt="24_check ssh key_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/17518929-4f3a-4fd4-a86d-b7a0a77d93fa">
<img width="789" alt="25_connect with ssh agent" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/d614ed24-faa3-42d3-9efb-67034a45f494">

## check that the playbook run successfully
<img width="948" alt="26_ansible playbook succesful" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/02dccb62-0071-40c1-b8c2-eb38e761ee1c">










 
 


