# Ansible-AWS-Nginx-Deployment
![Black Brown Geometric Motivational Desktop Wallpaper (1)](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/739fae6d-5a36-4946-9c6e-0a0a26cbac86)

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

### Screenshot
![Screenshot from 2024-02-22 10-40-50](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/44a3921e-f528-49df-b4ff-b7104dbde5e8)

### Step 9: Deploying Nginx on Dev Servers
Successfully installed and started Nginx on server-1 and server-2.

### Screenshot
![Screenshot from 2024-02-22 10-41-41](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/6e84b587-dff1-4555-9659-a797fdecb2d3)

![Screenshot from 2024-02-22 10-48-17](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/a8af580a-98df-4751-9269-e2ccd6e212bd)

![Screenshot from 2024-02-22 10-48-40](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/0b8cc112-dd56-4a29-9bd1-09f2b272e0dc)

### Step 10: Static Web Page Deployment on Production Server
Within the ansible_playbook directory, created index.html and a playbook named static_page.yml for installing Nginx, starting it, and serving a static web page on server-3.
```yaml
-
  name: Install Nginx and Serve Static Website
  hosts: production-env
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
    - name: Deploy Static WebPage
      copy:
        src: index.html
        dest: /var/www/html/
```

```html
<!-- Contents of index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ansible for DevOps</title>
<style>
    body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 0;
        background-color: #f0f0f0;
    }
    .container {
        max-width: 800px;
        margin: auto;
        background: white;
        padding: 20px;
    }
    h1, h2 {
        color: #333;
    }
    p {
        margin: 10px 0;
        line-height: 1.6;
    }
    .fade-in {
        animation: fadeInAnimation ease 3s;
        animation-iteration-count: 1;
        animation-fill-mode: forwards;
    }
    @keyframes fadeInAnimation {
        0% {
            opacity: 0;
        }
        100% {
            opacity: 1;
        }
    }
</style>
</head>
<body>
<div class="container fade-in">
    <h1>Ansible for DevOps</h1>
    
    <h2>Use Cases</h2>
    <p>Ansible can be used for configuration management, application deployment, task automation, and IT orchestration.</p>
    
    <h2>Principles of Ansible</h2>
    <p>Ansible operates on an agentless architecture, leveraging SSH for communication. Its simplicity, repeatability, and security are core principles.</p>
    
    <h2>Why Use Ansible?</h2>
    <p>Ansible is easy to learn and use, does not require special agent software to be installed on nodes, and can manage complex deployments and configurations.</p>
    
    <h2>Advantages</h2>
    <p>Advantages include its agentless architecture, flexibility, scalability, and the vast community support providing modules for various integrations.</p>
    
    <h2>Architecture</h2>
    <p>Ansible uses a simple architecture with Playbooks written in YAML, which describe the automation jobs, and an inventory file that lists the nodes to be managed.</p>
</div>
<script>
    // Example JavaScript for interaction or more complex animations
    document.addEventListener('DOMContentLoaded', function() {
        // Simple interaction or more complex animations can go here
    });
</script>
</body>
</html>
```
### Screenshot
![Screenshot from 2024-02-22 11-15-11](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/3cebc496-ab02-42fa-bb99-b31fd0ed0c1d)

![Screenshot from 2024-02-22 11-11-17](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/aa468e8c-3b05-4007-8d91-0d1cc2df48db)

### Step 11: Successful Deployment
Successfully deployed the static page on server-3.

### Screenshot
![Screenshot from 2024-02-22 11-14-48](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/45951c11-dc9d-4f23-acd1-04a061064f7a)

![Screenshot from 2024-02-22 11-17-01](https://github.com/DeoreRohit4/Ansible-Project-on-AWS/assets/102886808/8d8fac6d-ed2d-48a9-9853-de0575951b29)

## Conclusion
This project illustrates the power of Ansible for automating deployment tasks in a cloud environment, simplifying the management of multiple servers and the deployment of services like Nginx.
