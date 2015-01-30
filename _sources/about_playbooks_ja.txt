プレイブック集について
=============================
このプロジェクトには2種類のプレイブックが含まれています。

* 構成管理のためのプレイブック集

  + パッケージをインストールしたり、パラメータを設定するために利用されます。
  + （例）Hadoopコアパッケージのインストール

* 運用作業のためのプレイブック

  + OSサービスやミドルウェアの運用作業を行うためのプレイブック集です。
  + （例）HDFSのフォーマット

構成管理のためのプレイブック集
-------------------------------
playbooks/confディレクトリ以下に以下のプレイブック集があります。

====================== ==========================================
ディレクトリ           用途
====================== ==========================================
playbooks/conf/common  コモンな設定。OSの設定。
playbooks/conf/cdh5    CDH5のHDFS/YARN環境の構築／設定
playbooks/conf/ansible Ansibleの実行環境の設定
====================== ==========================================

common
~~~~~~
基本的な設定を行います。

* playbooks/conf/common/common_all.yml

  + すべての基本的な設定を行うプレイブックです

* playbooks/conf/common/common_only_common.yml

  + 「common」ロールのみ実行するプレイブックです

cdh5
~~~~

CDH5のHDFS/YARN環境の構築／設定を行います。

* cdh5_all.yml

  + 他のプレイブックのラッパーです
  + すべてのサーバの構成管理を実行できます

* cdh5_cl.yml

  + Hadoopクライアント環境を構成管理します
  + 「cdh5_cl」ロールを実行します

* cdh5_journalnode.yml

  + HDFS JournalNodeを構成管理します
  + 「cdh5_jn」ロールを実行します

* cdh5_namenode.yml

  + HDFS NameNodeを構成管理します
  + 「cdh5_nn」ロールを実行します

* cdh5_other.yml

  + MapReduce HistoryServerとYARN Proxy環境を構成管理します
  + 「cdh5_ot」ロールを実行します

* cdh5_resourcemanager.yml

  + YARN ResouceManagerを構成管理します
  + 「cdh5_rm」ロールを実行します

* cdh5_slave.yml

  + HDFS DataNodeとYARN NodeManagerを構成管理します
  + 「cdh5_sl」ロールを実行します

* cdh5_spark.yml

  + Hadoopクライアント環境のSparkコアを構成管理します
  + 「cdh5_spark」ロールを実行します

* cdh5_zookeeper.yml

  + Zookeeperを構成管理します
  + 「zookeeper_server」ロールを実行します

ansible
~~~~~~~
Ansible実行環境を構成管理します。

.. note:: 

   Hadoop HDFS/YARN環境を構成管理するにあたっては、
   このプレイブック集は必須ではありません。
   もし手動でAnsible実行環境を構成管理した場合は不要です。

* ansible_client.yml

  + Ansibleを実行する構成管理サーバ（このプロジェクトではHadoopクライアント環境としています）上に
    Ansible実行環境を構築します
  + 「ansible」ロールを実行します

* ansible_remote.yml

  + Ansibleを高速化するためのパッケージをリモートサーバ群にインストールします
  + 「ansible_remote」ロールを実行します

運用作業のためのプレイブック集
-------------------------------

playbooks/operationディレクトリ以下に運用作業のためのプレイブック集があります。

========================= ====================================================================
ディレクトリ              用途
========================= ====================================================================
playbooks/operation/cdh5  Hadoop HDFS/YARNサービスの運用に用います
                          例えばHDFSの初期化や各サービスの起動／停止です。
playbooks/operation/ec2   Hadoop用のAWS EC2インスタンスを起動します
========================= ====================================================================

cdh5
~~~~

Hadoopの各サービスを運用するためのプレイブック集です。
詳しくはディレクトリ内のREADMEを参照ください。

ec2
~~~~
AWS EC2インスタンスを起動するためのプレイブック集です。
詳しくはディレクトリ内のREADMEを参照ください。
