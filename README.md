## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Project Diagram](https://github.com/slyhy/azure_cloud_deployment/blob/master/Images/Project%20Diagram.png)

[These files](https://github.com/slyhy/azure_cloud_deployment/tree/master/ansible_playbooks) have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the beat_install.yml file may be used to install only certain pieces of it, such as Filebeat.

  - [beat_install.yml](https://github.com/slyhy/azure_cloud_deployment/blob/master/ansible_playbooks/beat_install.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly __stable__, in addition to restricting __traffic__ to the network.
- Load balancers focus on network stability by taking incoming network and distributing it among the avalable machines. 
- A jump box allows a point of access for a user, so that internal network access from the outside may be restricted.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the __logs__ and system __metrics__.
- Filebeat collects, parses and vizualizes logs from the system or other modules and forwards them to Kibana. 
- Metricbeat collects and ships different system and service metrics (e.g. CPU and memory usuage) and forwards them to Kibana.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Webserver| 10.0.0.5   | Linux w/DVWA container|
| Web-2    | Webserver| 10.0.0.6   | Linux w/DVWA container|
| ELK      | ELK Stack| 10.1.0.4   | Linux w/ELK container |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the __Jump Box__ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- My local IP

Machines within the network can only be accessed by __the Jump Box__.
- The Jump Box is the machine that was allowed to acces the ELK VM. It's IP address is: 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump-Box | Yes                 | Local IP             |
| Red_Team_LB | Yes              | All HTTP traffic over port 80|
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |
| ELK      | Yes                 | 10.0.0.4 and Local IP       |   

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- A properly configured playbook allows for the easy and correct deployment ELK machines. 

The playbook implements the following tasks:
- Installs docker.io and python3-pip using apt module
- Install docker module using pip module
- Inscrease virtual memory and configure system to increase virtal memory on restart.
- Download and launch ELK container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Images/docker_ps_output.png](https://github.com/slyhy/azure_cloud_deployment/blob/master/Images/ELK_Container.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5 - Web-1
- 10.0.0.6 - Web-2

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is used to collect system logs such as system.syslog which records system events. 
- Metricbeat is used to collect information on different system metrics like CPU usage.  

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the __playbook (e.g beat_install.yml for filebeat/metricbeat or elk_deployment.yml for elk)__ file to __/etc/ansible/roles/__.
- Update the __playbook__ file to include __host name where you want to install the playbook (e.g. weberservers or elk)__.
- Run the playbook, and navigate to __Kibana web app (ELK_VM_IP:5601/app/kibana)__ to check that the installation worked as expected.
