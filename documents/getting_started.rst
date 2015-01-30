Getting Started
=================
In this section, the procedure to constuct HDFS/YARN environment is explained.

Prerequirement for simplicity
--------------------------------
* You have enough number of servers. The host names of servers are shown in :ref:`sec-servers`

  + eg. master01, master02, master03, client01, slave01, slave02, slave03, slave04, slave05

* You can login to each server by SSH from the server where you execute ansible by the host names.

  + eg. Your server has configured /etc/hosts to access Hadoop cluster's servers.

* You can use "sudo" in each server

Install Ansible
------------------
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

Modify configuration of Ansible
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Copy ansible.cfg.sample to ansible.cfg

.. code-block:: shell

 $ cd ansible
 $ cp ansible.cfg.sample ansible.cfg

Create symbolic link of inventory file.

.. code-block:: shell

 $ ln -s hosts.sample hosts

Modify hosts file to be copied to /etc/hosts

.. code-block:: shell

 $ sudo vi roles/common/files/hosts.default

Install and configure Hadoop
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Install and configure Hadoop.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5/cdh5_all.yml -k -s -e "common_hosts_replace=True"

Initilize Hadoop environment.

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5/init_zkfc.yml -k -s 
 $ ansible-playbook playbooks/operation/cdh5/init_hdfs.yml -k -s 

Start services

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5/start_cluster.yml -k -s 

Congratulation!
~~~~~~~~~~~~~~~~~~
Now, you can access HDFS from client01.

Example of the command.

.. code-block:: shell

 $ hdfs dfs -ls /

You can access the web service of HDFS with the following URL.
(One of them is Active and another is Standby)

* http://master01:50070
* http://master02:50070

And, you can execute sample jobs from client01.

Example of the command.

.. code-block:: shell

 $ hdfs dfs -mkdir /user/<user name>
 $ hdfs dfs -chown <user name>:<group name> /user/<user name>
 $ yarn jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar pi 100 100

You can access the web service of YARN with the following URL.
(One of them is Active and another is Standby)

* http://master02:8088
* http://master03:8088

Install Spark core
~~~~~~~~~~~~~~~~~~~~
Example of commands.

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5/cdh5_spark.yml -k -s

Now, you can use Spark's commands, such as spark-shell.

.. code-block:: shell

 $ spark-shell

As a result, you can run spark-shell with YARN environment.

You can access the web service of Spark driver with the following URL.

* http://client01:4040

