Ansible Playbooks to construct HDFS/YARN Cluster
==================================================

.. sectnum::
.. contents::

About playbooks
---------------

This is a library of playbooks to construct HDFS/YARN clusters.
You will have HDFS services with HA and YARN services without HA.

To write these playbooks, `dobachi's ansible-playbooks <https://bitbucket.org/dobachi/ansible-playbooks.git>`_
and `mcsrainbow's ansible-playbooks-cdh5 <https://github.com/mcsrainbow/ansible-playbooks-cdh5>`_ are used as reference.

Feature
--------

* HDFS with NameNode HA
* YARN
* Spark core on Client node

.. _servers:

Servers
--------

======== ================================================================================
Server   Use for
======== ================================================================================
master01 Primary NameNode, JournalNode, Zookeeper Server(id=1)
master02 Backup NameNode, JournalNode, Zookeeper Server(id=2), Primary ResourceManager
master03 JournalNode, Zookeeper Server(id=3), HistoryServer, Backup ResourceManager
client01 Hadoop Client, Spark Core
slave01  DataNode, NodeManager
slave02  DataNode, NodeManager
slave03  DataNode, NodeManager
slave04  DataNode, NodeManager
slave05  DataNode, NodeManager
======== ================================================================================

Playbooks for configuration
----------------------------

We can use playbooks in playbooks/conf directory to configure servers.

====================== ==========================================
Directory              Use for
====================== ==========================================
playbooks/conf/common  Common and basic configuration
playbooks/conf/cdh5    Configuration of CDH5 environment
playbooks/conf/ansible Configuration of Ansible environment
====================== ==========================================

common
~~~~~~

This is a set of common and basic configurations.

* playbooks/conf/common/common_all.yml

  + This playbook executes all basic roles

* playbooks/conf/common/common_only_common.yml

  + This playbook only executes "common" role

cdh5
~~~~

This is a set of configurations to construct CDH5 environment.

* cdh5_all.yml

  + This playbook is a comprehensive playbook which includes all other playbooks.
    You can build whole CDH5 environment.

* cdh5_cl.yml

  + This playbook executes basic roles and "cdh5_cl" role to build Hadoop Client environment

* cdh5_journalnode.yml

  + This playbook executes basic roles and "cdh5_jn" role to build HDFS JournalNode environment

* cdh5_namenode.yml

  + This playbook executes basic roles and "cdh5_nn" role to build HDFS NameNode environment

* cdh5_other.yml

  + This playbook executes basic roles and "cdh5_ot" role to build MapReduce HistoryServer and YARN Proxy environments

* cdh5_resourcemanager.yml

  + This playbook executes basic roles and "cdh5_rm" role to build YARN ResourceManager environment

* cdh5_slave.yml

  + This playbook executes basic roles and "cdh5_sl" role to build HDFS DataNode and YARN NodeManager environments

* cdh5_spark.yml

  + This playbook executes basic roles and "cdh5_spark" role to build Spark Core environment on Client Node

* cdh5_zookeeper.yml

  + This playbook executes basic roles and "zookeeper_server" role to build Zookeeper environment

ansible
~~~~~~~

This is a set of configuration about Ansible environment.

* ansible_client.yml

  + This playbook executes "ansible" role to configure nodes where we execute ansible command

* ansible_remote.yml

  + This playbook executes "ansible_remote" role to configure nodes which are configured by ansible

Playbooks for operation
-----------------------

We can use playbooks in playbooks/operation directory to operate services.

========================= ====================================================================
Directory                 Use for
========================= ====================================================================
playbooks/operation/cdh5  Operation of Hadoop Services.
                          e.g. Formating HDFS, Start/Stop services, ...
========================= ====================================================================

cdh5
~~~~

This is a set of operation of Hadoop services.
Please check README of the directory for more information.

Abstarct of roles
-----------------

================ =======================================================
Role name        Use for
================ =======================================================
ansible          Configuration of nodes where you executes ansible command
ansible_remote   Configuration of nodes which is configured ansible
cdh5_base        Basic configuraiton about Hadoop
cdh5_jn          Configuration of JournalNode
cdh5_nn          Configuraiton of NameNode
cdh5_ot          Configuraiton of HistoryServer and YARN Proxy
cdh5_rm          Configuraiton of ResourceManager
cdh5_sl          Configuration of DataNode and NodeManager
cdh5_spark       Configuration of Spark core on Hadoop client node
common           Basic configuration about OS, basic services, and so on
jdk              Configuraiotn of Oracle JDK
scala            Configuraiton of Scala on Hadoop client node
screen           Configuration of screen command
zookeeper_server Configuration of Zookeeper server
================ =======================================================

How to use this playbooks
--------------------------
You can use these playbooks to configure the following configuration.
These are independent of each other.

* Ansible client environment to use various Ansible functions
* Host name configuration
* CDH5 HDFS/YARN environment

Assumption
~~~~~~~~~~
You have nodes like servers_.

Configure Ansible node
~~~~~~~~~~~~~~~~~~~~~~
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
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to configure hostname of nodes,
You can use "common" role.

Execute ansible-playbook command with common_only_common.yml::

 $ cd /etc/ansible
 $ ansible-playbook playbooks/conf/common/common_only_common.yml -k -s -e "common_config_hostname=True server=cdh5_all"

This is usefull for configuration of EC2 instance, because your node may have variety of hostname after each node booted.

Construct CDH5 HDFS/YARN environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If you already have nodes, you can construct CDH5 HDFS/YARN environment by ansible-playbook command::

 $ ansible-playbook playbooks/conf/cdh5/cdh5_all.yml -k -s 
 $ ansible-playbook playbooks/operation/cdh5/init_zkfc.yml -k -s 
 $ ansible-playbook playbooks/operation/cdh5/init_hdfs.yml -k -s 

Start services::

 $ ansible-playbook playbooks/operation/cdh5/start_cluster.yml -k -s 

Install Spark Core
~~~~~~~~~~~~~~~~~~
You can install Spark Core into Client node by the following command::

 $ ansible-playbook playbooks/conf/cdh5/cdh5_spark.yml -k -s

.. vim: ft=rst tw=0
