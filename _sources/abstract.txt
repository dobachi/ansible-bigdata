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
* Configuring CDH5's Spark core on Client node
* Configuring Spark community edition's core on Client node
* Configuring Ganglia for the resource visualization
* Configuring InfluxDB and Grafana for the metrics visualization
* Configuring Pseudo environment for test
* Configuring Zeppelin for Spark notebook
* Configuring fluentd and td-agent
* Configuring Kafka cluster
* Configuring Confluent cluster
* Configuring Ambari

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
client01 Hadoop Client, Spark Core, Ganglia Slave, Zeppelin
slave01  DataNode, NodeManager, Ganglia Slave
slave02  DataNode, NodeManager, Ganglia Slave
slave03  DataNode, NodeManager, Ganglia Slave
slave04  DataNode, NodeManager, Ganglia Slave
slave05  DataNode, NodeManager, Ganglia Slave
kafka01  Kafka broker
kafka02  Kafka broker
kafka03  Kafka broker
manage   Ambari server
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
client01 Hadoop Client, Spark Core, Ganglia Slave, Zeppelin
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
kafka01  Kafka broker
kafka02  Kafka broker
kafka03  Kafka broker
manage   Ambari server
======== ================================================================================

**Server for pseudo environment**

======== ================================================================================
Server   Use for
======== ================================================================================
pseudo   NameNode, DataNode, SecondaryNameNode, ResourceManager, NodeManager,
         Spark-core, Spark history server
======== ================================================================================

* You can also configure InfluxDB, Grafana, Zeppelin by using playbooks

Software information
--------------------

======== ========================================================
Software Version
======== ========================================================
OS       CentOS6.6 or CentOS7.0
Hadoop   CDH5.3
Spark    Spark1.2 of CDH5
         or Spark community edition
Ansible  Ansible 1.8.2 of EPEL
InfluxDB Latest version of the community
Graphana 1.9.1 of the community
Zeppelin Latest version of the community
Kafka    0.9.0.0
======== ========================================================

Prerequirement
----------------
* You can login to each server by SSH from the server where you execute ansible.
* You can use "sudo" in each server.
  If you cannot use sudo, you should access remote servers by root user.
* When you manage EC2 instances, you need to use RSA key to login servers.
  For example, CentOS6 community instance use RSA key with root user in SSH access.
