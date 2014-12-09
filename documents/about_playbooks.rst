About playbooks
=========================
This project has two types of playbooks.

* Playbooks for configuration

  + These are used to install middlewares and configure parameters of OS and middlewares.

* Playbooks for operation

  + These are used to operate OS's services and middleware services.

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

