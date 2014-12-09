How to use this playbooks
==========================
You can use these playbooks to configure the following configuration.
These are independent of each other.

* Ansible client environment to use various Ansible functions
* Host name configuration
* CDH5 HDFS/YARN environment

Assumption
-------------
You have nodes like servers_.

Configure Ansible node
------------------------
First, you may need to configure Ansible node.

Install EPEL repository::

 $ sudo yum install -y http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-2.noarch.rpm

Install Ansible::

 $ sudo yum install ansible

Move original /etc/ansible::

 $ cd /etc
 $ sudo mv ansible ansible.org

Clone this repository::

 $ git clone https://github.com/dobachi/ansible-cdh5.git ansible

Modify hosts file to be copied to /etc/hosts::

 $ cd ansible
 $ sudo vi roles/common/files/hosts.default

Execute ansible-playbook command with common_all.yml::

 $ ansible-playbook playbooks/conf/common/common_all.yml -k -s -i hosts.sample -e "common_hosts_replace=True"

Copy ansible's hosts and modify it::

 $ sudo vi roles/ansible/templates/hosts.default.j2

Execute ansible-playbook command with ansible_client.yml::

 $ ansible-playbook playbooks/conf/ansible/ansible_client.yml -k -s -i hosts.sample -e "ansible_environment=default ansible_modify_cfg=True"

If you use EC2 and need a private key for SSH,
you should specify "ansible_private_key_file" paramter.
You should execute command with the parameter instead of the above command::

 $ ansible-playbook playbooks/conf/ansible/ansible_client.yml -k -s -i hosts.sample -e "ansible_environment=default ansible_modify_cfg=True ansible_private_key_file=${HOME}/mykey.pem"

Check whether all nodes are reachable and "sudo" is available::

 $ ansible -m ping cdh5_all -k -s

Configure host name of clusters (Option)
------------------------------------------

If you want to configure hostname of nodes,
You can use "common" role.

Execute ansible-playbook command with common_only_common.yml::

 $ cd /etc/ansible
 $ ansible-playbook playbooks/conf/common/common_only_common.yml -k -s -e "common_config_hostname=True server=cdh5_all"

This is usefull for configuration of EC2 instance, because your node may have variety of hostname after each node booted.

Construct CDH5 HDFS/YARN environment
----------------------------------------
If you already have nodes, you can construct CDH5 HDFS/YARN environment by ansible-playbook command.
In the following example, we configure common_hosts_replace is True.
As a result of this parameter configuration, Ansible replace /etc/hosts
by Ansible driver server's /etc/ansible/roles/common/files/hosts.default::

 $ ansible-playbook playbooks/conf/cdh5/cdh5_all.yml -k -s -e "common_hosts_replace=True"
 $ ansible-playbook playbooks/operation/cdh5/init_zkfc.yml -k -s 
 $ ansible-playbook playbooks/operation/cdh5/init_hdfs.yml -k -s 

Start services::

 $ ansible-playbook playbooks/operation/cdh5/start_cluster.yml -k -s 

Install Spark Core
---------------------
You can install Spark Core into Client node by the following command::

 $ ansible-playbook playbooks/conf/cdh5/cdh5_spark.yml -k -s
