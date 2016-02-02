<h1> Ansible Playbook to install Elasticsearch on ubuntu 14.04 </h1>

<h2> Version getting installed </h2>
2.x version of Elasticsearch <br>

<h2> User guide </h2>
1. Install Ansible
<href> http://docs.ansible.com/intro_installation.html </href>
This playbook is tested on OS-X as well as ubuntu box.

2. SSH setup to elk-server host as well all the elk-compute hosts
This is required for ansible. Please read below for more info:
http://docs.ansible.com/intro_getting_started.html

3. Git Checkout this code
git clone https://github.com/ronakmshah/ansible-elk.git<br/>
cd ansible-elk

4. Write hosts(inventory) file<br/>Read hosts.example for example<br/>cp hosts.example hosts and than make a change into hosts file as required. <br/>
[NOTE:There would be one or more elasticsearch node ips where elasticsearch will be installed.

5. After that one can verify connection to all the hosts by running:
ansible all -m ping [-u <user>]

6. Elasticsearch Server Name and IP
Modify /roles/elk-server/defaults/main.yml <br/>
to write correct elk_server name and ip.<br/>

7. Install Elasticsearch on a host<br/>
ansible-playbook -i hosts elk_server.yml



