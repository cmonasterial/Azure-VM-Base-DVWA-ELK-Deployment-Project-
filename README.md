# Azure VM Base DVWA & ELK Deployment Project
This particular repository will show you how to create a secure and easy deployment of DVWA and ELK that can be use for Cybersecurity students and professionals.

## Automated DVWA & ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Screenshots](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Diagrams/Cloud%20Security%20and%20Virtualization%20Homework%20with%20Elk%20VM.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat & metricbeat playbook file may be used to install only certain pieces of it, such as Filebeat.

List of playbook files and configuration files needed to make it work:

:triangular_flag_on_post:[Ansible Configuration](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Ansible/ansible.cfg)

:triangular_flag_on_post:[Ansible Hosts](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Ansible/hosts)

:triangular_flag_on_post:[DVWA Install  Playbook](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Ansible/pentest.yml)

:triangular_flag_on_post:[ELK Install Playbook](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Ansible/install-elk.yml)

:triangular_flag_on_post:[Filebeat Configuration](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Ansible/files/filebeat-config.yml)

:triangular_flag_on_post:[Metricbeat Configuration](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Ansible/files/metricbeat-config.yml)

:triangular_flag_on_post:[Filebeat Playbook](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Ansible/roles/filebeat-playbook.yml)

:triangular_flag_on_post:[Metricbeat Playbook](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Ansible/roles/metricbeat-playbook.yml)


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly *available*, in addition to restricting *traffic* to the network.

- Advantage of a load balancer is that it acts as a reverse proxy and distributes network or application traffic across a number of servers. Load balancers are used to increase capacity (concurrent users) and reliability of applications.
- The purpose of an SSH jump host server is to be the only gateway for access to your infrastructure reducing the size of any potential attack surface. Having a dedicated SSH access point also makes it easier to have an aggregated audit log of all SSH connections.

![Screenshots](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Diagrams/Jump%20Host.png)

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the *data* and system *logs*.

- Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing
- Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash. Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server, such as: Apache.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | Private IP/Public IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 13.92.7.84/10.1.0.4   | Linux           |
| Web-1    | Web Server| 10.1.0.5           | Linux                  |
| Web-2    |Web Server | 10.1.0.6  | Linux                 |
| Web-3    |Backup Web server      | 10.1.0.7            |Linux                  |
| ELK Server |Monitoring server      | 40.118.229.124/10.2.0.4            |Linux                  |
| Load Balancer |Load Balancer      | 104.211.61.233          |                  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ELK virtual machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 40.118.229.124 using port 5601 via a browser

Machines within the network can only be accessed internally via by Ansible container through SSH port 22 hosted by the Jump Host Server.
- The ELK server can be access via the Ansible container hosted by the Jump Host server using IP address of 10.2.0.4 via SSH port 22.

A summary of the access policies in place can be found in the table below. All VM access are internal network via SSH port 22.

| Name           | Publicly Accessible | Allowed IP Addresses |  Authentication Method        |
|----------------|---------------------|----------------------|-------------------------------|
| Jump Box       | Yes                 | 13.92.7.84           | Public Key                    |
| ELK Server     | No                  | 10.2.0.4             | Public Key                    |
| Web-2          | No                  | 10.1.0.5             | Public Key                    |
| Web-3          | No                  | 10.1.0.6             | Public Key                    |
| Web-4          | No                  | 10.1.0.7             | Public Key                    |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible Automation increased IT and DevOps agility
- Improved standardization and compliance
- Better control over cost of infrastructure and cloud resources.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 - 10.1.0.5
- Web 2 - 10.1.0.6
- Web 3 - 10.1.0.7

We have installed the following Beats on these machines:
- Filebeats and Metricbeats are installed to all web servers

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
