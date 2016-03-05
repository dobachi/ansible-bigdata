About roles
============
We manage OS and middlewares by using several seperated roles.

Roles to configure basic environments
----------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
common           Basic configuration about OS, basic services, and so on
prompt           Configuration of console prompt
screen           Configuration of screen command
user             Configuration of users
epel             Configuration of EPEL Repository
jdk              Configuraiotn of Oracle JDK
scala            Configuraiton of Scala on Hadoop client node
sbt              Configuration of Sbt
activator_mini   Configuraiton of Activator mini
================ =======================================================

Roles to configure Ansible
-----------------------------

================ =======================================================
Role name        Use for
================ =======================================================
ansible          Configuration of nodes where you executes ansible command
ansible_remote   Configuration of nodes which is configured ansible
================ =======================================================

Roles to boot EC2 instances for Hadoop cluster
------------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
ec2_hadoop       Boot EC2 instances for Hadoop cluster
================ =======================================================

Roles to configure CDH5 Hadoop
----------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
cdh5_base        Basic configuraiton about Hadoop
cdh5_jn          Configuration of JournalNode
cdh5_nn          Configuraiton of NameNode
cdh5_ot          Configuraiton of HistoryServer and YARN Proxy
cdh5_rm          Configuraiton of ResourceManager
cdh5_sl          Configuration of DataNode and NodeManager
zookeeper_server Configuration of Zookeeper server
================ =======================================================

Roles to configure CDH5 pseudo Hadoop
---------------------------------------
================ =======================================================
Role name        Use for
================ =======================================================
cdh5_pseudo      Basic configuraiton about Hadoop pseudo environment
================ =======================================================

Roles to configure Spark core on client node
------------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
cdh5_spark       Configuration of Spark core on Hadoop client node
================ =======================================================

Roles to configure Ganglia
------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
ganglia_master   Configuration of Ganglia Master and Web frontend
ganglia_slave    Configuration of Ganglia Slave
================ =======================================================

Roles to configure InfluxDB and Grafana
------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
influxdb         Configuration of InfluxDB
grafana          Configuration of Grafana
================ =======================================================

Roles to configure Spark community edition
-------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
spark_comm       Configuration of Spark community edition
================ =======================================================

Roles to configure Zeppelin
-------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
zeppelin         Configuration of Zeppelin community edition
================ =======================================================

Roles to configure fluentd or td-agent
-------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
fluentd          Configuration of fluentd (community edition)
td_agent         Configuration of td-agent
================ =======================================================

Roles to configure Kafka 
-------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
kafka            Configuration of Kafka cluster
================ =======================================================

Roles to configure Confluent
-------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
confluent_kafka  Configuration of Confluent packages
================ =======================================================

Roles to configure Ambari
-------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
ambari_server    Configuration of Ambari server
ambari_agent     Configuration of Ambari agent
================ =======================================================
