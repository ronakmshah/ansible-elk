<h1> Ansible-ELK </h1>

This is my attempt to create an ansible playbook to install ELK (Elasticsearch-Logstash-Kibana)
stack on a distributed systems and collect specifically OVS related logs, present it in a Kibana
dashboard and potentially add watcher (alerting plugin) to the stack to the mix.

<h2> Use-cases </h2>
1. Openstack based privated cloud powered by any OVS based networking stack. Single point to gather
all the WARN, DEBUG and ERROR logs, index them and present them rather than going to each compute node to 
read them in a text file.

2. SD-WAN network based on OVS networking stack. OVS component could be running on any CPEs.
Single point to gather all the WARN, DEBUG and ERROR logs, index them and present them in kibana
dashboard to the WAN admin.

3. Many more as we go along and add more elastic plugins.
 
<h3> Note </h3>
This playbook can be easily extended to collect other logs as well. 

I understand that there are ansible roles in galaxy.ansible.com
which I could have used. But I wanted to get my hands dirty anyways and wanted a bit customization
as well. 

I have tried to write a playbook based on the manual install that I already have tried 
based on <href> https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-4-on-ubuntu-14-04 </href>


<h2> User guide </h2>
1. Install Ansible
<href> http://docs.ansible.com/intro_installation.html </href>
This playbook is tested on OS-X as well as ubuntu box.

2. SSH setup to elk-server host as well all the elk-compute hosts
This is required for ansible. Please read below for more info:
http://docs.ansible.com/intro_getting_started.html

3. Git Checkout this code
git clone https://github.com/ronakmshah/ansible-elk.git
cd ansible-elk

4. Write hosts(inventory) file
Read hosts.example for example
cp hosts.example hosts and than make a change into hosts file as required.

There would be one elk-server host where elasticsearch, kibana and logstash server will need to be installed.
There would be one or more elk-compute/forwarder hosts where logstash-forwarder will be installed.

After that one can verify connection to all the hosts by running:
ansible all -m ping [-u <user>]

5. ELK Server Name and IP
Modify /roles/elk-server/defaults/main.yml
to write correct elk_server name and ip.

[Optional] You can also define a port where logstash server will be serving all the forwarders.
Default value is chosen to be 5000

6. Install ELK on a host
ansible-playbook -i hosts elk_server.yml

7. ELK Forwarder configuration
Modify roles/ovs-logstash-forwarder/defaults/main.yml
- Write correct elk_server name and port to connect.
  Default for port is 5000. This is the port that logstash server running on elk server 
  can be connected on. For simplicity I have kept it under the same variable. 

- Creaete a list of logstash_forwarder_files.
  Add path and type for each file that you wish to collect log of.

8. Install logstash forwarder on compute/forwarder node
ansible-playbook -i hosts elk_compute.yml


