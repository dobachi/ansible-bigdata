About hosts(inventory file)
===================================
This project provides two types of Ansible hosts sample.
The following is the location of the main services.

hosts.medium_sample:

 * Master Services including NameNode and ResourceManager: 3 nodes
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

About groups in inventory
==================================
This example of inventory includes the following configured groups.

**Main group**

========================= ===========================================================================
group                     description
========================= ===========================================================================
production                The top group which represents the whole of the environment,
                          such as data centers.
                          This group is used to define the environmental specific parameters.
local                     The dummy group to define localhost in inventory.
hadoop_all                The group whih represents the whole of Hadoop cluster.
                          This group includes all groups and nodes in the Hadoop cluster.
hadoop_master             This group represents all master nodes.
hadoop_namenode           This group represents the primary NameNode and the backup NameNode
hadoop_journalnode        This group represents JournalNodes.
hadoop_zookeeperserver    This group represents Zookeeper nodes.
                          *Important* : The parameter "zookeeper_server_id" is
                          configured with each nodes.
hadoop_resourcemanager    This group represents ResourceManagers
hadoop_other              This group represents nodes which provide Hadoop-related services,
                          such as HistoryServer.
hadoop_slave              This group represents slave nodes.
hadoop_client             This group represents client nodes.
                          The client nodes are used to execute commands to access Hadoop services
                          and other related services.
hadoop_pseudo             This group represents a node which provides Hadoop pseudo environment.
                          This is mainly used for the application development.
manage                    This group represents nodes which provides the management services,
                          such as Ganglia and Graphite.
kafka_cluster             This group represents Kafka brokers of Apache Kafka (Community version)
confluent_kafka_cluster   This group represents Kafka brokers of Confluent Kafka
confluent_schema_registry This group represents Confluent's schema registry service nodes
confluent_kafka_rest      This group represents Confluent's REST Proxy serivce nodes
data_loader               This group represents nodes which provide the services to load data
                          to the cluster. e.g. fluentd and td-agent
endosnipe                 This group represents nodes which provide EndoSNipe servides, such as
                          a dashbord.
heapstats                 This group represents nodes which use heapstats to monitor JVM processes.
========================= ===========================================================================

Managing several clusters
---------------------------
If you want to manage several Hadoop clusters or environments,
you can distinguish these environments by using different inventries which have different top-level groups.

e.g. the production environments, the test environments, the development environments and so on.

The group variables of each group define parameters specific to each environment.

Example

* group_vars/all/something ... This file provides default parameters common for all environments.
* group_vars/production/something ... This file provides parameters common for the production environments.
* group_vars/test/something ... This file provides parameters common for the test environments.

.. vim ft=rst tw=0
