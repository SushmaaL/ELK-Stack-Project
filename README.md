## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![RedTeamDiagram](https://github.com/SushmaaL/ELK-Stack-Project/blob/main/Diagram/RedTeamDiagram.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook/YAML files may be used to install only certain pieces of it, such as Filebeat.

[List of all the Playbook Files](Ansible)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting any unauthorized access to the network. 

  - It is an extra layer of security which aids in protecting websites from being hacked. Load balancers are a device that helps control the traffic that is being distributed across several servers and it also helps in preventing the occurence of distributed denial-of-service (DDos) attacks. 
  
  - This is why creating the Jump Box was crucial as it is the only area which provides full access for running administrative tasks across the network, making it a secure admin workstation. Since it is the only access point to modify or change anything in the other VM's that are a part of the resource group, creating this is a huge advantage to have as it would protect the other VM's from being hacked.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.

- Filebeat watches for log files and/ or locations that are specified by the user, collects those log events and then forwards them to Elasticsearch or Logstash to get documented.

- Metricbeat records both the metrics and statistics from systems which are running on the servers and then sends them to the specified output by the user and then forwards them to Elasticsearch or Logstash to get documented.


The configuration details of each machine may be found below.

| Name        | Function     | IP Address  | Operating System |
|-------------|--------------|------------ |------------------|
| Jump-Box-VM | Gateway      | 10.0.0.10   | Linux (Ubuntu)   |
| Web-1       | Server       | 10.0.0.8    | Linux (Ubuntu)   |
| Web-2       | Server       | 10.0.0.9    | Linux (Ubuntu)   |
| ELK-Stack-VM| Log Server   | 10.1.0.4    | Linux (Ubuntu)   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Jump Box VM's Whitelisted IP: 138.91.190.74 
  - Using VM's Public IP address

Machines within the network can only be accessed by Jump Box Provisioner.
- The Jump Box Provisioner will allow access to the ELK Server through SSH.

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Addresses          |
|-------------|---------------------|-------------------------------|
| Jump-Box-VM | Yes                 | 138.91.190.74                 |
| Web-1       | No                  | 10.0.0.10                     |
| Web-2       | No                  | 10.0.0.10                     |
| ELK-Stack-VM| No                  | 10.1.0.10                     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because 
you are able to duplicate or add any changes to any of the newly created VM's to run exaclty like the existing one and you can go through multiple servers easily as the installation process is fairly simple to setup and this also prevents any vulnerabilites that could have been overlooked during the process.

The playbook implements the following tasks:

- Install docker.io 
  - to run the containers
- Install phython3.pip
  - to install the Python software
- Install the docker python module
  - to control the state of the Docker containers 
- Download and lauch the docker DWVA web container
  - to be able to start the container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![DockerPS](https://github.com/SushmaaL/ELK-Stack-Project/blob/main/Images/DockerPSOutput.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- Web 1 Server: 10.0.0.8
- Web 2 Server: 10.0.0.9 

We have installed the following Beats on these machines:

- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat - collects the log files, directories and/or events that occur and then forwards them to a specifeid output. 
  - [Example](Images/Filebeat.png) 
- Metricbeat - collects the metrics and statistics from the currently running servers  and then forwards them to a specifeid output.
  - [Example](Images/MetricbeatExample.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install_elk_yml file to /etc/ansible directory.
- Update the hosts file to include the private IP address of Web 1, Web 2 and the ELK server.
- Run the playbook, and navigate to the ELK server to check that the installation worked as expected.

- The filebeat-playbook.yml is the playbook file and it is copied to /etc/ansible/roles/.

- In order to make the Ansible run the playbook on a specific machine, the host file needs to be updated. You would specify each machine to install on the ELK server vs the Filebeat by adding the IP addresses of the machines in the correct hosts of the Ansible file.

- You would navigate to this URL to check that the ELK server is rnning: http://[your.VM.IP]:5601/app/kibana - use the public IP address of the ELK server


These are the following commands the user will need to run to download the playbook:

- Command to log in to the environment: ssh azureuser@(JumpBoxPublicIP)
- Command to download playbook: ansible-playbook (name of file.yml)
- Command to update file: nano (name of file)
- Command to verify the container is on: sudo docker container list -a
  - Command if container is not on: sudo docker start (docker ID)
  - Command to attach the container to the VM: sudo docker attach (docker ID)
- Command to create and/or edit the files/hosts: nano files/hosts
- Command to access the files directory in ansible: cd /etc/ansible/files
- Command to acced the roles directory in ansible: cd /etc/ansible/roles
- Commands to update the files and all servers:
  - ansible-playbook elk-playbook.yml
  - ansible-playbook filebeat-playbook.yml
  - ansible-playbook metricbeat-playbook.yml
- Website to ensure all the files are being recieved: http://[your.VM.IP]:5601/app/kibana
