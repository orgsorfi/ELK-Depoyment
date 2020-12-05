## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Azure_VM_Diagrams](Diagrams/Azure_VM_Elk_Server.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. 
Alternatively, select portions of the -Yaml_scripts- files may be used to install only certain pieces of it, such as Filebeat.

  - /Ansible/Yaml_scripts/Elk_stack.yml -

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly Accessible, in addition to restricting unwanted taffic to the network.

- _ Load balancer defends an organization against distributed denial-of-service (DDoS) attacks. It is also placed in an NSG (network security group) that were it looks for rules to apply when incoming traffic attempts conections to the IP load balancer there have also been other tools installed to help control conections like a health porbe
- _ The jump box is a secure computer that admins use to launch a new netork and servers. One of the major benifits of the jump box, is that it will alwase be running the latest software updates. They can be ristriced to only access at very specific loctations. They do not usaly have a GUI and should not be able to surf the web. they also let you connect and update mutlipul compuers at once.
- _ Filebeat watches for changes in the log files on the VMs it looks for log files and then harvest the new data form it and passes it on to be proccesed_
- _ Metricbeat records and harvests data by collecting mertic from the system suc as apache, HAProxy, MySQL and System and then passes it on to be proccessed_

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function    | IP Address | Operating System |
|----------|-------------|------------|------------------|
| Jump Box | Gateway     | 10.0.0.4   | Linux            |
| Web 1    |DVWA site    | 10.0.0.7   | Linux            |
| Web 2    |DVWA site    | 10.0.0.8   | Linux            |
| Web 3    |DVWA site    | 10.0.0.5   | Linux            |
| Elk      |ELK container| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the - ELK and Jump - machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _67.169.250.57 _

Machines within the network can only be accessed by Jump Box.
- _Elk-Stack-VM can be accessed by the web machines and the ump box from internal network and by the newtwork admins office computer. Its IP address is 67.169.250.57

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 67.169.250.57        |
| Elk VM   | Yes                 | 67.169.250.57        |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Main advantage of automating configuration with Ansible is you can make changes to all containers at once with out haveing to move into each machine its like using group policies in a windows enviorment _

The playbook implements the following tasks:
- Downloading the ELK (sebp/elk:716) image to all machines using the curl command.
- Package installatation with Dkpg.
- Copying the config file so that the port numbers are open to send the log data.
- Setting up and running diffrent commands to congfigure and start the elk status.

The following screenshot displays the result of running `Elk_readout` after successfully configuring the ELK instance.

 
[Elk](Pictures/Elk.PNG)
- _ Pictures/ELK.png_                                         

This is and example of what the command line readout should look like when it has compleated properly without errors

  
### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 @10.0.0.7 Web-2 @ 10.0.0.8 Web-3 @ 10.0.0.5

We have installed the following Beats on these machines:
- Filebeat installed and configured
- Metricbeat installed and configured

These Beats allow us to collect the following information from each machine:
- They feed into kibana logs data consisting of: var/log files (filebeat) and system-level monitoring data (metricbeat). These are automated and push new data on a 10 sec interval.  

- The following pictures are examples of the needed information to setup the .config files for each beat and the sections that need to be updated

_ -This is for the setup of filebeat config file (output.elasticsearch) section 

[Filebeat 1](Pictures/filebeat setup.kibina.2.PNG)

_ - This is for the setup of filebeat config file (setup.kibana) section 

[Filebeat 2](Pictures/fliebeat setup.kibina.PNG)

_ - In kibana on the resourch page check for file input should look like this when all is done, and you test for data imputs

[Filebeat 3](Pictures/verify filebeat data.PNG)

_ - This is for the setup of metricbeat config file (output.elasticsearch) section 

[Metricbeat 1](Pictures/Metricbeat.setup.1.PNG)

_ - This is for the setup of metricbeat config file (setup.kibana) setction 

[Metricbeat 2](Pictures/Metricbeat.setup.2.PNG)

_ - In kibana on the resourch page check for file input should look like this when all is done, and you test for data inputs

[Metricbeat 3](Pictures/docker metrics.PNG)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the elk_stack.yml file copy to the /etc/ansible/roles file and update the remote_user: (with the correct name)
- In the ansible /etc/ansible/hosts file add the host an ELK section with the Internal IP address and ansible_python_interpreter=/usr/bin/python3 
- Now pull up a browser and navigate to the public IP address of the ELK VM at port 5601 with the /app/kibana line should look like this HTTP://publicip:5601/app/kibana
- Run the playbook, and navigate to public ip address of the VM at port :5601/app/kibana to check that the installation worked as expected.

_- The specific commands the user will need to run to download the playbook, update the files, etc._

- To copy from a remote machine move all the files into a single folder and give it a name like /Yaml_scripts
- Then use the SCP protocol like this for the remote machine to your machine
 scp adminR@104.40.23.39:/home/adminR/Yaml_playbook_scripts/*.yml ./yaml_scripts 
- Using this command to move them from the docker to the remote host machine
 sudo docker cp 862a4:/etc/ansible/Yaml_scripts ~/(filepath) 
- I used this command to move all my scripts to the same folder in my ansible to clean them up
 cp *.yml Yaml_scripts/ 

_- To copy to the container you would use something like this -_
 scp /local/file/path adminR@example: /remote/path
 sudo docker cp /local/file/path "container number":/remote/path
 then you can attack to the container and run the script in the newly creat folder



