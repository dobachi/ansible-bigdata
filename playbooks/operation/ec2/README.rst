******************
README
******************
Playbooks to boot EC2 instances.

.. sectnum::

How to boot Hadoop inctances?
==============================

Create inventory file
-------------------------
If you don't have an inventory file,
create inventory file, /etc/ansible/hosts while referring /etc/ansible/hosts.sample.

Define environment variables for AWS access
-----------------------------------------------
We use environment variables to configure AWS access keys.
Define AWS_ACCESS_KEY and AWS_SECRET_KEY in your ~/.bashrc

::

 export AWS_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXx
 export AWS_SECRET_KEY=XXXXXXXXXXXXXXXXXXXXXXXXX

If you don't have AWS keys,
create keys while referring AWS web site.

Define parameters for ec2_hadoop role
----------------------------------------
You can find the parameter description for ec2_hadoop role in roles/ec2_hadoop/defaults/main.yml

To define your own parameters,
you need to create the group variable file (e.g. group_vars/top) and write parameter defines in this file.

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
---------------
Execute ansible-playbook command.

::

 $ ansible-playbook playbooks/operation/ec2/hadoop_nodes_up.yml -k

