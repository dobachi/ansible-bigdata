About hosts
==============
We have two types of Ansible hosts sample.
The following is the location of the main services.

hosts.medium_sample:

 * Master Services(NameNode, Zookeeper, JournalNode and ResourceManager): 3 nodes
 * Client: 1 node
 * Slave: 5 nodes

hosts.large_sample:

 * NameNode: 2 nodes
 * Zookeeper and JournalNode: 3 nodes
 * ResourceManager: 2 nodes
 * Client: 1 node
 * Slave: 10 nodes

.. note::

   Both hosts sample includes "hadoop_pseudo" group,
   which is the pseudo Hadoop environment.
