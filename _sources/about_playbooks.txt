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

**abstract**

============================ =============================================================
Directory                    Use for
============================ =============================================================
playbooks/conf/common        Common and basic configuration
playbooks/conf/cdh5          Configuration of CDH5 environment
playbooks/conf/cdh5_pseudo   Configuration of CDH5 Pseudo environment
playbooks/conf/ansible       Configuration of Ansible environment
playbooks/conf/ganglia       Configuration of Ganglia
playbooks/conf/influxdb      Configuration of InfluxDB and Grafana
playbooks/conf/spark_comm    Configuration of Spark community edition
playbooks/conf/zeppelin      Configuration of Zeppelin community edition
playbooks/conf/fluentd       Configuration of fluentd and td-agent
playbooks/conf/kafka         Configuration of Kafka
playbooks/conf/confluent     Configuration of Confluent packages including Kafka
playbooks/conf/ambari        Configuration of Ambari
============================ =============================================================

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

cdh5_pseudo
~~~~~~~~~~~~~

This is a set of configurations to construct CDH5 pseudo environment.

* cdh5_pseudo.yml

  * You can build whole CDH5 pseudo environment.

* cdh5_spark.yml

  * You can build spark environment on CDH5 pseudo.

ansible
~~~~~~~

This is a set of configuration about Ansible environment.
If you have manually configured Ansible environment, such as ansible.cfg, inventory file and so on,
you don't need these playbooks.

* ansible_client.yml

  + This playbook executes "ansible" role to configure nodes where we execute ansible command

* ansible_remote.yml

  + This playbook executes "ansible_remote" role to configure nodes which are configured by ansible

ganglia
~~~~~~~~~

This is  a set of configuration about Ganglia.
We have two playbooks for Ganglia master and slave.

* ganglia_all.yml

  + The wrapper playbook of configuration of both of Ganglia master and slave

* ganglia_master.yml

  + The playbook to configure Ganglia master

* ganglia_slave.yml

  + The playbook to configure Ganglia slave

influxdb
~~~~~~~~~
* all.yml

  + Configure influxdb and Grafana.

spark_comm
~~~~~~~~~~~
* all.yml

  + Configure all nodes

* spark_base.yml

  + Execute basic configuration of Spark

* spark_client.yml

  + Configure client environment to develop Spark applications

* spark_history.yml

  + Configure environment to run Spark history server 

* spark_libs.yml

  + Configure library environment to use native libraries in MLlib

zeppelin
~~~~~~~~~~~
* zeppelin.yml

  + Configure zeppelin environment

fluentd
~~~~~~~~~~~~
* fluentd.yml

  + Configure fluentd

* td_agent.yml

  + Configure td-agent

kafka
~~~~~~~~~~~~
* kafka_brocker.yml

  + Configure Kafka broker nodes.

confluent
~~~~~~~~~~~~
* kafka.yml

  + Configure Confluent packages including Kafka

ambari
~~~~~~~~~~~~
* ambari_agent.yml

  + Configure Ambari agent manually (Not through Ambari server)

* ambari_server.yml

  + Configure Ambari server

Playbooks for operation
-----------------------

We can use playbooks in playbooks/operation directory to operate services.

================================ ====================================================================
Directory                        Use for
===============================- ====================================================================
playbooks/operation/cdh5         Operation of Hadoop Services.
                                 e.g. Formating HDFS, Start/Stop services, ...
playbooks/operation/cdh5_pseudo  Operation of Hadoop Services.
                                 e.g. Formating HDFS, Start/Stop services, ...
playbooks/operation/common       Operations about SSH key exchange.
playbooks/operation/ec2          Operation to boot EC2 instances
playbooks/operation/httpd        Start and stop httpd
playbooks/operation/influxdb     Operation about InfluxDB initilization
playbooks/operation/spark_com    Operation of Spark services and building Spark packages
playbooks/operation/zeppelin     Start and stop zeppelin services
playbooks/operation/fluentd      Start and stop td-agent services
playbooks/operation/kafka        Start and stop Kafka cluster
playbooks/operation/confluent    Start and stop Confluent services including Kafka
playbooks/operation/ambari       Setup Ambari server.
                                 Start and stop each services.
