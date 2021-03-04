# ELK

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

  - https://github.com/keubanks-creator/Cybersecurity/blob/main/Diagrams/Homework12.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. Alternatively, select portions of the .yml files may be used to install only certain pieces of the stack, such as Filebeat and/or Metricbeat.

  - https://github.com/keubanks-creator/Cybersecurity/blob/main/Ansible/ELK/elk-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
Load balancers allow servers to spread the workload out evenly and can provide an extra layer of security by having an off-load function. In the case of a DOS attack, the balancer can direct all the traffic from the server to a public cloud provider. Adding a jump box to the network is a great security measure. 

A jump box is used solely for administrative tasks, this saves the risk of admin details being used on the same computer that you websurf on, as that is easier for hackers to attack. Jump box's can have rules set so only specific IP addresses can access them, making them even more secure.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network and system metrics.
- Filebeat detects changes to the filesystem. We have set this one up to collect apache logs. 
- Metricbeat dectects changes to systems metrics, one example being changes in the CPU. Metricbeat as detects failed ssh and sudo attempts. 

The configuration details of each machine may be found below.

| Name       | Function   | IP Address | Operating System |
|------------|------------|------------|------------------|
| Jump-Box   | Gateway    | 10.0.0.4   | Linux            |
| Web-1      | Web Server | 10.0.0.5   | Linux            |
| Web-2      | Web Server | 10.0.0.6   | Linux            |
| Web-3      | Web Server | 10.0.0.7   | Linux            |
| ELK-Server | Monitoring | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 73.7.17.51

Machines within the network can only be accessed by each other.
- All virtual machines in the Red_Team_Virtual_Network have access to the ELK VM because there are connected using peerings to the Elk-Net. The IP address subnet is 10.0.0.0/16.

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses                 |
|------------|---------------------|--------------------------------------|
| Jump-Box   | Yes                 | 73.7.17.51                           |
| Web-1      | No                  | 10.0.0.4                             |
| Web-2      | No                  | 10.0.0.4                             |
| Web-3      | No                  | 10.0.0.4                             |
| ELK-Server | No                  | 10.0.0.4 10.0.0.5 10.0.0.6 10.0.0.7  | 

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this saves time by eliminating repetive tasks and allows for fewer mistakes.

The playbook implements the following tasks:
- Firstly, the host is set to elk to only run on the elk server
- The playbook then installs docker.io and python3-pip
- The pip module is installed next 
- After those are installed we increase the virtual memory using sysctl vm.max_map_count and setting that to the value of "262144"
- The docker elk container is downloaded next, using a specified image and specified ports, in this case we use image 761 and published ports; 5601, 9200, 5044.


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

 - https://github.com/keubanks-creator/Cybersecurity/blob/main/Ansible/Images/docker-ps.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat


These Beats allow us to collect the following information from each machine:
- Filebeat ships and monitors log files.
- Metricbeat ships host metrics. 

 
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the -config.yml files to the etc/ansible/ folder.
- Update the hosts file to include your VM you want the playbook to run on. Add [elk] and the ELK IP of 10.1.0.4 and add [webservers] with the IP addresses of 10.0.0.5, 10.0.0.6, 10.0.0.7.
- https://github.com/keubanks-creator/Cybersecurity/blob/main/Ansible/Ansible-Cfg/hosts.rtf
- Run the playbooks, and navigate to [elkserverVM-IP]:5601 to check that the installation worked as expected.

