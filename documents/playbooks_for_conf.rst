Playbooks for configuration
----------------------------
The playbooks in "playbooks/conf" directory provide functions
to configure nodes.

In this section, the short descriptions for each playbook are shown.

common
~~~~~~

This is a set of common and basic configurations including OS parameters.

* playbooks/conf/common/common_all.yml

  + The playbook which provides all basic configurations.

* playbooks/conf/common/common_only_common.yml

  + The playbook which provides configurations only in "common" role

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
* kafka_broker.yml

  + Configure Confluent Kafka brokers

* kafka_schema.yml

  + Configure Confluent Schema Registry

* kafka_rest.yml

  + Configure Confluent REST Proxy

ambari
~~~~~~~~~~~~
* ambari_agent.yml

  + Configure Ambari agent manually (Not through Ambari server)

* ambari_server.yml

  + Configure Ambari server

jenkins
~~~~~~~~~~~~
* jenkins.yml

  + Configure Jenkins server

anacondace
~~~~~~~~~~~~
* anacondace2.yml

  + Configure Anaconda2 CE

* anacondace3.yml

  + Configure Anaconda3 CE

postgresql
~~~~~~~~~~~~
* postgresql.yml

  + Configure PostgreSQL

cdh5_hive
~~~~~~~~~~~~
* cdh5_hive.yml

  + Configure Hive and PostgreSQL

alluxio_yarn
~~~~~~~~~~~~
* alluxio_yarn.yml

  + Configure Alluxio on YARN

    - Configure client and slave nodes

tpc_ds
~~~~~~~~
* tpc_ds.yml

  + Configure TPC-DS packages

tensorflow
~~~~~~~~~~~~~~~~
* gpu_env.yml

  + Configure CUDA and cuDNN environment

