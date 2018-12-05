# dockerswarm_centos7

This is ansible playbook for creating docker swarm in prod area

Prepare:

- You have to need 8 hosts
- file /etc/hosts ansible server have to be like

192.168.x.x prod-m-01
192.168.x.x prod-m-02
192.168.x.x prod-m-03
192.168.x.x prod-w-01
192.168.x.x prod-w-02
192.168.x.x prod-w-03
192.168.x.x prod-w-04
192.168.x.x prod-w-05



- Check file hosts, add there are like 
[prod_all]
192.168.x.x
192.168.x.x
192.168.x.x
192.168.x.x
192.168.x.x
192.168.x.x
192.168.x.x
192.168.x.x

Run

ansible-playbook create_env.yml

What it will to do?



1 - create all needed soft for you (that i like to use)
 tasks/essentialsoftware.yml




2 - setup last vmware tools
 tasks/open-vm-tools.yml




3 - install kernel from elrepo-release-7.0-3.el7.elrepo.noarch.rpm 
tasks/kernel.yml




4 - configure ntp for correct time. It will add script to crontab. It is workaround for vmware software solution
tasks/ntpd.yml



5 - For good docker working you have to configure os sockets. It is take best practice linux
tasks/socket.yml 
see files
templates/limits.conf
templates/sysctl.conf
 


6 - Disable firewalld. In my case security is not in os level so it may be disables. Also here selinux and NetworkManager is disabled
tasks/disable-firewalld.yml



7 - If you use nfs - This script will configure nfs clients in servers. Defaults is disables. If you need - uncomment it in create_env.yml




8 - And the main script is setup and configure docker swarm.
    m1 - m2 - m3 is managers
    w1  - w2 - w3 - w4 - w5 is workers
tasks/swarm.yml
see files
templates/docker.services - for systemd works
daemon.json.j2 - add daemon json
template/hosts - have to be like this
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
127.0.0.1 localhost
192.168.x.x prod-m-01
192.168.x.x prod-m-02
192.168.x.x prod-m-03
192.168.x.x prod-w-01
192.168.x.x prod-w-02
192.168.x.x prod-w-03
192.168.x.x prod-w-04
192.168.x.x prod-w-05
192.168.x.x nexus 
192.168.x.x bitbucket 
192.168.x.x zabbix 



9 - This script for add all this zoo to zabbix. After you have to add in zabbix server by hands.
tasks/zabbix.yml
change zabbix ip external, zabbix ip internal.





ps

 ansible --version
ansible 2.7.1
 - i use linux CentOS-7-x86_64-Minimal-1804.iso
 - for vmware first create template, later clone template to virtual machine.
 - Servers will reboot some times - is not trouble.

configure file tasks/swarm.yml and change delegate_to - everywhere.





