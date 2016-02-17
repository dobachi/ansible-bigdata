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
* CDH5のSpark Coreをインストールし、Sparkの実行環境を整える
* コミュニティ版Spark Coreをインストールし、Sparkの実行環境を整える
* Gangliaによるリソース可視化環境を整える
* InfluxDBとGrafanaによるメトリクス可視化環境を整える
* テスト用のPseudo環境を構築する
* Sparkノートブック環境としてZeppelinの実行環境を整える
* fluentdやtd-agentを構成管理する
* Kafkaクラスタを構成管理する
* Ambariを構成管理する

.. _sec-servers-ja:

サーバ構成
-----------
このプレイブック集では以下のサーバ構成を前提としています。

**中規模クラスタ構成時のサーバ環境**

======== ================================================================================
サーバ名 サービス構成
======== ================================================================================
master01 Primary NameNode, JournalNode, Zookeeper Server(id=1), Ganglia Slave
master02 Backup NameNode, JournalNode, Zookeeper Server(id=2), Primary ResourceManager,
         Ganglia Slave
master03 JournalNode, Zookeeper Server(id=3), HistoryServer, Backup ResourceManager,
         Ganglia Slave, Ganglia Master, InfluxDB, Grafana
client01 Hadoop Client, Spark Core, Ganglia Slave, Zeppelin
slave01  DataNode, NodeManager, Ganglia Slave
slave02  DataNode, NodeManager, Ganglia Slave
slave03  DataNode, NodeManager, Ganglia Slave
slave04  DataNode, NodeManager, Ganglia Slave
slave05  DataNode, NodeManager, Ganglia Slave
kafka01  Kafka broker
kafka02  Kafka broker
kafka03  Kafka broker
manage   Ambari server
======== ================================================================================

**大規模クラスタ構築時のサーバ構成**

======== ================================================================================
Server   Use for
======== ================================================================================
master01 Primary NameNode, Ganglia Slave
master02 Backup NameNode, Ganglia Slave
master03 Primary ResourceManager, Ganglia Slave
master04 Backup ResourceManager, Ganglia Slave
master05 JournalNode, Zookeeper Server(id=1), Ganglia Slave
master06 JournalNode, Zookeeper Server(id=2), Ganglia Slave
master07 JournalNode, Zookeeper Server(id=3), Ganglia Slave
master08 HistoryServer, Ganglia Master, Ganglia Slave, InfluxDB, Grafana
client01 Hadoop Client, Spark Core, Ganglia Slave, Zeppelin
slave01  DataNode, NodeManager, Ganglia Slave
slave02  DataNode, NodeManager, Ganglia Slave
slave03  DataNode, NodeManager, Ganglia Slave
slave04  DataNode, NodeManager, Ganglia Slave
slave05  DataNode, NodeManager, Ganglia Slave
slave06  DataNode, NodeManager, Ganglia Slave
slave07  DataNode, NodeManager, Ganglia Slave
slave08  DataNode, NodeManager, Ganglia Slave
slave09  DataNode, NodeManager, Ganglia Slave
slave10  DataNode, NodeManager, Ganglia Slave
kafka01  Kafka broker
kafka02  Kafka broker
kafka03  Kafka broker
manage   Ambari server
======== ================================================================================

**疑似分散環境**

======== ================================================================================
サーバ名 サービス構成
======== ================================================================================
pseudo   NameNode, DataNode, SecondaryNameNode, ResourceManager, NodeManager,
         Spark-core, Spark history server
======== ================================================================================

* InfluxDB, Grafana, Zeppelinもプレイブックを利用して構成できます

ソフトウェア構成
-------------------

============= ================================
ソフトウェア  バージョン等
============= ================================
OS            CentOS6.6 or CentOS7.0
Hadoop        CDH5.3
Spark         Spark1.2 of CDH5
              or Spark1.3 community edition
Ansible       Ansible 1.8 of EPEL
InfluxDB      コミュニティ最新版
Graphana      コミュニティ版1.9.1
Zeppelin      コミュニティ最新版
Kafka         コミュニティ版0.9.0.0
============= ================================

必要事項
----------------
* Ansibleを実行するサーバからすべてのサーバにSSHで到達性があることを前提としています
* 本ドキュメントの例では、実行ユーザがsudoできることを前提としています。
  もしsudo権限が無いのであればrootユーザでSSH接続して実行する必要があります。
* EC2上のサーバを構成管理する際には、RSA鍵を利用したSSHアクセスになります。
  例えばCentOSコミュニティのCentOS6インスタンスの場合、標準ではrootユーザで
  RSA鍵を利用してログインします。
