How to use this playbooks
==========================
You can use these playbooks to configure the following configuration.
These playbooks are independent of each other.

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

 $ sudo yum install -y http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-2.noarch.rpm

Install Ansible

.. code-block:: shell

 $ sudo yum install ansible

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

 $ ansible -m ping cdh5_all -k -s

How to configure host names of nodes
------------------------------------------
If you want to configure hostname of nodes,
You can use "common" role and related playbooks.

Execute ansible-playbook command with common_only_common.yml

.. code-block:: shell

 $ cd /etc/ansible
 $ ansible-playbook playbooks/conf/common/common_only_common.yml -k -s -e "common_config_hostname=True server=cdh5_all"

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

Start services::

 $ ansible-playbook playbooks/operation/cdh5/start_cluster.yml -k -s 

How to install Spark Core
---------------------------
You can install Spark Core into Client node by the following command

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5/cdh5_spark.yml -k -s
