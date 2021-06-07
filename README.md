## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://user-images.githubusercontent.com/77760636/120901997-1da8d880-c5fb-11eb-942e-d260f01edf5f.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the live Elk Deployment file may be used to install only certain pieces of it, such as Filebeat.

  /etc/ansible/[install-elk.yml](https://github.com/Laurenthia/Elk-Stack-Project/blob/0c8b4c9e1e73ad88d03742d3986ea5eb7a5b5db5/install-elk.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.
- What aspect of security do load balancers protect? 
  -   The Load Balancer helps defend from (DDoS) Denial-of-Service attack by shifting to a different server. 
- What is the advantage of a jump box? 
  -   The advantage of a jump box forces all of the traffic to a single node. This Jump Box requires SSH for added security. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- What does Filebeat watch for?
  - Filebeat monitors the specified log files or locations, collects log events, and sends them to Elasticsearch. 
- What does Metricbeat record?
  - Metricbeat is lightweight that periodically records metrics from OS and from services running on the server. It takes the statistics and metrics that it collects and forwards them to Elasticsearch. 

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux (Ubuntu 18.04)           |
| Web 1    | DVWA     | 10.0.0.5   | Linux (Ubuntu 18.04)           |
| Web 2    | DVWA     | 10.0.0.6   | Linux (Ubuntu 18.04)           |
| Web 3    | DVWA     | 10.0.0.7   | Linux (Ubuntu 18.04)           |
| ELK      | ELK      | 10.2.0.5   | Linux (Ubuntu 18.04)           |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP Address

Machines within the network can only be accessed by SSH port 22.
- Which machine did you allow to access your ELK VM? 
  - Jumpbox from private IP 10.0.0.4 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Personal IP          |
| Web 1    | No                  | 10.0.0.4             |
| Web 2    | No                  | 10.0.0.4             |
| Web 3    | No                  | 10.0.0.4             |
| Elk      | No                  | 10.0.0.4 & Personal IP|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- It allows to be able to scale automation on multiple machines using a Playbook and not having to risk creating errors in configuring the machines manually.

The playbook implements the following tasks:
 - The header of the Playbook can specify which group of machines and with a different remote user.
 - The Playbook installs the following services:
   -  docker.io
   - python3-pip
   - docker
- To Launch and expose the container run sebp/elk:761 
   - The container should be started with the published ports:
   - 5601:5601 9200:9200 5044:5044   

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://github.com/Laurenthia/Elk-Stack-Project/blob/92a61aa6e847d016c846315c9b11c1558e90efc1/Elkteam_Docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 10.0.0.5
- Web 2 10.0.0.6
- Web 3 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
-Filebeat allows us to collect file system data, enabling an analyst to monitor for suspicious activity.
-Metricbeat allows us to collect specific metrics about the machines in the network such as CPU usage and uptime. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the [filebeat-config.yml](https://github.com/Laurenthia/Elk-Stack-Project/blob/7d02655467eca20f2e33ce092bd37f3b12cba20e/filebeat-config.yml) and [metricbeat-config.yml](https://github.com/Laurenthia/Elk-Stack-Project/blob/0c8b4c9e1e73ad88d03742d3986ea5eb7a5b5db5/metricbeat-config.yml)file to /etc/ansible/roles.
- Update the configuration file to include private IP of the Elk server to the Elasticsearch and Kibana
- Run the playbook, and navigate to the Elk VM to check that the installation worked as expected.
- Which file is the playbook? 
  - [filebeat-playbook.yml](https://github.com/Laurenthia/Elk-Stack-Project/blob/c743bb59c7b913a42d73c256de95758fde7bd690/filebeat-playbook.yml)
- Where do you copy it?
  -  curl https://github.com/Laurenthia/Elk-Stack-Project/blob/c743bb59c7b913a42d73c256de95758fde7bd690/filebeat-playbook.yml > '/etc/ansible/filebeat-playbook.yml'
- Which file do you update to make Ansible run the playbook on a specific machine?
  -  Update the Ansible [Hosts](https://github.com/Laurenthia/Elk-Stack-Project/blob/190ee4fb4680ad393dbaca2aa173d5fe264af010/Hosts) file within the Ansible container /etc/ansible/hosts
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?
  - Within the [install-elk.yml](https://github.com/Laurenthia/Elk-Stack-Project/blob/26d5ae911b18cd17d7612c40b87b982745b93bbf/install-elk.yml)
- Which URL do you navigate to in order to check that the ELK server is running?
  -http://[your.VM.IP]:5601/app/kibana 

### The Commands Needed to Run the Ansible Configuration for the Elk-Server:

- SSH into jumpBox Vm ssh RedAdmin@[Public IP address]
- Run sudo docker container list -a
- Run sudo docker start container [Container name]
- Run sudo docker attach container [Container name]
- Update the hosts file in /etc/ansible/[Hosts](https://github.com/Laurenthia/Elk-Stack-Project/blob/190ee4fb4680ad393dbaca2aa173d5fe264af010/Hosts)
- Create new Ansible playbook to use for your new Elk Vm curl [elk.yml](https://github.com/Laurenthia/Elk-Stack-Project/blob/26d5ae911b18cd17d7612c40b87b982745b93bbf/install-elk.yml) > /etc/ansible/install-elk.yml
- Run ansible-playbook intall-elk.yml
- After the Elk container is installed double check elk-docker container is running by SSH into Elk VM ssh sysadmin@[private IP address]
- Run sudo docker ps
- Since the Elk server runs on port 5601 you need to create an incoming rule for the security group that allows TCP traffic over the port 5601 from your IP address.
- Check that you can load the ELK stack server at http://[your.VM.IP]:5601/app/kibana.
If everthing works correcty, you should see the home webpage of kibana.
