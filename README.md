# Cybersecurity_project_1## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/mjwhite316/Cybersecurity_project_1/blob/Images/project%201%20diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat_playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

  
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


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system metrics.


The configuration details of each machine may be found below.


| Name       | Function     | IP Address | Operating System   |
|------------|--------------|------------|--------------------|
| Jump Box   | Gateway      | 10.0.0.4   | Linux-ubuntu 18.04 |
| Web 1      | webserver    | 10.0.0.5   | Linux-ubuntu 18.04 |
| Web 2      | webserver    | 10.0.0.6   | Linux-ubuntu 18.04 |
| Web 3      | webserver    | 10.0.0.7   | Linux-ubuntu 18.04 |
| Elk-Server | Elk platform | 10.1.0.4   | Linux-ubuntu 18.04 |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
23.169.64.0/27 - home location, laptop travels, and doesn't always get the same ip upon return.
173.10.18.185 - work location

Machines within the network can only be accessed by Jump Box.


A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses             |
|------------|---------------------|----------------------------------|
| Jump Box   | Yes                 | 23.169.64.0/27 and 173.10.18.185 |
| Web 1      | No                  | 10.0.0.4                         |
| Web 2      | No                  | 10.0.0.4                         |
| Web 3      | No                  | 10.0.0.4                         |
| Elk-server | No                  | 10.0.0.4                         |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
every time a web server is added, or one needs to be reconfigured due to an attack, or corruption, the file just needs to have any pertinent updates, and be rerun.

The playbook implements the following tasks:
- Install docker.io
- Install python3-pip
- Install docker python module
- Download and launch a docker container
- Enable docker service

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/mjwhite316/Cybersecurity_project_1/blob/main/1-elk761.jpg

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5, 10.0.0.6, 10.0.0.7

We have installed the following Beats on these machines:
- I was able to install filebeat on the machines, but there was an error in the communication with the ELK box. Even though all of the connections between the machines were up,
and Azure's test connection function said that they should have been able to communicate.

Azure confirmation: https://github.com/mjwhite316/Cybersecurity_project_1/blob/main/connection%20allowed.jpg

These Beats allow us to collect the following information from each machine:
Filebeat is able to look at whatever log file locations you specify. Some of the most useful logs could be those indicating who is logging into different machines and when. 
Related to that, from a security perspective the failed logon attempts can help to determine if there is someone attempting to gain unauthorized access to the system. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install_elk.yml (included in repository) file to /etc/ansible/
- Copy the filebeat_playbook.yml (included in repository) file to /etc/ansible/roles/
- Make sure the directory /etc/ansible/files exists
- Update the /etc/ansible/hosts file to include the ip addresses of all the webservers and elk machine in the proper location (see example file in this repository).
- Retrieve a filebeat-config file. (one can be found at: https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat) 
place it in /etc/ansible/files/filebeat-config.yml and update it to include the appropriate ip addresses of the elk server machine under the filebeat and kibana sections.
- Run the playbook, and navigate to http://<elk_server_public_ip>/app/kibana to verify that kibana is up and running. 
- If executed properly you will see a loading page similar to this:
https://github.com/mjwhite316/Cybersecurity_project_1/blob/main/Kibana%20loading%20page.jpg


