# Azure VM Base DVWA & ELK Deployment Project
This particular repository will show you how to create a secure and easy deployment of DVWA and ELK that can be used by Cybersecurity students and professionals.

## Automated DVWA & ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Screenshots](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Diagrams/Cloud%20Security%20and%20Virtualization%20Homework%20with%20Elk%20VM.png)


These files have been tested and used to generate a live DVWA and ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat & metricbeat playbook file may be used to install only certain pieces of it, such as Filebeat.

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
- Ansible installation to a Jump Host
- DVWA Configuration 
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
| ELK Server |Elastic, Logstash and Kibana Server     | 40.118.229.124/10.2.0.4            |Linux                  |
| Load Balancer |Load Balancer      | 104.211.61.233          |                  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ELK virtual machine can accept connections from the Internet just to use it as an application server via this IP addresses:

- 40.118.229.124 using port 5601 via a browser

Machines within the network can only be accessed internally via by Ansible container through SSH port 22 hosted by the Jump Host Server.
- The ELK server can be access via the Ansible container hosted by the Jump Host server using IP address of 10.2.0.4 via SSH port 22.

A summary of the access policies in place can be found in the table below. All VM access are internal network via SSH port 22.

| Name           | Publicly Accessible | Allowed IP Addresses |
|----------------|---------------------|----------------------|
| Jump Box       | Yes                 | 13.92.7.84           |
| ELK Server     | No                  | 10.2.0.4             |
| Web-2          | No                  | 10.1.0.5             |
| Web-3          | No                  | 10.1.0.6             |
| Web-4          | No                  | 10.1.0.7             |


Ansible was used to automate configuration of the DVWA web servers and ELK machine. No configuration was performed manually, which is advantageous because...

- Ansible Automation increased IT and DevOps agility
- Improved standardization and compliance
- Better control over cost of infrastructure and cloud resources.

### Ansible installation to a Jump Host

- Generate a public key using your Terminal or Gitbash (SSH-keygen)
- SSH to your Jump Host VM (SSH username@13.92.7.84)
- Then you can start installing Ansible. 

  1. Install docker.io to your Jump Box
  2. Run first this command: sudo apt update
  3. Then sudo apt install docker.io
  4. Verify that the Docker service is running with this command: sudo systemctl status docker
  5. If not running run this command: sudo systemctl status docker
  6. Once the Docker is installed, pull the container cybersecurity/asnible with this command: sudo docker pull cybersecurity/ansible
  7. Lunch the ansible container and connect it using the appropriate Docker commands: docker run -ti cyberxsecurity/ansible:latest bash 
  8. You can check if the docker is running by this command: sudo docker container ps -a
  9. You can also rename the container by this command: sudo docker rename orig_name new_name
  10. Then it is done. 

### DVWA Configuration

The DVWA installation playbook implements the following tasks:

- As you can see in the below screenshot, the first four lines are important. This will make Ansible know where and what group of servers you want to install and execute the commands for this playbook. As depicted below the first thing that needs to be installed is the docker.io to the DVWA web servers VM.

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/DVWA%20Install%20Part%201.PNG)

- The 2nd part of the of the installation is to install the phython3-pip. Pip is the standard package manager for Python. It allows you to install and manage additional packages that are not part of the Python standard library.

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/DVWA%20Install%20Part%202.PNG)

- The 3rd part of the installation is to install the docker module

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/DVWA%20Install%20Part%203.PNG)

- After executing the previous task then you can execute below command to download and lunch the DVWA
docker container. 

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/DVWA%20Install%20Part%204.PNG)

- This last part of the playbook is optional but nice to have. This particular task is to automatically start the Ansible docker service if you reboot your VM.

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/DVWA%20Install%20Part%205.PNG)

- Below screenshot is the command to use to run the DVWA playbook and what it looks like when executed. If you see red notes it means something is not right and need to correct it.  "Ok" message is a good indicator that it is successful.

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/pentest%20yml.PNG)

### ELK Configuration

The ELK installation playbook implements the following tasks:

- As you can see in the below screenshot, it has the same format of commands in the DVWA playbook. You need the difference in the hosts name. Same thing Ansible need to know where and what group of servers you want to install and execute the commands for this playbook.  As depicted below the first thing that needs to be installed is the docker.io to the ELK VM.

![Screenshots](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Elk%20Install%20Part%201.PNG)

