## Cyber Security Project 1
Sarah Wilson
## Automated ELK Stack Deployment
All the files provided in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/Valethiel/cyber-security-portfolio/blob/8b40dbb67807cc46cf7c15d0c24d9e59a9975178/Diagrams/Network\_Diagram\_with\_ELK.png)

These files have been tested and used to generate a live ELK deployment on Microsoft Azure. They can be used to either recreate the entire deployment pictured above, or alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat or Metricbeat.

- [Install Elk](https://github.com/Valethiel/cyber-security-portfolio/blob/8b40dbb67807cc46cf7c15d0c24d9e59a9975178/Ansible/install-elk.yml.txt)

**This document contains the following details:**

- Description of the Topology
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build
### Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D\*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available and protected, in addition to restricting malicious access to the network.

**What does the Load Balancer Protect?**

- Load balancers protect the confidentiality portion of the CIA Triad. They provide us with a public IP address that’s used to mask the backend resources from direct or indirect internet access.

**What are the advantages of a jump box?**

- The advantage of a jump box is that it acts as a single gateway router between multiple VMs on a network. When we are only securing and monitoring this one single gateway, it allows for us to focus on one single and secure connection to the jump box, instead of having to focus on multiple connections between all the multiple virtual machines. The jump box is exposed to the public internet, and and it sits as a defense shield in front of other machines so that not exposed to the public internet. It also controls user access to the virtual machines by allowing only connections from specific IP addresses to flow, and then it forwards them to the back-end resources.

To make sure the jump box is secure, we need to:

- add two-factor authentication for the SSH login to access the jump box
- implement a log monitoring system on the jump box
- lock the root account for access
- limit the sudo access of the administrator account on the jump box
- limit the number of machines attached
- implement a firewall on the jump box
- limit the jump box network access within the VPN

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file system (Filebeat) and system metrics (Metricbeat).

- Filebeat collects log data and streams: including the VM, host, container, node, pod, and all of other metadata for computerized correlation. It detects new containers in the system and monitors them.
- Metricbeat collects memory, filesystems, system-level CPU usage, disk IO, network IO statistics, and statistics for literally every process running on the system.

The configuration details of each machine may be found below:

|Name|Function|IP Address|Operating System|
| :-: | :-: | :-: | :-: |
|Jump-Box-Provisioner|Gateway Router|10.0.0.4|Linux 1vCPUs 1GiB Ram|
|Web-1|Webserver|10.0.0.5|Linux 1vCPUs 2GiB Ram|
|Web-2|Webserver|10.0.0.6|Linux 1vCPUs 2GiB Ram|
|ELK-Server|Database for Network Security Monitoring|10.1.0.4|Linux 2vCPUs 8GiB Ram|

### Access Policies
The machines on the internal network are not exposed to the public Internet.

Only the jump-box-provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 52.243.92.23

Machines within the network can only be accessed by SSH (port 22) and HTTP (port 80).

- Jump-Box-Provisioner can access the ELK Server from it’s Public IP address 52.243.92.23

A summary of the access policies in place can be found in the table below.

|Name|Publicly Accessible|Allowed IP Addresses|
| :-: | :-: | :-: |
|Jump-Box-Provisioner|No|52.243.92.23|
|Web-1|Yes|10.0.0.4|
|Web-2|Yes|10.0.0.4|
|ELK-Server|No|52.243.92.23, 10.0.0.4|

### Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually.

**Advantage of Automating Configuration with Ansible:**

- The main advantage of automating configuration with Ansible is the ability to install software and change/update configuration files in an unlimited amount of servers within a short period of time.

The playbook implements the following tasks:

- *install docker.io* downloads the software package needed for the Docker program that is needed for running containers.
- *install python3-pip* downloads the software package needed to install Python software.
- *install docker module* initiates the python client for the Docker client, which is required for Ansible to be able to manage the state of Docker containers.
- *increase virtual memory* allows the ELK Server to allot more memory to the system to allow the ELK container to run smoother.
- *use more memory* uses Ansible's sysctl module to configure the increased virtual memory for every time the VM has been restarted in it’s history.
- *started*: starts the container.
- *enable service docker on boot* allows the docker service to start up automatically if you have to restart your ELK machine.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker PS Output](https://github.com/Valethiel/cyber-security-portfolio/blob/2e83e1c85ecf835c29b366e8538c0986a922ae2a/Images/docker\_ss.jpg)
### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- Web-1 10.0.0.5
- Web-2 10.0.0.6

We have installed the following Beats on these machines:

- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:

- The Filebeat module collects and parses logs created by the system logging service. You will see syslog events, hostnames, and processes.
- The Metricbeat module collects CPU, memory, network, and disk statistics from the host. You will see inbound traffic and outbound traffic metrics along with top processes by CPU, processes by memory, interfaces by incoming and outgoing traffic, and number of processes.
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

- Copy the Filebeat and Metricbeat config files to the Ansible Container.
- Update the host file to include the ELKServer IP Address. Then update the Filebeat and Metricbeat playbook files to specify the IP Address on the webservers.
- Run the playbook, and navigate to "http://[ 52.243.92.23]:5601/app/kibana" to check that the installation worked as expected.

*As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.*

Install the Ansible and Host configuration files. Create a filebeat and metricbeat folder in your /etc directory: *(/etc/filebeat/) and (/etc/metricbeat)*. In the filebeat folder you can run
"curl -L -O https://raw.githubusercontent.com/elastic/beats/7.6/deploy/docker/filebeat.docker.yml" to get the filebeat configuration file. You can do the same method for metricbeat in the metricbeat folder, use
"curl -L -O https://raw.githubusercontent.com/elastic/beats/7.6/deploy/docker/metricbeat.docker.yml".

You will have to configure your filebeat/metricbeat YAML configuration files and ansible hosts configuration files to your specific network IP addresses and ports you have configured on the cloud.

Config Files:

- [AnsibleHost](https://github.com/Valethiel/cyber-security-portfolio/blob/9a1abe5bb173512523745be6fce7576a7827d4bf/Ansible/HostConfiguration.yml.txt)
- [FilebeatConfiguration](https://github.com/Valethiel/cyber-security-portfolio/blob/9a1abe5bb173512523745be6fce7576a7827d4bf/Ansible/FilebeatConfiguration.txt)
- [MetricbeatConfiguration](https://github.com/Valethiel/cyber-security-portfolio/blob/9a1abe5bb173512523745be6fce7576a7827d4bf/Ansible/MetricbeatConfiguration.txt)

YAML playbooks for filebeat and metricbeat:

- ![FilebeatPlaybook](https://github.com/Valethiel/cyber-security-portfolio/blob/9a1abe5bb173512523745be6fce7576a7827d4bf/Ansible/FilebeatPlaybook.yml.txt)
- ![MetricbeatPlaybook](https://github.com/Valethiel/cyber-security-portfolio/blob/9a1abe5bb173512523745be6fce7576a7827d4bf/Ansible/MetricbeatPlaybook.yml.txt)

Run ansible-playbook filebeat-playbook.yml and the metricbeat-playbook.yml from their respective folders.
