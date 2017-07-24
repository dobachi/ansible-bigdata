How to use this playbooks
==========================
This project provides many kinds of playbooks to configure and manage
nodes and serivces.

.. note::

   To understand each playbook, please refer  :ref:`sec-about-playbooks` section.

Assumption of this section
----------------------------
* You should have servers described in :ref:`sec-servers` section.
* You should be able to access all hosts listed in the inventry which is created by the following procedure.
  e.g. You should configure /etc/hosts to access nodes by hostname in advance.

.. _sec-configure-ansible-env:

How to configure Ansible execution environment
----------------------------------------------
If you have not configured Ansible execution environment,
you can use the following playbooks for it.

In this section, we start from installing Ansible packages.

Install packages
~~~~~~~~~~~~~~~~
Install EPEL repository

.. code-block:: shell

 $ sudo yum install -y epel-release

Install Ansible

.. code-block:: shell

 $ sudo yum install -y ansible

Clone playbooks
~~~~~~~~~~~~~~~
Clone this projects to any paths.

E.g.

.. code-block:: shell

 $ cd ~
 $ mkdir Sources
 $ cd Sources
 $ git clone https://github.com/dobachi/ansible-hadoop.git ansible

Create inventry
~~~~~~~~~~~~~~~~~~~~~~
Create an inventry for your environment.
You can use examples of this project, hosts.mdedium_sample and hosts.large_sample.

.. code-block:: shell

 $ cp hosts.medium_sample hosts.test
 $ ln -s hosts.test hosts

Modify the top group of the inventry and hostnames in groups.

.. code-block:: shell

 $ vim hosts

Create ansible.cfg
~~~~~~~~~~~~~~~~~~~~~~
Create ansible.cfg refering an example of this project.

.. code-block:: shell

 $ cp ansible.cfg.sample ansible.cfg

The important differences of the default ansible.cfg,
which you can find /etc/ansible/ansible.cfg, is 

* hostfile = hosts

  + To use an inventry file in the current directory.

* library = /usr/share/ansible:library

  + To include "library" directory in the current dicrectory.

* roles_path = roles

  + To use roles in the current directory.

Try ping to all nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check whether all nodes are reachable and "sudo" is available

.. code-block:: shell

 $ ansible -m ping hadoop_all -k -s

How to boot EC2 instances for Hadoop cluster
------------------------------------------------
If you want to use Hadoop on EC2 instances,
you can use playbooks/operation/ec2/hadoop_nodes_up.yml to boot instances.

Define environment variables for AWS access
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We use environment variables to configure AWS access keys.
Define AWS_ACCESS_KEY and AWS_SECRET_KEY in your ~/.bashrc

::

 export AWS_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXx
 export AWS_SECRET_KEY=XXXXXXXXXXXXXXXXXXXXXXXXX

If you don't have AWS keys,
create keys while referring AWS web site.

Define parameters for ec2_hadoop role
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can find the parameter description for ec2_hadoop role in roles/ec2_hadoop/defaults/main.yml

To define your own parameters,
you need to create the group variable file (e.g. group_vars/all/ec2) and write parameter defines in this file.

The following is an example of group_vas/top.

::

 ec2_hadoop_group_id: sg-xxxxxxxx
 
 ec2_hadoop_accesskey: xxxxx
 
 ec2_hadoop_itype: xx.xxxxx
 
 ec2_hadoop_master_image: ami-xxxxxxxx
 ec2_hadoop_slave_image: ami-xxxxxxxx
 ec2_hadoop_client_image: ami-xxxxxxxx
 
 ec2_hadoop_region: xx-xxxxxxxxx-x
 
 ec2_hadoop_vpc_subnet_id: subnet-xxxxxxxx

If you don't define required parameters,
you will see some errors, like::

 One or more undefined variables: 'ec2_hadoop_group_id' is undefined

Apply playbook
~~~~~~~~~~~~~~~~~~
Execute ansible-playbook command.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/ec2/hadoop_nodes_up.yml -c local

As a result, you can find an IP address list, an ansible inventory file and an example of /etc/hosts used in EC2 instances
in /tmp/ec2_<unix epoc time>.
<unix epoc time> is the time you executed this playbook.

(supplement) When you restart ec2 instances
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
When you restart ec2 instances, public IP addresses may change.
You can obtain new IP address tables by executing the playbook.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/ec2/hadoop_nodes_up.yml -c local