================================ ====================================================================

cdh5
~~~~

This is a set of operation of Hadoop services.
Please check README in the *cdh5* directory for more information.

ec2
~~~~
This is a set of operation to boot EC2 instances.
Please check README in the *ec2* directory for more information.

influxdb
~~~~~~~~
* create_db.yml
  
  + Create all databases in InfluxDB.

* create_graphite_db.yml

  + Create database in InfluxDB, which hold data gathered by Graphite's protocol.
    This is mainly used by Spark.

* create_grafana_db.yml

  + Create database in InfluxDB, which hold Grafana's dashboard data.

spark_comm
~~~~~~~~~~~
* make_spark_packages.yml

  + Compile Spark sources and build packages

* start_spark_historyserver.yml

  + Start Spark's history server

* stop_spark_historyserver.yml

  + Stop Spark's history server

zeppelin
~~~~~~~~~~
* build.yml

  + Compile and package Zeppelin
  + This is helper playbook to build Zeppelin.
    You can build Zeppelin according to Zeppelin official web site.

* restart_zeppelin.yml

  + Stop and start Zeppelin serives

* start_zeppelin.yml

  + Start zeppelin services by executing zeppelin-daemon.sh

* stop_zeppelin.yml

  + Stop zeppelin services by executing zeppelin-daemon.sh

fluentd
~~~~~~~~~~~~~~~~~~~~~
* restart_td_agent.yml

  + Stop and Start td-agent

* start_td_agent.yml

  + Start td-agent

* stop_td_agent.yml

  + Stop td-agent

kafka
~~~~~~~~~~~~~~~~~~~~~
* restart_kafka.yml

  + Stop and Start kafka

* start_kafka.yml

  + Start kafka

* stop_kafka.yml

  + Stop kafka

* create_topic.yml

  + Create topic on Kafka cluster

* delete_topic.yml

  + Delete topic on Kafka cluster

confluent
~~~~~~~~~~~~~
* restart_kafka_rest.yml

  + Stop and Start REST Proxy service

* restart_kafka_server.yml

  + Stop and Start Kafka broker service

* restart_zookeeper_server.yml

  + Stop and Start ZooKeeper serivce
  + If you configured ZooKeeper service on Kafka broker nodes,
    you can use this playbook to control such ZooKeeper serivces.

* start_kafka_rest.yml

  + Start Kafka REST Proxy serivce

* start_kafka_server.yml

  + Start Kafka broker service

* start_schema_registry.yml

  + Start Confluent schema registry service

* start_zookeeper_server.yml

  + Start ZooKeeper serivce
  + If you configured ZooKeeper service on Kafka broker nodes,
    you can use this playbook to control such ZooKeeper serivces.

* stop_kafka_rest.yml

  + Stop Kafka REST Proxy serivce

* stop_kafka_server.yml

  + Stop Kafka broker serivce

* stop_schema_registry.yml

  + Stop Confluent schema registry service

* stop_zookeeper_server.yml

  + Stop ZooKeeper serivce
  + If you configured ZooKeeper service on Kafka broker nodes,
    you can use this playbook to control such ZooKeeper serivces.


ambari
~~~~~~~~~~~~
* To setup Ambari server

  + setup.yml

* Starting and stopping each service

  + restart_all.yml
  + restart_ambari_metrics.yml
  + restart_hdfs.yml
  + restart_yarn.yml
  + restart_zookeeper.yml
  + start_all.yml
  + start_ambari_metrics.yml
  + start_hdfs.yml
  + start_yarn.yml
  + start_zookeeper.yml
  + stop_all.yml
  + stop_ambari_metrics.yml
  + stop_hdfs.yml
  + stop_yarn.yml
  + stop_zookeeper.yml
