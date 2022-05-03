## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.


![CloudDiagram drawio](https://user-images.githubusercontent.com/77586504/166480640-f6ac5dc8-98c6-40db-84dd-86a2f5eec887.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above.



This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- The primary function of a load balancer is to spread workloads across multiple servers to prevent overloading servers, optimize productivity, and maximize uptime. Load balancers also add resiliency by rerouting live traffic from one server to another if a server falls prey to DDoS attacks or otherwise becomes unavailable. In this way, load balancers help to eliminate single points of failure, reduce the attack surface, and make it harder to exhaust resources and saturate links.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
-Filebeat watches for log events and gather data about the file system.
-Metricbeat Monitors and Collect metrics from the system and services running on each server.

The configuration details of each machine may be found below.


| Name              	| Function           	| IP Address  	| Operating System       	|
|-------------------	|--------------------	|-------------	|------------------------	|
| --------          	| -------            	| ---------   	| ----------------       	|
| Jump Box          	| Gateway            	| 10.0.0.4    	| Linux Ubuntu 18.04-LTS 	|
| Local Workstation 	| External Network   	| 108.2.171.3 	| Windows                	|
| Web 1             	| Web Content        	| 10.0.0.5    	| Linux Ubuntu 18.04-LTS 	|
| Web 2             	| Web Content        	| 10.0.0.6    	| Linux Ubuntu 18.04-LTS 	|
| Elk-Server        	| Process Logs       	| 10.2.0.4    	| Linux Ubuntu 18.04-LTS 	|
| Load Balancer     	| Distribute Traffic 	|             	| None                   	|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
Machines within the network can only be accessed by JumpBox Provisioner 10.0.0.4


A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Addresses         |
|-------------|---------------------|------------------------------|
| Jump Box    | Yes                 | Local Workstation ssh port 22|
| Web 1       | No                  | 10.0.0.5 SSH 22              |
| Web 2       | No                  | 10.0.0.6 SSH 22              |
| Elk VM      | Only on Port 5601   | 10.0.0.5 10.0.0.6 Workstation|
| LoadBalancer| Yes                 | Workstation HTTP port 80     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible is faster and less error prone.
The main advantage of automating configuration with Ansible is that it saves time, and as sysadmins no longer have to spend a lot of time configuring machines manually. Ansbile allows repition and less bugs as the multiple human error aspect is no longer in affect.

The playbook implements the following tasks:
Install Docker to set up a container to house the ELK stack
Increase virtual memory to accomodate the resource intensive ELK stack
Download the image (sebp/elk:761) and start the container
Enable docker container to start on VM boot

sudo apt update (ensure system is updated)
sudo apt install -y docker.io (bring in docker)
sudo systemctl status docker (verify docker is running)
sudo docker pull cyberxsecurity/ansible:latest (download container)
sudo docker container list -a (list all installed docker containers)

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
See Images.


![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png) See Images


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
10.0.0.5
10.0.0.6

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
File Beat
Metric Beat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible/files/filebeat-config.yml
- Update the filebeat-config.yml file to include... installation path, username/pass, private ip of elk server
- Run the playbook, and navigate to  http://[public IP address of ELK Server]:5601/app/kibana to check that the installation worked as expected. o confirm that the ELK server is receiving logs from Web-1 and Web-2 you will navigate from within the Kibana GUI to Add Log Data --> System Logs --> DEB tab --> Step 5: Module Status --> Check Data.


_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_ filebeat-playbook.yml. It will be copied in the /etc/ansible/roles/filebeat-playbook.yml directory.
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ In Hosts file Add 
[elk] under [webservers]
- _Which URL do you navigate to in order to check that the ELK server is running?
http://[public IP address of ELK Server]:5601/app/kibana
_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
ssh azureuser[or chosen name]@jump-box-ip-address

sudo docker pull [name of container] to download container

sudo docker run -ti [name of container] bash

sudo docker start [name of container] to start the container

sudo docker attach [to connect to the Ansible container]

Navigate to the /etc/ansible directory

nano hosts to configure the IP addresses within the "webserver" and "elk" groups

nano ansible.cfg to specify the remote user.

ansible-playbook [name of playbook]
