プレイブックの利用方法
==========================
この節では、プレイブックを使って各サービスを構成管理する例を示します。
なお、各プレイブックはそれぞれ個別に実行できます。

前提条件
----------------------------
* You have servers described insection.
* サーバの構成は :ref:`sec-servers-ja` 節の内容に従っているものとします。

.. _sec-configure-ansible-env-ja:

Ansible実行環境を構成管理する例
-------------------------------
この節では、Ansibleをインストールするところから初めて、
プレイブックを使ってAnsible実行環境を構成管理する例を示します。

パッケージのインストール
~~~~~~~~~~~~~~~~~~~~~~~~~~
EPELレポジトリをインストールします。

.. code-block:: shell

 $ sudo yum install -y epel-release

Ansibleをインストールします。

.. code-block:: shell

 $ sudo yum install -y ansible

本プレイブック集のクローン
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
まずオリジナルの/etc/ansibleディレクトリをmvします。

.. code-block:: shell

 $ cd /etc
 $ sudo mv ansible ansible.org

gitレポジトリからプレイブック集をクローンします。

.. code-block:: shell

 $ git clone https://github.com/dobachi/ansible-hadoop.git ansible

Ansibleの設定修正
~~~~~~~~~~~~~~~~~~~~
/etc/hosts にコピーされるhostsのテンプレートを修正します。

.. code-block:: shell

 $ cd ansible
 $ sudo vi roles/common/files/hosts.default

プレイブック実行
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
最初はインベントリのサンプルファイルhosts.sampleを使ってローカルホストに基本的な設定を行います。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/common/common_all.yml -k -s -i hosts.sample -e "common_hosts_replace=True"

なおオプションで渡しているcommon_hosts_replaceという変数は、
/etc/hostsファイルを置き換えるかどうかを設定するものです。
今回は置き換えるためにTrueとしています。

つぎに/etc/ansible/hosts.defaultにコピーされるインベントリファイルのテンプレートを修正します。

.. code-block:: shell

 $ sudo vi roles/ansible/templates/hosts.default.j2

ansible_client.ymlを実行し、Ansible実行環境を整備します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ansible/ansible_client.yml -k -s -i hosts.sample -e "ansible_environment=default ansible_modify_cfg=True"

なおオプションで渡しているansible_environmentという変数は、
インベントリファイルのサフィックスを表します。
またansible_modify_cfgという変数は、
ansible.cfgを置き換えるかどうかを設定するものです。
今回は置き換えるためにTrueとしています。

もしHadoopクラスタがEC2インスタンスで構成されている場合、
SSHログインの際に鍵認証が必要になります。
ansible.cfg内で鍵のPATHを指定するために、「ansible_private_key_file」という変数を設定します。
この場合の実行コマンド例を以下に示します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ansible/ansible_client.yml -k -s -i hosts.sample -e "ansible_environment=default ansible_modify_cfg=True ansible_private_key_file=${HOME}/mykey.pem"

最後に、全サーバにSSHログイン可能かどうか確認します。

.. code-block:: shell

 $ ansible -m ping hadoop_all -k -s

EC2インスタンスの構成
----------------------
この節では、Hadoop用のAWS EC2インスタンスを起動するための手順例を示します。

前提
~~~~
* 本プレイブック集を利用して、もしくは手動で、Ansible実行環境が設定されていること

環境変数の設定
~~~~~~~~~~~~~~~
AWS access keyなどを設定します。
アクセス鍵についてはAWSのページを参照し、あらかじめ作成してください。

以下のような環境変数を定義します。

::

 export AWS_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXx
 export AWS_SECRET_KEY=XXXXXXXXXXXXXXXXXXXXXXXXX

「ec2_hadoop」ロールのパラメータ設定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
パラメータはroles/ec2_hadoop/defaults/main.ymlに定義されています。
ただし設定必要なパラメータは、最初コメントアウトされた状態になっています。
各自の値を設定し、コメントアウトを解除してください。

なお、group_vas/all/ec2等のグループ変数に記載しても良いです。
設定例を以下に示します。

::

 ec2_hadoop_group_id: sg-xxxxxxxx
 
 ec2_hadoop_accesskey: xxxxx
 
 ec2_hadoop_itype: xx.xxxxx
 
 ec2_hadoop_master_image: ami-xxxxxxxx
 ec2_hadoop_slave_image: ami-xxxxxxxx
 ec2_hadoop_client_image: ami-xxxxxxxx
 
 ec2_hadoop_region: xx-xxxxxxxxx-x
 
 ec2_hadoop_vpc_subnet_id: subnet-xxxxxxxx

もし設定漏れがある場合は以下のようなメッセージが出力されますので、
すべてのパラメータを設定するようにしてください。::

 One or more undefined variables: 'ec2_hadoop_group_id' is undefined

プレイブックの実行
~~~~~~~~~~~~~~~~~~
ansible-playbookコマンドを実行します。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/ec2/hadoop_nodes_up.yml -c local