How to configure host names of nodes
------------------------------------------
If you want to configure hostname of nodes,
You can use "common" role and related playbooks.

Execute ansible-playbook command with common_only_common.yml

.. code-block:: shell

 $ cd /etc/ansible
 $ ansible-playbook playbooks/conf/common/common_only_common.yml -k -s -e "common_config_hostname=True server=hadoop_all"

This is usefull for configuration of EC2 instance, because your node may have variety of hostname after each node booted.

How to configure CDH5 HDFS/YARN environment
--------------------------------------------
You can construct CDH5 HDFS/YARN environment by ansible-playbook command.

Preparement
~~~~~~~~~~~~
If you have not configured Ansible execution environment,
you should configure it.
You can reference :ref:`sec-configure-ansible-env` section.

Procedure
~~~~~~~~~
In the following example, we configure common_hosts_replace is True.
As a result of this parameter configuration, Ansible replace /etc/hosts
by Ansible driver server's /etc/ansible/roles/common/files/hosts.default

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5/cdh5_all.yml -k -s -e "common_hosts_replace=True"
 $ ansible-playbook playbooks/operation/cdh5/init_zkfc.yml -k -s 
 $ ansible-playbook playbooks/operation/cdh5/init_hdfs.yml -k -s 

Start services

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5/start_cluster.yml -k -s 

How to install Spark environment on CDH5 environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can install Spark Core into Client node by the following command

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5/cdh5_spark.yml -k -s

If you want to start Spark's history server,
please execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5/start_sparkhistory.yml -k -s


How to configure CDH5 Pseudo environment
--------------------------------------------
You can construct CDH5 HDFS/YARN environment by ansible-playbook command.

Preparement
~~~~~~~~~~~~
If you have not configured Ansible execution environment,
you should configure it.
You can reference :ref:`sec-configure-ansible-env` section.

Procedure
~~~~~~~~~
In the following example, we configure common_hosts_replace is True.
As a result of this parameter configuration, Ansible replace /etc/hosts
by Ansible driver server's /etc/ansible/roles/common/files/hosts.default

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5_pseudo/cdh5_pseudo.yml -k -s -e "common_hosts_replace=True"
 $ ansible-playbook playbooks/operation/cdh5_pseudo/init_hdfs.yml -k -s 

Start services

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5_pseudo/start_cluster.yml -k -s 

How to install Spark environment on Hadoop pseudo environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can install Spark Core into Client node by the following command

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5_pseudo/cdh5_spark.yml -k -s

If you want to start Spark's history server,
please execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5_pseudo/start_sparkhistory.yml -k -s

How to install Ganglia environment
---------------------------------------
You can install Gaglia services with the following command::

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ganglia/ganglia_all.yml -k -s

How to use unicast for communication between gmonds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This playbook uses multicast for communication between gmonds as default.
In some situcation, you may want to use unicast.
For example, you are using ec2 of AWS.

The parameter "ganglia_slave_use_unicast" is used to define
whether you use unicast or not.
If you set this parameter True in your group_vars, you can use unicast.

Example(group_vars/all/ganglia)::

 ganglia_slave_use_unicast: True

Please configure the parameter "ganglia_slave_host" as well as "ganglia_slave_use_unicast"
This parameter is used to define the destination which each gmond sends metrics,
and should be a representative node which gmetad connect.

How to install and configure InfluxDB and Grafana
-----------------------------------------------------
You can install InfluxDB and Grafana services with the followign command.
Be careful for a machine to which you install InfluxDB,
because InfluxDB uses 8088 port but Hadoop YARN ResourceManager also use 8088 port.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/influxdb/all.yml -k -s

You can access http://<Grafana server>:3000/ to watch grafana.

How to install Spark community edition
----------------------------------------

Obtain package or compile sources
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can get Spark pacakge from `Spark official download site <https://spark.apache.org/downloads.html>`_ .

If you want to use a package compiled by your self,
you should build it according to `Spark offical build procedure <https://spark.apache.org/docs/latest/building-spark.html>`_ .

You can also use playbooks/operation/spark_comm/make_spark_packages.yml to build it.
When you use this playbook, please specify the following parameters used in this playbook.

* spark_comm_src_dir
* spark_comm_version
* spark_comm_mvn_options
* spark_comm_hadoop_version

