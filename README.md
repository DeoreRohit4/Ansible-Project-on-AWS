# Ansible Project on AWS: Automated Nginx Deployment

## Project Overview
This project demonstrates the use of Ansible for automating the deployment of Nginx on AWS EC2 instances. The setup includes one Ansible master server and three target servers. Nginx was installed and started on two development servers (server-1 and server-2), and on the third server (server-3), designated as the production server, Nginx was installed, started, and used to serve a static web page.

## Tools Used
- AWS EC2
- Ansible
- SSH
- SCP

## Steps

### Step 1: EC2 Initialization
Launched an EC2 instance as the ansible-master-server. Then, launched three additional EC2 instances using the same key-pair as the master server and named them server-1, server-2, and server-3.

### Screenshot
![Screenshot from 2024-02-22 10-45-43](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/9040fb07-13eb-4f29-8e6e-0556eed675d3)

### Step 2: Ansible Installation on the Master Server
Connected to the ansible-master-server via SSH from a local machine and installed Ansible using the following commands:
```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```
### Screenshot
![Screenshot from 2024-02-21 19-03-38](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/d3b2cefe-2155-4425-9cfc-8c0f08421573)

![Screenshot from 2024-02-21 19-04-48](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/dbebb90e-2b21-4cd8-b494-1a1af4f847de)

### Step 3: Key Transfer
Copied the private key from the local machine to the ansible-master-server using the SCP command:
```bash
scp -i "ansible-master-key.pem" ansible-master-key.pem ubuntu@ec2-3-83-240-22.compute-1.amazonaws.com:/home/ubuntu/keys
```
### Screenshot
![Screenshot from 2024-02-21 19-24-47](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/c631854d-b3da-448b-87e4-6f974449b630)

![Screenshot from 2024-02-21 19-25-24](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/99bac3ff-4f03-4a20-acac-291fab25d1e4)

### Step 4: Hosts Configuration
Configured the /etc/ansible/hosts file to include the three servers, adding them under a specific variable for easier management.

### Screenshot
![Screenshot from 2024-02-22 09-51-56](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/993d4207-ae73-48cf-918b-335d96790fd1)

### Step 5: Testing Connections
Verified connectivity to all servers from the ansible-master-server using the command:
```bash
ansible servers -m ping
```

### Screenshot
![Screenshot from 2024-02-22 09-56-01](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/e091d320-5a12-4c5a-8c9b-f4cb388707f5)

### Step 6: Updating Servers
Executed an ad-hoc command to update all servers:
```bash
ansible servers -a "sudo apt update"
```
### Screenshot
![Screenshot from 2024-02-22 09-59-21](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/3b1ccd8c-6302-43ea-989f-eead9b5bcf93)

### Step 7: Environment Grouping
In /etc/ansible/hosts, grouped server-1 and server-2 under dev-env, and server-3 under production-env. Set variables accordingly.

### Screenshot
![Screenshot from 2024-02-22 10-21-28](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/d29f4564-5253-4eab-a572-635dbb2bd732)

### Step 8: Nginx Installation Playbook for Dev Servers
Created an ansible_playbook directory and wrote a playbook named install_nginx.yml to install and start Nginx on server-1 and server-2.
```yaml
-
  name: Install and Start Nginx
  hosts: dev-env
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: latest
    - name: Start Nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

