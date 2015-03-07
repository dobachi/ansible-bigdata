Abstract
============

About playbooks
---------------

This is a library of playbooks to construct HDFS/YARN clusters.
You will have HDFS and YARN services with HA.

Although the configuration of OS and middlewares are not well tuned about performance,
this is enough to be used to construct small cluster.
In other words, if you would like to construct and manage large clusters,
you may need to configure OS and middlewares adequately.

To write these playbooks, `dobachi's ansible-playbooks <https://bitbucket.org/dobachi/ansible-playbooks.git>`_
and `mcsrainbow's ansible-playbooks-cdh5 <https://github.com/mcsrainbow/ansible-playbooks-cdh5>`_ are used as reference.

Feature
--------
This project has the following functions.
Each function is available individually.

* Configuring Ansible execution environment
* Booting EC2 instances for Hadoop cluster
* Configuring HDFS/YARN with NameNode HA
* Configuring Spark core on Client node
* Configuring Ganglia for the resource visualization
* Configuring InfluxDB and Grafana for the metrics visualization
* Configuring Pseudo environment for test

.. _sec-servers:

Servers
--------
This project's assumption about middleware components and servers.

**Servers for medium cluster**

======== ================================================================================
Server   Use for
======== ================================================================================
master01 Primary NameNode, JournalNode, Zookeeper Server(id=1), Ganglia Slave
master02 Backup NameNode, JournalNode, Zookeeper Server(id=2), Primary ResourceManager,
         Ganglia Slave
master03 JournalNode, Zookeeper Server(id=3), HistoryServer, Backup ResourceManager,
         Ganglia Slave, Ganglia Master, InfluxDB, Grafana
client01 Hadoop Client, Spark Core, Ganglia Slave
slave01  DataNode, NodeManager, Ganglia Slave
slave02  DataNode, NodeManager, Ganglia Slave
slave03  DataNode, NodeManager, Ganglia Slave
slave04  DataNode, NodeManager, Ganglia Slave
slave05  DataNode, NodeManager, Ganglia Slave
======== ================================================================================

**Servers for large cluster**

======== ================================================================================
Server   Use for
======== ================================================================================
master01 Primary NameNode, Ganglia Slave
master02 Backup NameNode, Ganglia Slave
master03 Primary ResourceManager, Ganglia Slave
master04 Backup ResourceManager, Ganglia Slave
master05 JournalNode, Zookeeper Server(id=1), Ganglia Slave
master06 JournalNode, Zookeeper Server(id=2), Ganglia Slave
master07 JournalNode, Zookeeper Server(id=3), Ganglia Slave
master08 HistoryServer, Ganglia Master, Ganglia Slave, InfluxDB, Grafana
client01 Hadoop Client, Spark Core, Ganglia Slave
slave01  DataNode, NodeManager, Ganglia Slave
slave02  DataNode, NodeManager, Ganglia Slave
slave03  DataNode, NodeManager, Ganglia Slave
slave04  DataNode, NodeManager, Ganglia Slave
slave05  DataNode, NodeManager, Ganglia Slave
slave06  DataNode, NodeManager, Ganglia Slave
slave07  DataNode, NodeManager, Ganglia Slave
slave08  DataNode, NodeManager, Ganglia Slave
slave09  DataNode, NodeManager, Ganglia Slave
slave10  DataNode, NodeManager, Ganglia Slave
======== ================================================================================

**Server for pseudo environment**

======== ================================================================================
Server   Use for
======== ================================================================================
pseudo   NameNode, DataNode, SecondaryNameNode, ResourceManager, NodeManager,
         Spark-core, Spark history server
======== ================================================================================

Software information
--------------------

======== =============================
Software Version
======== =============================
OS       CentOS6.6 or CentOS7.0
Hadoop   CDH5.3
Spark    Spark1.2 of CDH5
Ansible  Ansible 1.8.2 of EPEL
======== =============================

Prerequirement
----------------
* You can login to each server by SSH from the server where you execute ansible.
* You can use "sudo" in each server
