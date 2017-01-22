Playbooks for operation
-----------------------

We can use playbooks in playbooks/operation directory to operate services.

================================ ====================================================================
Directory                        Use for
================================ ====================================================================
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
playbooks/operation/postgresql   Start and stop PostgreSQL. Initialize PostgreSQL DB.
playbooks/operation/cdh5_hive    Set up the metastore. Start and stop
playbooks/operation/alluxio_yarn Deploy Alluxio cluster to YARN
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

postgresql
~~~~~~~~~~~~~~~~~~
* setup db

  + initdb.yml

* start and stop postgresql

  + start_postgresql.yml
  + stop_postgresql.yml
  + restart_postgresql.yml

cdh5_hive
~~~~~~~~~~~~~
* setup

  + create_metastore_db.yml

* start and stop services

  + start_metastore.yml
  + stop_metastore.yml

deploy_yarn
~~~~~~~~~~~~~~
* deploy Alluxio application to YARN

  + deploy_alluxio.yml

.. vim: ft=rst tw=0
