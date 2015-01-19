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

* Configuring Ansible execution environment
* Booting EC2 instances for Hadoop cluster
* Configuring HDFS with NameNode HA
* Configuring YARN with ReourceManager HA
* Configuring Spark core on Client node

.. _sec-servers:

Servers
--------
This project's assumption about middleware components and servers.

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
