How to use this playbooks
==========================
You can use these playbooks to configure the following configuration.
**These playbooks are independent of each other** .

* Ansible client environment to use various Ansible functions
* Host name configuration
* CDH5 HDFS/YARN environment

Assumption of this section
----------------------------
* You have servers described in :ref:`sec-servers` section.

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
Move original /etc/ansible

.. code-block:: shell

 $ cd /etc
 $ sudo mv ansible ansible.org

Clone this repository

.. code-block:: shell

 $ git clone https://github.com/dobachi/ansible-hadoop.git ansible

Modify configuration
~~~~~~~~~~~~~~~~~~~~
Modify hosts file to be copied to /etc/hosts

.. code-block:: shell

 $ cd ansible
 $ sudo vi roles/common/files/hosts.default

Execute playbooks one by one
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Execute ansible-playbook command with common_all.yml

.. code-block:: shell

 $ ansible-playbook playbooks/conf/common/common_all.yml -k -s -i hosts.sample -e "common_hosts_replace=True"

Copy ansible's hosts and modify it

.. code-block:: shell

 $ sudo vi roles/ansible/templates/hosts.default.j2

Execute ansible-playbook command with ansible_client.yml

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ansible/ansible_client.yml -k -s -i hosts.sample -e "ansible_environment=default ansible_modify_cfg=True"

If you use EC2 and need a private key for SSH,
you should specify "ansible_private_key_file" paramter.
You should execute command with the parameter instead of the above command

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ansible/ansible_client.yml -k -s -i hosts.sample -e "ansible_environment=default ansible_modify_cfg=True ansible_private_key_file=${HOME}/mykey.pem"

Check whether all nodes are reachable and "sudo" is available

.. code-block:: shell

 $ ansible -m ping hadoop_all -k -s

How to boot EC2 instances for Hadoop cluster
------------------------------------------------
If you want to use Hadoop on EC2 instances,
you can use playbooks/operation/ec2/hadoop_nodes_up.yml to boot instances.

Create inventory file
~~~~~~~~~~~~~~~~~~~~~~
If you don't have an inventory file,
create inventory file, /etc/ansible/hosts while referring /etc/ansible/hosts.sample.

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

.. code-block:: shell

 $ ansible-playbook playbooks/conf/influxdb/all.yml -k -s

Then, create a database in InfulxDB to hold data gathered by Graphite's protocol.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/influxdb/create_graphite_db.yml -k -s

Create a database in InfulxDB to store Grafana's dashboard data.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/influxdb/create_grafana_db.yml -k -s

The database's name and user name to connect the database is 
configured in group_vars/all/meta, group_vars/all/influxdb and group_vars/all/grafana like the following.

**group_vars/all/meta**

.. code-block:: yaml

 meta_graphitedb_in_influxdb: 'graphite'
 meta_grafanadb_in_influxdb: 'grafana'

**group_vars/all/influxdb**

.. code-block:: yaml

 influxdb_server: "{{ groups['hadoop_other'][0] }}"
 influxdb_admin_user: "root"
 influxdb_graphite_db_name: "{{ meta_graphitedb_in_influxdb }}"
 influxdb_grafana_db_name: "{{ meta_grafanadb_in_influxdb }}"

**group_vars/all/grafana**

.. code-block:: yaml

 grafana_influxdb_list:
   - name: "{{ meta_graphitedb_in_influxdb }}"
     server: "{{ groups['hadoop_other'][0] }}"
     db_name: "{{ meta_graphitedb_in_influxdb }}"
     admin_name: "root"
     admin_pass: "root"
     grafanaDB: "false"
   - name: "{{ meta_grafanadb_in_influxdb }}"
     server: "{{ groups['hadoop_other'][0] }}"
     db_name: "{{ meta_grafanadb_in_influxdb }}"
     admin_name: "root"
     admin_pass: "root"
     grafanaDB: "true"


Please read `Grafana's documents <http://grafana.org/docs/features/intro/>`_ to learn
how to configure graphs.

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
To configure Kafka cluster, please execute the following playbook.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/confluent/kafka.yml -k -s

After finishing configuration, you need to start Kafka cluster.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/start_kafka_server.yml -k -s

You can use Confluent's schema registry serivce and Kafka REST Proxy service.

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

.. set ft=rst tw=0 et ts=2 sw=2