Confiure parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can use playbooks/conf/spark_comm/all.yml to configure Spark community edition envirionment.

This playbooks and roles expect to get Spark tar package by HTTP method.
You should configure the following parameter to specify where Ansible should get Spark tar package.

* spark_comm_package_url_base
* spark_comm_package_name

The download URL is consited like {{ spark_comm_package_url_base }}/{{ spark_comm_package_name }}.tgz
For example, if the download URL is "http://example.local/spark/spark-1.4.0-SNAPSHOT-bin-2.5.0-cdh5.3.2.tgz",
spark_comm_package_url_base is "http://example.local/spark" and spark_comm_package_name is "spark-1.4.0-SNAPSHOT-bin-2.5.0-cdh5.3.2".

.. note::

   spark_comm_package_name does not include ".tgz"

Execute playbooks
~~~~~~~~~~~~~~~~~~~~
After configuration of parameters, you can execute Ansible playbooks.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/spark_comm/all.yml -k -s

Stat history server
~~~~~~~~~~~~~~~~~~~~~~
Start Spark's history server by the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/spark_comm/start_spark_historyserver.yml -k -s

Configure Zeppelin
-----------------------------------

Obtain sources and build
~~~~~~~~~~~~~~~~~~~~~~~~~~
First, according to `Official README <https://github.com/apache/incubator-zeppelin/blob/master/README.md>`_ , you need to compile source codes and make a package.

Please take care about the compile option.
You should specify Spark and Hadoop versions you use now.

The following is an example to configure CDH5.3.3、Spark1.3、YARN environment.

.. code-block:: shell

 $ mvn clean package -Pspark-1.3 -Dhadoop.version=2.5.0-cdh5.3.3 -Phadoop-2.4 -Pyarn -DskipTests 

You can also use playbooks/operation/zeppelin/build.yml, the helper playbook.
Before executing this playbook, please configure the following parameters in the playbook.

* zeppelin_git_url
* zeppelin_src_dir
* zeppelin_version
* zeppelin_comiple_flag
* zeppelin_hadoop_version

Finally, the playbook to configure Zeppelin make use of the package
which you compiled the above procedure.
The package is downloaded from web service by HTTP,
so that you need to put the package on a HTTP web server.

Executing playbook
~~~~~~~~~~~~~~~~~~
To configure Zeppelin, please execute the following playbook.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/zeppelin/zeppelin.yml -k -s

After finishing configuration, you need to start Zeppelin service.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/zeppelin/start_zeppelin.yml -k -s


Configure Kafka cluster
-------------------------------

Information
~~~~~~~~~~~~~~~
We assume that Zookeeper ensemble was congured on master01, master02 and master03. 
If you have any other Zookeeper ensemble, you should modify kafka role's parameters.


Executing playbook
~~~~~~~~~~~~~~~~~~~~~~
To configure Kafka cluster, please execute the following playbook.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/kafka/kafka_broker.yml -k -s

After finishing configuration, you need to start Kafka cluster.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/kafka/start_kafka.yml -k -s

Configure Confluent services
-------------------------------

Information
~~~~~~~~~~~~~~~
We assume that Zookeeper ensemble was congured on master01, master02 and master03. 
If you have any other Zookeeper ensemble, you should modify kafka role's parameters.


Executing playbook
~~~~~~~~~~~~~~~~~~~~~~
To configure Kafka broker cluster, please execute the following playbook.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/confluent/kafka_broker.yml -k -s

After finishing configuration, you need to start Kafka cluster.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/start_kafka_server.yml -k -s

As the same as the above procedure,
you can install Schema Registry and Kafka REST Proxy
by using kafka_schema.yml and kafka_rest.yml in playbooks/conf/confluent directory.
And, use the following playbooks to these services, 

.. code-block:: shell

 $ ansible-playbook playbooks/operation/start_schema_registry.yml -k -s
 $ ansible-playbook playbooks/operation/start_kafka_rest.yml -k -s


Configure Ambari
-------------------------

To install the basic packages, execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ambari/ambari_server.yml -k -s

Install Ambari agent to all machines.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ambari/ambari_agent.yml -k -s

Execute initilization of Ambari server.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/ambari/setup.yml -k -s

Then you can access Ambari web UI on "manage" node.

.. note:: Todo: blueprint

Configure Jenkins
--------------------------
To install Jenkins and related packages, execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/jenkins/jenkins.yml -k -s

