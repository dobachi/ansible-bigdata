概要
=====================

このプレイブック集について
--------------------------

このプレイブック集を利用すると、HA構成のHDFS、YARN環境を構築できます。
小さめのクラスタを立てるのに向いた内容となっているため、
大きなクラスタを立てる時にはHadoopやOSのパラメータチューニングが別途必要となることにご注意ください。

なお、このプレイブック集は、 `dobachi's ansible-playbooks <https://bitbucket.org/dobachi/ansible-playbooks.git>`_ や
`mcsrainbow's ansible-playbooks-cdh5 <https://github.com/mcsrainbow/ansible-playbooks-cdh5>`_ などを参考に作りました。

特徴
--------
このプレイブック集の機能は以下の通りです。
*各機能単体で利用できます。*

* Ansible実行環境を整える
* Hadoop環境用のAWS EC2インスタンスを起動する
* HA構成のHDFS、YARN環境を構築する
* Spark Coreをインストールし、Sparkの実行環境を整える

.. _sec-servers-ja:

サーバ構成
-----------
このプレイブック集では以下のサーバ構成を前提としています。

**構成**

======== ================================================================================
サーバ名 サービス構成
======== ================================================================================
master01 Primary NameNode, JournalNode, Zookeeper Server(id=1)
master02 Backup NameNode, JournalNode, Zookeeper Server(id=2), Primary ResourceManager
master03 JournalNode, Zookeeper Server(id=3), HistoryServer, Backup ResourceManager
client01 Hadoop Client, Spark Core
slave01  DataNode, NodeManager
slave02  DataNode, NodeManager
slave03  DataNode, NodeManager
slave04  DataNode, NodeManager
slave05  DataNode, NodeManager
======== ================================================================================

**ソフトウェア**

============= =============================
ソフトウェア  バージョン等
============= =============================
OS            CentOS6.6 or CentOS7.0
Hadoop        CDH5.2
Spark         Spark1.2 of CDH5
Ansible       Ansible 1.8.2 of EPEL
============= =============================

