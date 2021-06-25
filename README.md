# Cybersecurity-Bootcamp
This repository contains all projects, files, and diagrams that I created during my time within the Ohio State University College of Engineering's Cybersecurity Bootcamp 

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/r0tas/Cybersecurity-Bootcamp/blob/main/Images/Project1Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ELK playbook file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting unauthorized access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log data and system metrics.

The configuration details of each machine may be found below.

| Name       | Function   | IP Address | Operating System |
|------------|------------|------------|------------------|
| Jump Box   | Gateway    | 10.0.0.4   | Linux            |
| Web-1 VM   | Web Server | 10.0.0.7   | Linux            |
| Web-2 VM   | Web Server | 10.0.0.8   | Linux            |
| ELK Server | ELK Server | 10.1.0.9   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

2603:6011:2f07:478c:5493:ce18:72d6:4da1 (Local Machine)

Machines within the network can only be accessed by SSH from the JumpBox Provisioner.

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible  | Allowed IP Addresses     |
|------------|----------------------|--------------------------|
| Jump Box   | Yes w/ SSH and white | Local Machine IP Address |                |            | listed IP address    |                          |
| Web-1 VM   | No                   | Jump Box IP 10.0.0.4     |
| Web-2 VM   | No                   | Jump Box IP 10.0.0.4     |
| ELK Server | No                   | Jump Box IP 10.0.0.4     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because manual configuration is often susceptible to human error. Ansible greatly simplifies this process and allows for the efficient configuration of multiple machines, reducing time and costs.

The playbook implements the following tasks:
- Designates hosts and users
- Installs Docker
- Installs pip3
- Installs Docker Python module
- Allocates more memory to the machines
- Downloads and launches a Docker ELK container with images and published ports
- Boots Docker

![ELK Playbook](https://github.com/r0tas/Cybersecurity-Bootcamp/blob/main/Images/ELK_YML_Playbook.JPG)

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![List ELK Containers](https://github.com/r0tas/Cybersecurity-Bootcamp/blob/main/Images/ELK_docker_ps.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 VM 10.0.0.7
- Web-2 VM 10.0.0.8

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects log events, monitors log files or locations, and forwards them to Elasticsearch or Logstash for indexing. Metricbeat will systematically collect metrics from the operating system and servers running and forward them to Elasticsearch or Logstash. Metricbeat monitors services such as Apache, MySQL, or Nginx, as well as others.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the installation playbook file to a .yml file in the /etc/ansible directory inside the ansible container.

- Update the hosts file in the ansible directory to include the correct hosts and groups; don't forget to add "ansible_python_interpreter=/usr/bin/python3" after the IP address 

![Ansible hosts file](https://github.com/r0tas/Cybersecurity-Bootcamp/blob/main/Images/Ansible_Hosts.JPG)

- Run the playbook, and navigate to http://[YourELKServerIP]:5601/app/kibana (in this case, the ELK server IP is 13.64.101.168, so the full link would be http://13.64.101.168:5601/app/kibana) to check that the installation worked as expected.


### Commands

To generate an SSH key: 

- $ ssh-keygen 
- If generating a key inside of an Ansible container for a webserver or other machine inside of the network, do not set a passphrase in order to enable automation of configuration 

To SSH into the JumpBox Provisioner VM from local machine: 

- $ ssh -i (full path to the private ssh key used when setting up the VMs in the Azure portal) username@ip_of_jumpbox 
  - Enter passphrase for ssh key when prompted 
  - This will take the user into the JumpBox
  - Example command: ssh -i ~/.ssh/id_rsa azadmin@52.152.235.68 

To install Docker in the JumpBox: 

- $ sudo apt install docker.io 

To pull Ansible image: 

- $ sudo docker pull cyberxsecurity/ansible 

To see what images are pulled: 

- $ sudo docker images 

To create a container: 

- $ sudo docker run -ti --name (name of container) (image) bash 
  - Can use any unique name, and include an image that has already been pulled, such as cyberxsecurity/ansible 

To start a container: 

- $ sudo docker start (name of container) 
  - Only start a container once if it is not running, otherwise it will create a new container each time it is started 

To get into a container: 

- $ sudo docker exec -ti (name of container) bash 
  - There are other commands that will do the same thing 

To see what Docker containers are available: 

- $ sudo docker container ls -a 
  - This will list all the containers with their Container ID, Image, Command, Creation Date, Status, Ports, and Names 

To run an Ansible playbook ./yml file: 

- $ ansible-playbook elk.yml 
  - Use for any Ansible playbook file 
