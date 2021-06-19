## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/dtcyberboot/CyberSchool/blob/main/Images%20for%20RedTeam%20Network/CORAL%20-%20RedTeam%20Network%20Diagram%20w%20ELK.pdf

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. 
Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

playbook.yml
elkplaybook.yml
filebeat-playbook.yml


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

Load balancers protect the servers/network from denial of service attacks.  They distribute traffic amongst two or more servers (in this case, three).
The advantage to using a jumpbox is that it can be firewalled and hold containers to protect the network from public exposure and system breaches.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system performance.
	- Filebeat monitors the log files
	- Metricbeat records metrics/statistics from the system

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name      | Function | Private IP |   Public IP    | Operating System |
|---------- |----------|------------|----------------|------------------|
| Jump Box  | Gateway  | 10.0.0.4   | 13.92.31.45    |Linux Ubuntu 18.04|
| DVWA Web-1|   VM     | 10.0.0.5   | None           |Linux Ubuntu 18.04|
| DVWA Web-2|   VM     | 10.0.0.6   | None           |Linux Ubuntu 18.04|
| DVWA Web-1|   VM     | 10.0.0.7   | None           |Linux Ubuntu 18.04|
| ELK VM    | ELKStack | 10.2.0.4   | 52.165.129.217 |Linux Ubuntu 18.04|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
	- 175.68.182.52 - my local machine (*NOTE: not my actual public IP. Changed for security.) 

Machines within the network can only be accessed by 10.0.0.4.
The following are machines that can access the ELK VM 
	- My local machine (175.68.182.52) via port 5601
	- Ansible jumpbox (13.92.31.45; 10.0.0.4) via ssh port 22

A summary of the access policies in place can be found in the table below.

| Name           | Publicly Accessible | Allowed IP Addresses  |
|----------------|---------------------|-----------------------|
| Jump Box       | No	               | 175.68.182.52         |
| DVWA VMs       | No                  | 13.92.31.45; 10.0.0.4 |
|   Web-1, 2 & 3 |                     |                       |
| ELK Stack	 | No                  | 175.68.182.52;        |
|                |                     | 13.92.31.45; 10.0.0.4 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
running ansible from the command line ensures consistency in running the configuuration scripts, regardless of location.

The playbook implements the following tasks:
	- Install docker.io
	- Install python-pip3
	- Install docker python module
	- Change memory on ELK VM
	- Download and launch a docker elk container
	- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/dtcyberboot/CyberSchool/blob/main/Images%20for%20RedTeam%20Network/ssh%20from%20JB%20ansible%20container%20to%20ELK-SERVER%20and%20show%20elk%20container.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
	- 10.0.0.5 (Web-1)
	- 10.0.0.6 (Web-2)
	- 10.0.0.7 (Web-3)

We have installed the following Beats on these machines:
	- filebeat-7.4.0-amd64.deb
	- metricbeat-7.6.1-amd64.deb
 
These Beats allow us to collect the following information from each machine:
	- Filebeat sends log files to Kibana.
	- Metricbeat collects and displays statistice/metrics of the systems and services on the machine.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
	- Copy the filebeat-config.yml file and the filebeat-playbook.yml to the ansible container (musing_swartz) /etc/ansible.
	- Update the hosts file to include 10.0.0.5, 10.0.0.6, 10.0.0.7
	- Run the playbook, and navigate to Kibana (http://52.165.129.217 <your.ELKVM.IP>:5601/app/kibana) to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
	- The filebeat playbook is <filebeat-playbook.yml> located in the ansible container on the JumpBox (musing_swartz) at /etc/ansible.
	- The file to update to make Ansible run the playbook on a specific machine is the hosts file located on the ansible container (musing_swartz) in the /etc/ansible folder. 
		- In the hosts file, under [webservers] header, spcify the web server IPs to access (10.0.0.5, 10.0.0.6, 10.0.0.7)
	- The URL to navigate to in order to check that the ELK server is running is http://52.165.129.217 <your.ELKVM.IP>:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._