成功した場合は */tmp/ec2_<プレイブックを実行したときのUNIXエポック時刻>* のディレクトリ以下に以下の内容のファイルが配備されています。
EC2インスタンスにアクセスしたり、EC2インスタンスによるクラスタを構成管理する際に利用してください。

* プレイベートIPアドレス、パブリックIPアドレスの一覧
* クラスタ内で利用できるAnsibleインベントリファイルのサンプル
* クラスタ内で利用できる/etc/hostsファイルのサンプル

（補足）EC2インスタンスを再起動した場合
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
EC2インスタンスを再起動するとpublicなIPアドレスが変わります。
その場合はもう一度プレイブックを実行すると、改めてIPアドレス一覧のファイルが生成されます。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/ec2/hadoop_nodes_up.yml -c local

ホスト名の設定
------------------------------------------
「common」ロールを使って各サーバのホスト名を設定できます。
主にEC2インスタンスでHadoopクラスタを構成した際に、
自動付与されたホスト名から扱いやすいホスト名に変更する場合に利用します。

前提
~~~~
* 本プレイブック集を利用して、もしくは手動で、Ansible実行環境が設定されていること

手順
~~~~
以下の通り、ansible-playbookコマンドを実行します。

.. code-block:: shell

 $ cd /etc/ansible
 $ ansible-playbook playbooks/conf/common/common_only_common.yml -k -s -e "common_config_hostname=True server=hadoop_all"

CDH5のHDFS/YARN環境を構成する例
--------------------------------------------

前提条件
~~~~~~~~~~~~
* 本プレイブック集を利用して、もしくは手動で、Ansible実行環境が設定されていること

手順
~~~~~~~~~
以下の通り、ansible-playbookコマンドを実行します。

なお、ここではオプションでcommon_hosts_replace変数をTrueにしているため、
各サーバの/etc/hostsを置き換えることに注意してください。
/etc/ansible/roles/common/files/hosts.defaultの内容に置き換えられます。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5/cdh5_all.yml -k -s -e "common_hosts_replace=True"
 $ ansible-playbook playbooks/operation/cdh5/init_zkfc.yml -k -s 
 $ ansible-playbook playbooks/operation/cdh5/init_hdfs.yml -k -s 

適切に設定されたらサービスを起動してください::

 $ ansible-playbook playbooks/operation/cdh5/start_cluster.yml -k -s 

Spark実行環境の整備
~~~~~~~~~~~~~~~~~~~~~
以下の通り、ansible-playbookコマンドを実行します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5/cdh5_spark.yml -k -s

Sparkのヒストリサーバを起動する場合は以下のコマンドを実行します。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5/start_sparkhistory.yml -k -s

CDH5のPseudo環境を構成する例
--------------------------------------------

前提条件
~~~~~~~~~~~~
* 本プレイブック集を利用して、もしくは手動で、Ansible実行環境が設定されていること

手順
~~~~
以下の通り、ansible-playbookコマンドを実行します。
各サーバの/etc/hostsを置き換えることに注意してください。
/etc/ansible/roles/common/files/hosts.defaultの内容に置き換えられます。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5_pseudo/cdh5_pseudo.yml -k -s -e "common_hosts_replace=True"
 $ ansible-playbook playbooks/operation/cdh5_pseudo/init_hdfs.yml -k -s 

適切に設定されたらサービスを起動してください::

 $ ansible-playbook playbooks/operation/cdh5_pseudo/start_cluster.yml -k -s 

CDH5のPseudo環境にSpark実行環境を整備
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
以下の通り、ansible-playbookコマンドを実行します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5_pseudo/cdh5_spark.yml -k -s

Sparkのヒストリサーバを起動する場合は以下のコマンドを実行します。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5_pseudo/start_sparkhistory.yml -k -s

Ganglia環境を構成する
--------------------------------------------

前提条件
~~~~~~~~~~~~
* 本プレイブック集を利用して、もしくは手動で、Ansible実行環境が設定されていること

手順
~~~~
以下の通り、ansible-playbookコマンドを実行します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ganglia/ganglia_all.yml -k -s

gmondでユニキャストを使う方法
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
デフォルトでは、マルチキャストを使うようになっています。
EC2インスタンスでクラスタを構成している場合など、
マルチキャストを利用できない時はユニキャストを利用することになります。

そのためには、"ganglia_slave_use_unicast"というパラメータをTrueに設定してください。

例（group_vars/all/ganglia）::

 ganglia_slave_use_unicast: True

また合わせて"ganglia_slave_host"というパラメータも設定するようにしてください。
このパラメータは、gmondがメトリクスを送る対象を示します。
多くの場合、Gangliaのマスタデーモンgmetadは、
各グループの代表サーバをポーリングします。
このポーリングする対象を"ganglia_slave_host"の設定値にするとよいです。

InfluxDBとGrafanaを構成する
----------------------------
InfluxDBとGrafanaをインストールするには以下のコマンドを実行します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/influxdb/all.yml -k -s

Graphiteプロトコルで受領したデータを格納するデータベースを
InfluxDB内の作成します。
これは主にSparkのメトリクスを受領するために用います。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/influxdb/create_graphite_db.yml -k -s

