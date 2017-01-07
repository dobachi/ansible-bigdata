About hosts(inventory file)
===================================
We have two types of Ansible hosts sample.
The following is the location of the main services.

hosts.medium_sample:

 * Master Services(NameNode, Zookeeper, JournalNode and ResourceManager): 3 nodes
 * Client: 1 node
 * Slave: 5 nodes
 * Manage: 1 nodes
 * Kafka: 3 nodes

hosts.large_sample:

 * NameNode: 2 nodes
 * Zookeeper and JournalNode: 3 nodes
 * ResourceManager: 2 nodes
 * Client: 1 node
 * Slave: 10 nodes
 * Manage: 1 nodes
 * Kafka: 3 nodes

.. note::

   Both hosts sample includes "hadoop_pseudo" group,
   which is the pseudo Hadoop environment.

About groups in inventory
==================================
This example of inventory includes the following configured groups.

**Main group**

========================= ===========================================================================
group                     description
========================= ===========================================================================
production                The top group which represent whole of this environment.
                          This group is used to define the environmental specific parameters
                          using group variables.
local                     The dummy group to define localhost in inventory
hadoop_all                This group includes all groups and nodes in Hadoop cluster
hadoop_master             This group includes all groups and nodes which provides
                          Hadoop's master service, such as NameNodes.
hadoop_namenode           This group includes both of primary NameNode and backup NameNode
hadoop_journalnode        This group includes JournalNodes
hadoop_zookeeperserver    This group includes Zookeeper nodes.
                          The parameter "zookeeper_server_id" is configured with each nodes.
hadoop_resourcemanager    This group includes ResourceManagers
hadoop_other              This group includes node to provides Hadoop side services,
                          such as HistoryServer.
hadoop_slave              This group includes slave nodes of Hadoop
hadoop_client             This group includes all groups and nodes which is used
                          as clients of Hadoop and other services.
hadoop_pseudo             This group includes a ndoe which provide Hadoop pseudo environment.
manage                    This group includes nodes to control clusters
kafka_cluster             This group includes Kafka brokers of Apache Kafka (Community version)
confluent_kafka_cluster   This group includes Kafka brokers of Confluent Kafka
confluent_schema_registry This group includes Confluent's schema registry service nodes
confluent_kafka_rest      This group includes Confluent's REST Proxy serivce nodes
========================= ===========================================================================

If you are managing several Hadoop cluster and environment, such as production, test, etc,
you can use the top-level group to define such environment.
The group variables of each group may define parameters which are specific to each environment.

Example::

 group_vars/all/something        ... This includes default parameters for all environment
 group_vars/production/something ... This includes parameters which are specific to the production environment
 group_vars/test/something       ... This includes parameters which are specific to the test environment

If you define several inventory file which represent each environment,
you can use them to swich environments which you want to configure in executing ansible-playbook command.

