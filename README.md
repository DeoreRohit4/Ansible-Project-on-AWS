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


