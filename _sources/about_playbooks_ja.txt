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

============================= ==========================================
ディレクトリ                  用途
============================= ==========================================
playbooks/conf/common         コモンな設定。OSの設定。
playbooks/conf/cdh5           CDH5のHDFS/YARN環境の構築／設定
playbooks/conf/cdh5_pseudo    CDH5の疑似分散環境の構築/設定
playbooks/conf/ansible        Ansibleの実行環境の設定
playbooks/conf/ganglia        Ganglia環境の構築／設定
playbooks/conf/influxdb       InfluxDBとGrafanaの構築／設定
playbooks/conf/spark_comm     コミュニティ版Sparkの構築/設定
playbooks/conf/zeppelin       コミュニティ版Zeppelinの構築/設定
playbooks/conf/fluentd        fluentdとtd-agentの構築/設定
playbooks/conf/kafka          Kafkaクラスタの構築/設定
playbooks/conf/ambari         Ambariサーバやエージェントの設定
============================= ==========================================

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

cdh5_pseudo
~~~~~~~~~~~~

CDH5のHDFS/YARNのPseudo環境を構築／設定します。

* cdh5_pseudo.yml

  + Pseudo環境を構築、設定します

* cdh5_spark.yml

  + Spark環境をPseudo環境に構築、設定します

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

ganglia
~~~~~~~~~

Gangliaを構成管理します。
Gangliaはマスタ／スレーブ構成なのでそれぞれを管理するためのプレイブックがあります。

* ganglia_all.yml

  + マスタとスレーブを構成するプレイブックのラッパーです

* ganglia_master.yml

  + マスタを構成するためのプレイブックです

* ganglia_slave.yml

  + スレーブを構成するためのプレイブックです

influxdb
~~~~~~~~~
* all.yml

  + InfluxDBとGrafanaを設定します

spark_comm
~~~~~~~~~~~
* all.yml

  + クラスタ全体の設定

* spark_base.yml

  + 全ノード共通の基本設定

* spark_client.yml

  + アプリケーションを開発すためのクライアント環境の整備

* spark_history.yml

  + Sparkのヒストリサーバを起動するための設定

* spark_libs.yml

  + MLlibでネイティブライブラリを利用するための設定

zeppelin
~~~~~~~~~~~
* zeppelin.yml

  + Zeppelinを構成管理します

fluentd
~~~~~~~~~~~~
* fluentd.yml

  + fluentdを構成管理します

* td_agent.yml

  + td-agentを構成管理します

kafka
~~~~~~~~~~~~
* kafka_brocker.yml

  + Kafkaを構成管理します

ambari
~~~~~~~~~~~~~
* ambari_agent.yml

  + AmbariエージェントをAmbariサーバに頼らずに設定

* ambari_server.yml

  + Ambariサーバを設定

運用作業のためのプレイブック集
-------------------------------

playbooks/operationディレクトリ以下に運用作業のためのプレイブック集があります。

================================= ====================================================================
ディレクトリ                      用途
================================= ====================================================================
playbooks/operation/cdh5          Hadoop HDFS/YARNサービスの運用に用います
                                  例えばHDFSの初期化や各サービスの起動／停止です。
playbooks/operation/cdh5_pseudo   Hadoop HDFS/YARNサービスの運用に用います
                                  例えばHDFSの初期化や各サービスの起動／停止です。
playbooks/operation/ec2           Hadoop用のAWS EC2インスタンスを起動します
playbooks/operation/httpd         HTTPサービスを起動／停止します
playbooks/operation/influxdb      InfluxDBを初期化します
playbooks/operation/spark_com     コミュニティ版Sparkのビルドやサービスの起動/停止に用います
playbooks/operation/zeppelin      Zeppelinのサービスを起動/停止する
playbooks/operation/fluentd       td-agentサービスを起動/停止する
playbooks/operation/kafka         Kafkaクラスタの起動/停止およびトピックの整理
playbooks/operation/ambari        Ambariサーバの初期設定。各サービスの起動・停止
================================= ====================================================================

cdh5
~~~~

Hadoopの各サービスを運用するためのプレイブック集です。
詳しくはディレクトリ内のREADMEを参照ください。

ec2
~~~~
AWS EC2インスタンスを起動するためのプレイブック集です。
詳しくはディレクトリ内のREADMEを参照ください。

influxdb
~~~~~~~~
* create_db.yml
  
  + すべての必要なデータベースをInfluxDBに作成します。

* create_graphite_db.yml

  + InfluxDBにGraphiteプロトコルで受領したデータを格納するデータベースを作成します。
    主にSparkのGraphiteプロトコルによるメトリクスを保存するために使用します。

* create_grafana_db.yml

  + Grafanaのダッシュボード情報を保存するデータベースをInfluxDBに作成します。

spark_comm
~~~~~~~~~~~
* make_spark_packages.yml

  + Sparkソースコードのコンパイルとパッケージ作成

* start_spark_historyserver.yml

  + Sparkのヒストリサーバを起動する

* stop_spark_historyserver.yml

  + Sparkのヒストリサーバを停止する

zeppelin
~~~~~~~~~~
* build.yml

  + Zeppelinをコンパイルしパッケージングします
  + このプレイブックはコミュニティ公式ドキュメントに記載されているコンパイル手順を自動化した
    ヘルパー機能です

* restart_zeppelin.yml

  + Zeppelinのサービスを停止して起動します

* start_zeppelin.yml

  + zeppelin-daemon.shを実行することでサービスを起動します

* stop_zeppelin.yml

  + zeppelin-daemon.shを実行することでサービスを停止します

fluentd
~~~~~~~~~~~~~~~~~~~~~
* restart_td_agent.yml

  + td-agentのサービスを停止して起動します

* start_td_agent.yml

  + td-agentのサービスを起動します

* stop_td_agent.yml

  + td-agentのサービスを停止します

kafka
~~~~~~~~~~~~~~~~~~~~~
* restart_kafka.yml

  + Kafkaクラスタを起動/停止します

* start_kafka.yml

  + Kafkaクラスタを起動します

* stop_kafka.yml

  + Kafkaクラスタを停止します

* create_topic.yml

  + トピックを作成します

* delete_topic.yml

  + トピックを削除します

ambari
~~~~~~~~~~~~
* Ambariサーバの初期設定

  + setup.yml

* 各サービスを起動・停止

  + restart_all.yml
  + restart_ambari_metrics.yml
  + restart_hdfs.yml
  + restart_yarn.yml
  + restart_zookeeper.yml
  + start_all.yml
  + start_ambari_metrics.yml
  + start_hdfs.yml
  + start_yarn.yml
  + start_zookeeper.yml
  + stop_all.yml
  + stop_ambari_metrics.yml
  + stop_hdfs.yml
  + stop_yarn.yml
  + stop_zookeeper.yml
