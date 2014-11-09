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
playbooks/conf/common | common and basic configuration
playbooks/conf/cdh5 | configuration of CDH5 environment
playbooks/conf/ansible | configuration of Ansible environment