Configure Anaconda CE
--------------------------
To install Anaconda2 CE, execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/anacondace/anacondace2.yml -k -s

To install Anaconda3 CE, execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/anacondace/anacondace3.yml -k -s

The above command installs Anaconda CE pakcages to /usr/local/anacondace directory.
If you want to configure PATH, please do it yourself.

Configure Hive
--------------------------
To install Hive and related packages, execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5_hive/cdh5_hive.yml -k -s -e "server=hadoop_client"

The above command installs PostgreSQL and Hive packages as well as common packages.
To Initialize PostgreSQL Database, execute the following command.
This command remove existing database and initialize database.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/postgresql/initdb.yml -s -k -e "server=hadoop_client"
 $ ansible-playbook playbooks/operation/postgresql/restart_postgresql.yml -s -k -e "server=hadoop_client"

To create user and database, execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5_hive/create_metastore_db -k -s -e "server=hadoop_client"

To define schema, execute the following command *on the Hadoop client*.

.. code-block:: shell

 $ cd /usr/lib/hive/scripts/metastore/upgrade/postgres
 $ sudo -u postgres psql
 postgres=# \c metastore
 metastore=# \i hive-schema-1.1.0.postgres.sql
 metastore=# \pset tuples_only on
 metastore=# \o /tmp/grant-privs
 metastore=#   SELECT 'GRANT SELECT,INSERT,UPDATE,DELETE ON "'  || schemaname || '". "' ||tablename ||'" TO hiveuser ;'
 metastore-#   FROM pg_tables
 metastore-#   WHERE tableowner = CURRENT_USER and schemaname = 'public';
 metastore=# \o
 metastore=# \pset tuples_only off
 metastore=# \i /tmp/grant-privs
 metastore=# \q

To start metastore service, execute the following command.


.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5_hive/cdh5_hive.yml -s -k -e "server=hadoop_client" 
 $ ansible-playbook playbooks/operation/postgresql/restart_postgresql.yml -s -k -e "server=hadoop_client"
 $ ansible-playbook playbooks/operation/cdh5_hive/start_metastore.yml -k -s -e "server=hadoop_client"

If you also use Hive as a input of Spark,
please copy hive-site.xml from /etc/hive/conf to /etc/spark/conf.


Configure Alluxio on YARN
----------------------------
To configure Alluxio environment at the client,
please execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/alluxio/alluxio_yarn.yml -k -s

This configures /usr/local/alluxio, compiles sources, adds some directories to PATH, and so on.

.. note::

   The role, alluxio_yarn, creates a tar file which is used when you deploy
   an application to YARN and replace alluxio-yarn.sh in Alluxio package.
   This is because the original alluxio-yarn.sh create tar files every time
   you deploy applications and it is not convenient.

If you want to deploy an Alluxio application to YARN,
please execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/alluxio_yarn/deploy_alluxio.yml -k -s

You can configure the following variables.

* alluxio_yarn_hadoop_home: "/usr/lib/hadoop"
* alluxio_yarn_yarn_home: "/usr/lib/hadoop-yarn"
* alluxio_yarn_hadoop_conf_dir: "/etc/hadoop/conf"
* alluxio_yarn_num_workers: "3"
* alluxio_yarn_working_dir: "hdfs://mycluster/tmp"
* alluxio_yarn_master: '{{ groups["hadoop_slave"][0] }}'

Configure TPC-DS
---------------------
To configure TPC-DS, please execute the following command.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/tpc_ds/tpc_ds.yml -k -s

The default target node is localhost.
If you want to configure any other nodes,
please execute the command with overwriting "server" variable like the following.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/tpc_ds/tpc_ds.yml -k -s -e "server=haddoop_client:hadoop_slave"

Configure CUDA and cuDNN
----------------------------
First, you should download cuDNN package from NVIDIA's download site manually.
This is because we need to register NVIDIA's site before downloading the package.
In this role, we use cudnn-8.0-linux-x64-v5.1.solitairetheme8.
Before executing the playbook, you should store cudnn-8.0-linux-x64-v5.1.solitairetheme8 in roles/cuda/files directory.

If you want to install required packages,
please execute the command with overwriting "server" variable like the following.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/tensorflow/gpu_env.yml -k -s -e "server=hd-client01"


.. set ft=rst tw=0 et ts=2 sw=2
