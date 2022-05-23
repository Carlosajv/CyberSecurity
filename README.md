## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![vNet Diagram](https://github.com/Carlosajv/CyberSecurity/blob/main/Diagrams/Screenshot%202022-05-21%20121309.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat and Metricbeat..


-[Ansible.Conf](https://github.com/Carlosajv/CyberSecurity/blob/main/Ansible/Ansible.cfg.txt)

-[Pentest.YML](https://github.com/Carlosajv/CyberSecurity/blob/main/Ansible/Pentest.yml.txt)

-[Install-ELK.YML](https://github.com/Carlosajv/CyberSecurity/blob/main/Ansible/Install-ELK.yml.txt)

-[Filebeat-Config.YML](https://github.com/Carlosajv/CyberSecurity/blob/main/Ansible/Filebeat-Config.yml.txt)

-[Filebeat-Playbook.YML](https://github.com/Carlosajv/CyberSecurity/blob/main/Ansible/Filebeat-Playbook.yml.txt)

-[Metricbeat-Config.YML](https://github.com/Carlosajv/CyberSecurity/blob/main/Ansible/Metricbeat-Config.yml.txt)

-[Metricbeat-Playbook.YML](https://github.com/Carlosajv/CyberSecurity/blob/main/Ansible/Metricbeat-Playbook.yml.txt)


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

-What aspect of security do load balancers protect? 


According to Azure security baseline for Azure Load Balancer, the load balancer's main purpose is to distribute web traffic across multiple servers. In our network, the load balancer was installed in front of the VM to

-protect Azure resources within virtual networks.

-monitor and log the configuration and traffic of virtual networks, subnets, and NICs.

-protect critical web applications

-deny communications with known malicious IP addresses

-record network packets

-deploy network-based intrusion detection/intrusion prevention systems (IDS/IPS)

-manage traffic to web applications

-minimize complexity and administrative overhead of network security rules

-maintain standard security configurations for network devices

-document traffic configuration rules

-use automated tools to monitor network resource configurations and detect changes



-What is the advantage of a jump box?


A Jump Box or a "Jump Server" is a gateway on a network used to access and manage devices in different security zones. A Jump Box acts as a "bridge" between two trusted networks zones and provides a controlled way to access them. We can block the public IP address associated with the VM. It helps to improve security also prevents all Azure VM’s to expose to the public.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.

-What does Filebeat watch for?
Filebeat helps keep things simple by offering a lightweight way (low memory footprint) to forward and centralize logs, files and watches for changes.


-What does Metricbeat record?
Metricbeat helps monitor servers by collecting metrics from the system and services running on the server so it records machine metrics and stats, such as uptime.



The configuration details of each machine may be found below.

| Name       | Function      | IP Address     | Operating System |
|------------|---------------|----------------|------------------|
| Jump Box   | Gateway       | 10.0.0.4       | Linux            |
| Web-1      | webserver     | 10.0.0.5       | Linux            |
| Web-2      | webserver     | 10.0.0.6       | Linux            |
| ELK-Server | Kibana        | 10.1.0.4       | Linux            |
| Red-TeamLB | Load Balancer | 40.122.215.16  | DVWA             |


In addition to the above, Azure has provisioned a load balancer in front of all machines except for the jump box. The load balancer's targets are organized into availability zones: Web-1 + Web-2


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
40.77.55.33 Machines within the network can only be accessed by SSH from Jump Box.

Which machine did you allow to access your ELK VM? 
My personal computer

What was its IP address?
24.104.246.127

A summary of the access policies in place can be found in the table below.

| Name      | Publicly Accessible | Allowed IP Addresses |
|-----------|:-------------------:|:--------------------:|
| Jump Box  | YES                 | 40.77.55.33          |
| ELKServer | YES                 | 40.77.55.33:5601     |
| DVWA 1    | NO                  | 10.0.0.4             |
| DVWA 2    | NO                  | 10.0.0.4             |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible can be used to easily configure new machines, update programs, and configurations on hundreds of servers at once, and the best part is that the process is the same whether we're managing one machine or dozens and even hundreds.

What is the main advantage of automating configuration with Ansible?

-Ansible is focusing on bringing a server to a certain state of operation.

The playbook implements the following tasks:

1) Install Docker.io 

2)Install python3-pip 

3)Install Docker Python Module

4) Download and launch a Docker web container 


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Running Docker](https://github.com/Carlosajv/CyberSecurity/blob/main/Images/InstallELK.png)

### Target Machines & Beats

This ELK server is configured to monitor the following machines:

- Web-1 (DVWA 1) | 10.0.0.5

- Web-2 (DVWA 2) | 10.0.0.6

We have installed the following Beats on these machines:

-Filebeat

-Metricbeat


These Beats allow us to collect the following information from each machine:

-Filebeat: Filebeat detects changes to the filesystem. I use it to collect system logs and more specifically, I use it to detect SSH login attempts and failed sudo escalations.

-Metricbeat: Metricbeat detects changes in system metrics, such as CPU usage and memory usage.

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the elk_install.yml file to /etc/ansible/roles/elk_install.yml

- Update the hosts file to include the attribute, such as [elk], then include your destination ip of the ELK server directly below.

- Run the playbook, and navigate to http://[your_elk_server_ip]:5601/app/kibana to check that the installation worked as expected.

Answer the following questions to fill in the blanks:

Which file is the playbook? 

-Playbooks are the files where Ansible code is written. Playbooks are written in YAML format. YAML stands for Yet Another Markup Language. Playbooks are one of the core features of Ansible and tell Ansible what to execute.

-For the ANSIBLE : We will create the pentest.yml as our playbook.

-For FILEBEAT: We will create filbeat-playbook.yml as our playbook.

-For METRICBEAT: We will create metricbeat-playbook.yml as our playbook.

-For ELK: We will create Install-ELK.yml.txt as our playbook.

Where do you copy it?

-/etc/ansible/roles/elk_install.yml

Which file do you update to make Ansible run the playbook on a specific machine?

-/etc/ansible/hosts file
 
How do I specify which machine to install the ELK server on versus which to install Filebeat on?

- /etc/ansible# curl -L -O https://ansible.com/  > ansible.cfg

- /etc/ansible# nano ansible.cfg

- Press CTRL + W (to search > enter remote_user then change `remote_user = sysadmin`

-Where : `sysadmin` is the remote user that has control over ansible.


Which URL do you navigate to in order to check that the ELK server is running?

-Test Kibana on web : http://[your.ELK-VM.External.IP]:5601/app/kibana

-Test Kibana on localhost: sysadmin@10.1.0.4: curl localhost:5601/app/kibana