- Same as the DVWA, the 2nd part of the of the installation is to install the phython3-pip. 

![Screenshots](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Elk%20Install%20Part%202.PNG)

- The 3rd part of the installation is same as DVWA installing the docker module

![Screenshots](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Elk%20Install%20Part%203.PNG)

- The 4th part is also very important.  ELK needs an ample amount of memory.  When you setup your VM it requires minimum of 4GB system memory or else ELK will not run due to lack of system resource. Below screenshot are commands that will assign enough memory for the ELK to function properly.

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Elk%20Install%20Part%204.PNG)

- After executing the previous task you can now download and lunch the ELK docker container. You have to specify the ports that ELK use and the port you will be using to access the ELK in the web. In my case here I use the port 5601.

![Screenshots](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Elk%20Install%20Part%205.PNG)

- Same thing as the DVWA, last part of the playbook is optional but nice to have. This command will automatically start the Ansible docker service if you reboot your VM.

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Elk%20Install%20Part%206.PNG)

- Below screenshot is the command to use to run the ELK playbook and what it looks like when executed. Same thing as DVWA, if you see red notes it means something is not right and need to correct it.  "Ok" message is a good indicator that it is successful.

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Install-Elk%20Yml.PNG)

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Screenshots](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Screen%20Shot%20of%20Docker%20PS%20for%20Elk.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 - 10.1.0.5
- Web 2 - 10.1.0.6
- Web 3 - 10.1.0.7

After successfully running this playbook you now have installed the Filebeats and Metricbeats on your machines.

- Filebeats and Metricbeats are now installed to all web servers using the ELK Install playbook. As you can see it seamlessly installed the beats in the three web servers in just one shot.

What are these Beats for? This will allow us to collect information from each machine that we want to try to monitor and record logs. Below are the screenshot and explain what are the main usage and what is used for.

- Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events such as apache logs, website logs, system logs etc. This forwards them either to Elasticsearch or Logstash for indexing.  Below is a sample screenshot of Filebeat.

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Kibana%20Filebeat%20Screenshot.PNG)

- Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash. It periodically collects metric data from your target servers, this could be operating system metrics such as CPU or memory or data related to services running on the server. It can also be used to monitor other beats and ELK stack itself. Below is a sample screenshot of Metricbeat.

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/d0aa95a1cf5ba520700f205239db6eafc85a59dc/Screenshots/Kibana%20Metricbeat%20Screenshot.PNG)

### Filebeat and Metricbeat Diagram

![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Diagrams/ELK%20Diagram.PNG)
(source: https://slidetodoc.com/extending-open-stackansible-with-automated-operational-management-william/)

### See it in action and live by clicking below DVWA or ELK Logo 

   [![](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Logos/DVWA-Small.png)](http://104.211.61.233/) [![](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Logos/ELK-Small.png)](http://40.118.229.124:5601/app/kibana#/home)
   
- Note: In case DVWA ask you for a username and password just type admin as the user and password as the credentials

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the Filebeat (Filebeat-Playbook.yml) and Metricbeat (Metricbeat-Playbook.yml) playbook files to your /etc/ansible directory. To make it manageable I created a subdirectory "roles" and made it as my repository of playbooks.
- Make sure to update the Filebeat (filebeat-config.yml) and Metricbeat (metricbeat-config.yml) configuration files. You need to edit to indicate what specific machine you want to host the Filebeat and Metricbeat app. This configuration files will be copied to the proper Filebeat and Metricbeat folders once you execute their respective playbooks. In my case I created a subdirectory folder named "files" as a repository for my configuration files. Below are the entries that you need to modify on both configuration files before you execute their respective playbook.

    ![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Filebeat%20and%20Metricbeat%20Configuration%20Screenshot.PNG)
    ![Screenshot](https://github.com/cmonasterial/Azure-VM-Base-DVWA-ELK-Deployment-Project-/blob/main/Screenshots/Filebeat%20and%20Metricbeat%20Configuration%20Screenshot%202.PNG)

- Run the playbook, and navigate to http://40.118.229.124:5601/app/kibana to check that the installation worked as expected.

##### Useful Commands List

| Ansible Commands   | Usage |
|----------------|-----------|
|                |           |
|                |           |
|                |           |
|                |           |
|                |           |

#### Visual Guides for proper creation of Azure VMs for DVWA & ELK
