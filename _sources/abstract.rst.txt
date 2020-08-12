Abstract
============

About playbooks
---------------

This is a library of playbooks to construct HDFS/YARN clusters with some kinds of Big Data tools, such as Apache Spark.
You can construct a Hadoop cluster with HA as well as a pseudo Hadoop environment.

The roles contains only basic configurations.
I recommend that you customize or parameterize roles to configure systems appropriately for your workload.

Feature
--------
The project contains the following features.

* Examples of the inventry file of Ansible
* Basic configurations provided via role variables and group_vars
* Roles to configure and operate middleware
* Playbooks to configure and operate middleware

The main products which this project can deploy are:

* Bigtop based Apache Hadoop cluster
  * Pseudo environment
  * Distributed environment with NameNode and ResourceManager HA
* Bigtop and community based Apache Spark

.. _sec-servers:

Servers
--------
This project's assumption about middleware components and servers.

**Servers for medium cluster**

======== ================================================================================
Server   Use for
======== ================================================================================
master01 Primary NameNode, JournalNode, ZooKeeper Server(id=1), Ganglia Slave
master02 JournalNode, ZooKeeper Server(id=2), Primary ResourceManager,
         Ganglia Slave
master03 JournalNode, ZooKeeper Server(id=3), HistoryServer, Standby ResourceManager,
         Standby NameNode, Ganglia Slave, Ganglia Master, InfluxDB, Grafana, Spark History Server
client01 Hadoop Client, Spark Client, Ganglia Slave, Zeppelin
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
master02 Standby NameNode, Ganglia Slave
master03 Primary ResourceManager, Ganglia Slave
master04 Standby ResourceManager, Ganglia Slave
master05 JournalNode, ZooKeeper Server(id=1), Ganglia Slave
master06 JournalNode, ZooKeeper Server(id=2), Ganglia Slave
master07 JournalNode, ZooKeeper Server(id=3), Ganglia Slave
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
         Spark, Spark History Server
======== ================================================================================

Software information
--------------------

======================== ========================================================
Software                 Version
======================== ========================================================
OS                       (I use CentOS 7)
Ansible                  (I use 2.9.9)
Hadoop                   2.8.5 (Bigtop 1.4.0)
Spark                    2.2.3 (Bigtop 1.4.0)
Spark Community version  3.0.0
======================== ========================================================

Prerequirement
----------------
* Login to each server by SSH from the server where you execute ansible.
* "sudo" as admin user in each server.

