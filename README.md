# Week13-Project1-JohnH
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Security](Diagrams/CloudSecurityDiagram.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Playbook file may be used to install only certain pieces of it, such as Filebeat.

```yml
---
- name: Configure Elk VM with Docker
  hosts: elk
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
        state: present                                                                                                                                       >    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144                                                                                                             >        name: vm.max_map_count
        value: "262144"
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
        published_ports:                                                                                                                                                -  5601:5601                                                                                                                                                  -  9200:9200                                                                                                                                                  -  5044:5044
  - 
  -
  ```

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly Available, in addition to restricting Traffic to the network.
-  What aspect of security do load balancers protect? What is the advantage of a jump box? Availability

-  Load Balancing is an important security role as computing moves more to the cloud. The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks. It does this by shifting attack traffic from the corporate server to a public cloud provider.

-  When a jump box is used, its hidden benefit is that any tools in place for the SAN system are maintained on that single system. Therefore, when an update to the SAN management software is available, only a single system requires the update.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Logs and system Traffic.
- What does Filebeat watch for?_Log files and log events. Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

- What does Metricbeat record?_Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table.

| Name     | Function  | IP Address | Operating System |   |
|----------|-----------|------------|------------------|---|
| Jump Box | Gateway   | 10.1.0.4   | Linux            |   |
| Web-1    | Webserver | 10.1.0.5   | Linux            |   |
| Web-2    | Webserver | 10.1.0.6   | Linux            |   |
| ELK-VM   | Webserver | 10.2.0.4   | Linux            |   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Add whitelisted IP addresses_

Machines within the network can only be accessed by Jump-Box through SSH.
- Which machine did you allow to access your ELK VM? What was its IP address?

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Address |   |
|----------|---------------------|--------------------|---|
| Jump Box | Yes                 | 40.117.210.49      |   |
| Web-1    | No                  | 10.1.0.5           |   |
| Web-2    | No                  | 10.1.0.6           |   |
| ELK-VM   | No                  | 10.2.0.4           |   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_You can put a command from multiple servers into a single playbook.

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.
- install docker io
- python pip
- install docker
- systemctl -w vm.max_map_count=26144

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Screenshot](Diagrams/docker_ps.jpg)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring_
- Web-1 10.1.0.5
- Web-2 10.1.0.6

We have installed the following Beats on these machines:
- Specify which Beats you successfully installed_
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.

-  Filebeat collects the changes that have been made. 
-  Metricbeat collects metrics and statistics

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the yml file to ansible folder.
- Update the config file to include...
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

_Answer the following questions to fill in the blanks:_
- Which file is the playbook? Where do you copy it?_File beat config file /etc/ansible/files/
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_edit etc host file to webservers/elk server ip addresses
- Which URL do you navigate to in order to check that the ELK server is running?
  http://40.79.31.186:5601/app/kibana#/home
  
_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._