Grafanaのダッシュボード情報を格納するデータベースを作成します。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/influxdb/create_grafana_db.yml -k -s

なお、作成するデータベースの名前や接続に使用するユーザ名は、
group_vars/all/influxdbにて以下のように設定しています。

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

またGrafanaのグラフを設定する方法は、  `Grafana's documents <http://grafana.org/docs/features/intro/>`_
などをご参照ください。

コミュニティ版Sparkを構成する
----------------------------------------

パッケージの取得、もしくはビルド
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
`Spark公式ダウンロードサイト  <https://spark.apache.org/downloads.html>`_ から
ビルド済みパッケージを取得できます。

もし何らかの理由で自分でビルドしたい場合は、
`Spark公式のビルド手順 <https://spark.apache.org/docs/latest/building-spark.html>`_ を参照ください。

またplaybooks/operation/spark_comm/make_spark_packages.ymlというプレイブックを利用することも可能です。
その場合、以下に示すパラメータを任意で設定してください。

* spark_comm_src_dir
* spark_comm_version
* spark_comm_mvn_options
* spark_comm_hadoop_version

パラメータの設定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
コミュニティ版Sparkの構成には、playbooks/conf/spark_comm/all.ymlというプレイブックを利用できます。

このプレイブックはHTTPでビルド済みパッケージを取得することを前提としています。
そのためのダウンロードURLを設定するため、以下のパラメータを任意に定義してください。

* spark_comm_package_url_base
* spark_comm_package_name

なお、ダウンロードURLは {{ spark_comm_package_url_base }}/{{ spark_comm_package_name }}.tgz のようになります。
例えばダウンロードURLが"http://example.local/spark/spark-1.4.0-SNAPSHOT-bin-2.5.0-cdh5.3.2.tgz"のとき、
spark_comm_package_url_baseは"http://example.local/spark"となり、spark_comm_package_nameは"spark-1.4.0-SNAPSHOT-bin-2.5.0-cdh5.3.2"となります。

.. note::

   spark_comm_package_nameが".tgz"を含まないことに注意してください

プレイブックの実行
~~~~~~~~~~~~~~~~~~~~
パラメータの設定後は、プレイブックを実行します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/spark_comm/all.yml -k -s

ヒストリサーバの起動
~~~~~~~~~~~~~~~~~~~~~~
ヒストリサーバを起動します。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/spark_comm/start_spark_historyserver.yml -k -s

コミュニティ版Zeppelinを構成する
-----------------------------------

ソースコードの取得とビルド
~~~~~~~~~~~~~~~~~~~~~~~~~~
`公式のREADME <https://github.com/apache/incubator-zeppelin/blob/master/README.md>`_ の通り、
コンパイルしてパッケージングします。

注意点はコンんパイル時のオプション（プロパティ設定）です。
現在利用しているHadoopやSparkのバージョンを指定するのを忘れないで下さい。

CDH5.3.3、Spark1.3、YARN環境を利用する場合の例を以下に示します。

.. code-block:: shell

 $ mvn clean package -Pspark-1.3 -Dhadoop.version=2.5.0-cdh5.3.3 -Phadoop-2.4 -Pyarn -DskipTests 

また補助プレイブックplaybooks/operation/zeppelin/build.ymlを利用することもできます。
その場合は以下のパラメータを任意に設定してください。

* zeppelin_git_url
* zeppelin_src_dir
* zeppelin_version
* zeppelin_comiple_flag
* zeppelin_hadoop_version

なお、本プレイブック集に含まれているZeppelinの構成管理用プレイブックでは、
作成したパッケージをHTTPで取得して利用します。

本手順でコンパイル、パッケージ化したものをHTTPで取得可能な場所に配備してください。

プレイブックの実行
~~~~~~~~~~~~~~~~~~
以下のようにプレイブックを実行し、Zeppelinを構成します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/zeppelin/zeppelin.yml -k -s

構成完了後、Zeppelinのサービスを起動します。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/zeppelin/start_zeppelin.yml -k -s

Kafkaクラスタを構成する
-------------------------------

注意事項
~~~~~~~~~~~~~~~
Zookeeperアンサンブルがmaster01, master02、master03にて動作していることを前提とします。
もしほかに独自のアンサンブルがある場合には、ロールパラメータを上書きしてください。


プレイブックの実行
~~~~~~~~~~~~~~~~~~~~~~
以下のようにプレイブックを実行し、Kafkaクラスタを構成します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/kafka/kafka_broker.yml -k -s

構成完了後、Kafkaクラスタを起動してください。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/kafka/start_kafka.yml -k -s

Ambariを構成する
-------------------------

Ambariサーバの基本的なパッケージをインストールします。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ambari/ambari_server.yml -k -s

Ambariエージェントを全マシンにインストールします。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/ambari/ambari_agent.yml -k -s

Ambariサーバを初期化します。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/ambari/setup.yml -k -s

以上の作業でAmbariサーバのウェブUIにアクセスできるようになっているので、
あとはGUI経由でクラスタを構築します。

.. note:: Todo: ブループリント

.. vim: ft=rst tw=0 et ts=2 sw=2
