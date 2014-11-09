# Ansible Playbooks to construct HDFS/YARN Cluster

## About playbooks

This is a library of playbooks to construct HDFS/YARN clusters.
You will have HDFS services with HA and YARN services without HA.

## Feature

* HDFS with NameNode HA
* YARN
* Spark core on Client node

## Servers

Server | Use for
--- | ---
master01 | Primary NameNode, JournalNode, Zookeeper Server(id=1)
master02 | Backup NameNode, JournalNode, Zookeeper Server(id=2), ResourceManager
master03 | Backup NameNode, JournalNode, Zookeeper Server(id=3), HistoryServer
client01 | Hadoop Client, Spark Core
slave01 | DataNode, NodeManager

## Playbooks for configuration

We can use playbooks in playbooks/conf directory to configure servers.

Directory | Use for
--- | ---
playbooks/conf/common | Common and basic configuration
playbooks/conf/cdh5 | Configuration of CDH5 environment
playbooks/conf/ansible | Configuration of Ansible environment

### common

This is a set of common and basic configurations.

* playbooks/conf/common/common_all.yml

  * This playbook executes all basic roles

* playbooks/conf/common/common_only_common.yml

  * This playbook only executes "common" role

### cdh5

This is a set of configurations to construct CDH5 environment.

* cdh5_all.yml

  * This playbook is a comprehensive playbook which includes all other playbooks.
    You can build whole CDH5 environment.

* cdh5_cl.yml

  * This playbook executes basic roles and "cdh5_cl" role to build Hadoop Client environment

* cdh5_journalnode.yml

  * This playbook executes basic roles and "cdh5_jn" role to build HDFS JournalNode environment

* cdh5_namenode.yml

  * This playbook executes basic roles and "cdh5_nn" role to build HDFS NameNode environment

* cdh5_other.yml

  * This playbook executes basic roles and "cdh5_ot" role to build MapReduce HistoryServer and YARN Proxy environments

* cdh5_resourcemanager.yml

  * This playbook executes basic roles and "cdh5_rm" role to build YARN ResourceManager environment

* cdh5_slave.yml

  * This playbook executes basic roles and "cdh5_sl" role to build HDFS DataNode and YARN NodeManager environments

* cdh5_spark.yml

  * This playbook executes basic roles and "cdh5_spark" role to build Spark Core environment on Client Node

* cdh5_zookeeper.yml

  * This playbook executes basic roles and "zookeeper_server" role to build Zookeeper environment

### ansible

This is a set of configuration about Ansible environment.

* ansible_client.yml

  * This playbook executes "ansible" role to configure nodes where we execute ansible command

* ansible_remote.yml

  * This playbook executes "ansible_remote" role to configure nodes which are configured by ansible

## Playbooks for operation

We can use playbooks in playbooks/operation directory to operate services.

Directory | Use for
--- | ---
playbooks/operation/cdh5 | Operation of Hadoop Services. e.g. Formating HDFS, Start/Stop services, ...

### cdh5

This is a set of operation of Hadoop services.
Please check README of the directory for more information.

## Abstarct of roles

Role name | Use for
--- | ---
ansible | Configuration of nodes where you executes ansible command
ansible_remote | Configuration of nodes which is configured ansible
cdh5_base | Basic configuraiton about Hadoop
cdh5_jn | Configuration of JournalNode
cdh5_nn | Configuraiton of NameNode
cdh5_ot | Configuraiton of HistoryServer and YARN Proxy
cdh5_rm | Configuraiton of ResourceManager
cdh5_sl | Configuration of DataNode and NodeManager
cdh5_spark | COnfiguration of Spark core on Hadoop client node
common | Basic configuration about OS, basic services, and so on
jdk | Configuraiotn of Oracle JDK
scala | Configuraiton of Scala on Hadoop client node
screen | Configuration of screen command
zookeeper_server | Configuration of Zookeeper server

