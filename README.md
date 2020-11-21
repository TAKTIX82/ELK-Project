## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![https://github.com/TAKTIX82/ELK-Project/blob/main/Images/Project-1-ELK-Stack1.png](https://github.com/TAKTIX82/ELK-Project/blob/main/Images/Project-1-ELK-Stack1.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  ![filebeat-playbook.yml](https://github.com/TAKTIX82/ELK-Project/blob/main/Ansible/filebeat-playbook.yml)
  
  ![metricbeat-playbook.yml](https://github.com/TAKTIX82/ELK-Project/blob/main/Ansible/metricbeat-playbook.yml)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology:

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Dmn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- It protects your data networks from DDos attacks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the  and system logs.
- Filebeat monitors log files that you specify and forwards them to Elasticsearch or Logstach.
- Metricbeat periodically collects the metrics and statistics from your OS then outputs what you specify to the Elasticssearch or Logstach.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name       | Function      | IP Address                                 | Operating System |
|------------|---------------|--------------------------------------------|------------------|
| Jump Box   | Gateway       | 52.237.232.147(Public)/10.0.0.4(Private)   | Linux            |
| Web-1      | Server        | 10.0.0.8(Private)                          | Linux            |
| Web-2      | Server        | 10.0.0.10(Private)                         | Linux            |
| RedTeam-LB | Load Balancer | 13.70.126.216(Public)                      | Linux            |
| ELK-VM     | Server        | 13.76.84.215(Public)/10.1.0.4(Private)     | Linux            |

### Access Policies:

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 123.208.254.53 (LocalHost IP Address)

Machines within the network can only be accessed by JumpBox Provisioner.
- The JumpBox Provisioner with IP 10.0.0.4 (Private)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 123.208.254.53       |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |
| ELK-VM   | No                  | 10.0.0.4             |

### Elk Configuration:

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main benefit of Ansible is to allows IT admins to automate the setup of multiple machines with one file.

The playbook implements the following tasks:
- Installed Docker.io and Python3.pip
- Installed the Docker module
- Increased memory on the ELK-VM
- Download and install a docker elk container sebp/elk:761 to run on specific ports
- Enabled docker services on start up

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![ELK-Screenshot.png](https://github.com/TAKTIX82/ELK-Project/blob/main/Images/ELK-Screenshot.png)

### Target Machines & Beats:
This ELK server is configured to monitor the following machines:
- Web-1 VM 10.0.0.8 (Private)
- Web-2 VM 10.0.0.10 (Private)

We have installed the following Beats on these machines:
![Filebeat-Screenshot.png](https://github.com/TAKTIX82/ELK-Project/blob/main/Images/Filebeat-Screenshot.png)
![Metricbeat-Screenshot.png](https://github.com/TAKTIX82/ELK-Project/blob/main/Images/Metricbeat-Screenshot.png)

These Beats allow us to collect the following information from each machine:
- Filebeat is used to collect log files from the 2 specified VM's Web-1 and Web-2
- Example of what logs Filebeats module collects is audit logs, depeciation logs, server logs, garbage collection logs and slow logs
- Metricbeat is used to periodically collect metrics of the OS and from services running on the server.
- Examples of what metrics and statistics it collects from is Apache, HAProxy, MongoDB, MySQL, Nginx, PostgreSQL, Redis, System and Zookeeper


### Using the Playbook:
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into your Jump-Box-Provisioner and follow the steps below:

--Filebeat--

- Copy the deb Filebeat config file from Kibana with curl -L -O https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml
- Update the filebeat-config.yml file to include the following 

   ![update-filebeat-config.png](https://github.com/TAKTIX82/ELK-Project/blob/main/Images/update-filebeat-config.png)
  
- Run the playbook (ansible-playbook filebeat-playbook.yml), and navigate to http://13.76.84.215:5601/app/kibana > Add Logs:Add log data > System Logs > 5: Module status to check that the installation worked as expected.
- Which file is the playbook? ![filebeat-playbook.yml](https://github.com/TAKTIX82/ELK-Project/blob/main/Ansible/filebeat-playbook.yml)
- Where do you copy it? Copy it from your Jump-Box-Provisioner /etc/ansible/files/filebeat-config.yml to the Web-1 & Web-2 file /etc/filebeat/filebeat.yml
- Which file do you update to make Ansible run the playbook on a specific machine? Update /etc/ansible/hosts
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? Update the array in /etc/ansible/hosts file to have Web-1 & Web-2 VMs under a title/heading of [webservers] and have the ELK stack under a title/heading of [ELK-server]
- Which URL do you navigate to in order to check that the ELK server is running? http://13.76.84.215:5601/app/kibana

--Metricbeat---

- Copy the deb Metricbeat config file from Kibana with curl -L -O https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-config.yml
- Update the metricbeat-config.yml file to include the following

   ![update-metricbeat-config.png](https://github.com/TAKTIX82/ELK-Project/blob/main/Images/update-metricbeat-config.png)
  
- Run the playbook (ansible-playbook metricbeat-playbook.yml), and navigate to http://13.76.84.215:5601/app/kibana > Metrics:Add metric data > Docker metrics > 5:Module statusto check that the installation worked as expected.
- Which file is the playbook? ![metricbeat-playbook.yml](https://github.com/TAKTIX82/ELK-Project/blob/main/Ansible/metricbeat-playbook.yml)
- Where do you copy it? Copy it from your Jump-Box-Provisioner /etc/ansible/files/metricbeat-config.yml to the Web-1 & Web-2 file /etc/metricbeat/metricbeat.yml
- Which file do you update to make Ansible run the playbook on a specific machine? Update /etc/ansible/hosts 
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? Update the array in /etc/ansible/hosts file to have Web-1 & Web-2 VMs under a title/heading of [webservers] and have the ELK stack under a title/heading of [ELK-server]
- Which URL do you navigate to in order to check that the ELK server is running? http://13.76.84.215:5601/app/kibana

### Bonus: 
