Getting Started
=================
この節では、Hadoop HDFS/YARN環境を構築する流れを説明します。
各プレイブックの詳細は別節の説明をご覧ください。

簡単化のための前提
--------------------------------
* :ref:`sec-servers-ja` に示された数のサーバを利用できること。またホスト名が正しく設定されていること。

  + ホスト名は、master01, master02, master03, client01, slave01, slave02, slave03, slave04, slave05です。

* Ansibleを実行するサーバからクラスタの各サーバへホスト名でSSHログインできること

  + 例えば、/etc/hosts内にクラスタの各サーバについてのホスト名、IPアドレスが設定されていること。

* 各サーバでsudoできること

Install Ansible
------------------
EPELレポジトリをインストールします。

.. code-block:: shell

 $ sudo yum install -y epel-release

Ansibleをインストールします。

.. code-block:: shell

 $ sudo yum install -y ansible

プレイブック集をクローンします
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
まず既存のファイルをmvします。

.. code-block:: shell

 $ cd /etc
 $ sudo mv ansible ansible.org

クローンします。

.. code-block:: shell

 $ git clone https://github.com/dobachi/ansible-hadoop.git ansible

Ansibleを基本設定します。
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ansible.cfg.sampleをansible.cfgにコピーします。

.. code-block:: shell

 $ cd ansible
 $ cp ansible.cfg.sample ansible.cfg

インベントリファイルのシンボリックリンクを作成します。

.. code-block:: shell

 $ ln -s hosts.sample hosts

各サーバの/etc/hostsにコピーされるテンプレートファイルを修正します。

.. code-block:: shell

 $ sudo vi roles/common/files/hosts.default

Hadoopインストールと設定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
パッケージをインストールし、設定します。

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5/cdh5_all.yml -k -s -e "common_hosts_replace=True"

サービスを初期化します。
なお、HDFSを初期化する手順が含まれていますのでご注意ください。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5/init_zkfc.yml -k -s 
 $ ansible-playbook playbooks/operation/cdh5/init_hdfs.yml -k -s 

サービスを起動します。

.. code-block:: shell

 $ ansible-playbook playbooks/operation/cdh5/start_cluster.yml -k -s 

おめでとうございます
~~~~~~~~~~~~~~~~~~~~~
これでHDFSにアクセスできるようになりました。
client01から以下のコマンドを実行してください。

Example of the command.

.. code-block:: shell

 $ hdfs dfs -ls /

HDFSのウェブサービスには以下のURLからアクセスできます。
片方ががアクティブで、片方がスタンバイです。

* http://master01:50070
* http://master02:50070

YARNのサンプルジョブを実行できます。

Example of the command.

.. code-block:: shell

 $ hdfs dfs -mkdir /user/<user name>
 $ hdfs dfs -chown <user name>:<group name> /user/<user name>
 $ yarn jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar pi 100 100

YARNのウェブサービスには以下のURLでアクセスできます。
片方ががアクティブで、片方がスタンバイです。

* http://master02:8088
* http://master03:8088

Sparkコアのインストール
~~~~~~~~~~~~~~~~~~~~~~~
コマンド例

.. code-block:: shell

 $ ansible-playbook playbooks/conf/cdh5/cdh5_spark.yml -k -s

これでclient01上で、spark-shellなどのコマンドを実行できるようになりました。

.. code-block:: shell

 $ spark-shell

上記コマンドの結果、YARNに接続した状態でSparkのシェルが起動します。
Sparkドライバのウェブサービスにアクセスするには、以下のURLを利用します。

* http://client01:4040
