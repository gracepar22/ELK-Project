# ELK-Project
Ansible YAML scripts and Bash scripts with images/screenshots
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._
---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: gracep
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install python3-pip
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module (It will default to pip3)
    - name: Install Docker module
      pip:
        name: docker
        state: present

      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

      # Use sysctl module
    - name: Use more VM memory
      sysctl:
        name: vm.max_map_count
        value: '262144'
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        # Please list the ports that ELK runs on
        published_ports:
          -  5601:5601
          -  9200:9200
          -  5044:5044

      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting acess to the network.

- _TODO: What aspect of security do load balancers protect? 
⦁	It defends an organization aganist distributed denial-of-service(DDoS) attacks. It offers a health probe function to regualryl check all the machines behind the load balancer.

What is the advantage of a jump box?_
⦁	It's a secure VM 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jump box and system network.
- _TODO: What does Filebeat watch for? It monitors log files or locations that you sepecify, collects log events and forwards them either to Elasticsearch or Logstash for indexing.
- _TODO: What does Metricbeat record? It peridodically collects mertrics from the opertating system and from services that are running on the server. Metricbeattakes metrics and statistics that it collects and ships them to the output that specify such as Elasticsearch or logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name        | Function   | IP Address | Operating System  |
|-------------|------------|------------|-------------------|
|   Jumpbox   | Gateway    | 10.0.0.4   | Linux             |
| Web-1       | Webserver  | 10.0.0.5   | Linux             |
| Web-2       | Webserver  | 10.0.0.6   | Linux             |
| ELK-Project | Monitoring | 10.1.0.4   | Linux             |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_
⦁	5061 Kibana port

Machines within the network can only be accessed by Jump Box Provisioner.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address? 
⦁	My home laptop & my IP address is 99.118.237.235

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Address |
|-------------|---------------------|--------------------|
|   Jumpbox   | Yes                 | 99.118.237.235     |
| Web-1       | No                  | 10.1.0.4           |
| Web-2       | No                  | 10.1.0.4           |
| ELK-Project | No                  | 10.1.0.4           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
⦁	Ansible is a free open-source tool we can use, it has a very simple setup and how to use it. You don't require coding skills to use it. Ansible is very powerful tool that lets you model high complex IT work. Its a flexible tool that you can customize it to your needs. It doesn't require to install any other software or firewall ports to the clients systems you want to automate and you don't need to have a separate set up managemnt structure. Its an efficient tool cause you don't need to have extra software and you have more room for application resources on your server.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
⦁	install docker.io
⦁	install pip3
⦁	install docker python module
⦁	increase virtual memory
⦁	download & launch a docker

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
| Name  | IP Address |
|---------|-------------|
| Web-1 | 10.0.0.5   |
| Web-2 | 10.0.0.6   |

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
⦁	microbeats

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
⦁	filebeat- used to monitor the Elasticsearch log files, collects log events and ship them to the monitoring cluster. Your recent logs are visible and on Monitoring page in Kibana.
⦁	metricbeat- it helps you monitor your servers by collectiong metrics from the system and services running on the server such as; Apache.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to Ansible Control.
- Update the hosts file to include... 
[webservers]
#alpha.example.org
#beta.example.org
#192.168.1.100
#192.168.1.110
10.0.0.5 ansible_python_interpreter=/usr/bin/python3
10.0.0.6 ansible_python_interpreter=/usr/bin/python3

[elk]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3

- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
⦁	filebeat-playbook.yml & metricbeat-playbook.yml
⦁	cp filebeat-playbook.yml metricbeat-playbook.yml /etc/ansiible/files

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
⦁	You have to update the hosts file to make Ansivle run the playbook on a specific machine also specify which machine to install the ELK server on verus which to install Filebeat.

- _Which URL do you navigate to in order to check that the ELK server is running?
⦁	http://20.98.162.30:5601/app/kibana#/home

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
