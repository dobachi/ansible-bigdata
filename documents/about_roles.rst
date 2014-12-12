About roles
============
We manage OS and middlewares by using several seperated roles.

Roles to configure basic environments
----------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
common           Basic configuration about OS, basic services, and so on
jdk              Configuraiotn of Oracle JDK
scala            Configuraiton of Scala on Hadoop client node
screen           Configuration of screen command
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

Roles to configure Spark core on client node
------------------------------------------------

================ =======================================================
Role name        Use for
================ =======================================================
cdh5_spark       Configuration of Spark core on Hadoop client node
================ =======================================================
