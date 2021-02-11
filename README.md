# ELK-Stack
This repository will hold everything related to the ELK Stack Project

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/wildcatsr2us/ELK-Stack/blob/main/Peer%20Network%20Blue%20Team_ELK%20Diagram%20Network%20(1).jpg

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - The ansible-playbooks metricbeat-playbook.yml and the filebeat-playbook.yml

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

- What aspect of security do load balancers protect? What is the advantge of a jumpbox?
Load Balancing contributes to the Availability aspect of security in regards to the CIA Triad. It ensures availability and distributes traffic evenly across the network. The advantage of the Jumpbox is to have a starting point for launching administrative tasks, such as gateway to administrative containers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
-  What does Filebeat watch for?_ It collects data about the file system from remote machines.
-  What does Metricbeat record?_ It collects machine metrics, such as uptime.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function  | IP Address | Operating System |
|----------|---------- |------------|------------------|
| Jump Box | Gateway   | 10.0.0.4   |Linux Ubuntu 18.04|            
| Web1     |Docker-DVWA| 10.0.0.7   |Linux Ubuntu 18.04|
| Web2     |Docker-DVWA| 10.0.0.8   |Linux Ubuntu 18.04|
| Web3     |Docker-DVWA| 10.0.0.9   |Linux Ubuntu 18.04|
| ELK1 VM  | ELK VM    | 10.2.0.4   |Linux Ubuntu 18.04|
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP address

Machines within the network can only be accessed by Jumpbox.
-  The only machine that access the ELK VM (10.2.0.4) is from the Jumpbox (10.0.0.4).

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Personal IP          |
| Web1     | No                  | 10.0.0.4             |
| Web2     | No                  | 10.0.0.4             |
| Web3     | No                  | 10.0.0.4             |
| ELK1 VM  | No                  | 10.0.0.4             |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantages of automating configuration through Ansible is the ease of use and an extremely easy learning curve. Through the use of Playbooks you are able to configure multiple Machines through the use of a single command after initial configuration.

The playbook implements the following tasks:
- The playbook implements the following tasks:
- Create a New VM on your virtual network (something easy such as ELK VM) Keep a note of the Private IP (10.0.0.4) and the Public IP (0.0.0.0) you will need the Private IP to SSH into the VM and the Public IP to connect to the Kibana Portal (HTTP Site) to view all Metrics/Syslogs.
- Download and Configure the "elk-docker" container In the etc/ansible/hosts you will need to add a new group [elk] and the Private IP (10.0.0.4) to the group. Then you need to create a new ansible-playbook that will download, install, configures the "Elk-VM1" to map the following ports [5601,9200], and starts the container. This will be done in the /etc/ansible/files/filebeat-config.yml and metricbeat-config.yml files. Then find the sections output.elasticsearch and setup.kibana to amend the IP addresses to reflect your own.
- Launch and expose the container "After installing and starting the new container. You can verify that the container is up and running by SSHing into the container from your JumpBox. Once you are in the [Elk-Server] run the command [sudo docker ps]
- Create new Inbound Security Rules to allow Ports: 5601 and 9200 "The Inbound Security Rules should allow access from your Personal Network"
- Open a new browser and type in the [Public IP:5601] to access the Kibana Portal Site

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
-  10.2.0.4 ELK VM-1

We have installed the following Beats on these machines:
-  Filebeat & MetricBeat

These Beats allow us to collect the following information from each machine:
- Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events and forwards them either to Elasticsearch or Logstash for indexing. For example, the location for the binary files, found in usr/share/filebeat/bin. Metricbeat collects metrics from the operating system and from services running on the server. Metricbeat then takes the metrics and statistics that it collects and ships them to the output that you specify.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- - Copy the filebeat.yml file to the /etc/ansible/roles/files/ directory.
- Update the configuration file to include the Private IP address. This will be done in the /etc/ansible/files/filebeat-config.yml and metricbeat-config.yml files. Find the sections output.elasticsearch and setup.kibana to amend the IP addresses appropriately to reflect your own.
- Create a new playbook in the /etc/ansible/roles/ directory that will install, drop in the updated configuration file, enable and configure system module, run the filebeat setup, and start the filebeat service.
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.


_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_It's the filebeat-playbook.yml. It is located at /etc/ansible/roles/filebeat-playbook.yml
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? The file you need to update is the filebeat.yml file which is a configuration file that will be dropped into the Elk-VM1 during the run of the ansible-playbook. When you update the hosts file in the ansible directory you will need to create a new group called [elk] and add the Private IP of the Elk-VM1 to the group elk. After this, you will need to nano into your filebeat.yml to configure to represent the private IP of the ELK-VM1 as well.
- _Which URL do you navigate to in order to check that the ELK server is running?The URL to use to verify the Elk-Server is running is the Public IP (YourELKVMPublicIP:5601/app/kibana)

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

The commands you will need to run the playbook’s are:
- ssh azureuser@JumpBox(10.0.0.4)     
- sudo docker container list -a
          (locate your ansible container, for our network the container name is 21a4a529d707)
- sudo docker start [21a4a529d707]
- sudo docker attach [21a4a529d707]
- cd /etc/ansible/
- install-elk.yml (configures ElkVM1 and starts the elk container) shouldn’t take more than a few minutes for the deployment/activation/implementation of the elk.
- cd /etc/ansible/roles/
- then run the command ‘ansible-playbook filebeat-playbook.yml’ (installs Filebeat)   for metric beat run ’ansible-playbook metricbeat-playbook.yml (installs Metricbeat)
- In a web browser tab navigate to this kibana web page with the following URL template.  http://40.122.200.90:5601/app/kibana   -40.122.200.90 being the private IP address of the ElkVM1-
-If succussfully deployed you should now be able to load your log data from the ansible containers you have deployed with ELk.
