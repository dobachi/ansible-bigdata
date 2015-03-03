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

なお、group_vas/topに記載しても良いです。
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

 $ ansible-playbook playbooks/operation/ec2/hadoop_nodes_up.yml -k

成功した場合は */tmp/ec2_<プレイブックを実行したときのUNIXエポック時刻>* のディレクトリ以下に以下の内容のファイルが配備されています。
EC2インスタンスにアクセスしたり、EC2インスタンスによるクラスタを構成管理する際に利用してください。

* プレイベートIPアドレス、パブリックIPアドレスの一覧
* クラスタ内で利用できるAnsibleインベントリファイルのサンプル
* クラスタ内で利用できる/etc/hostsファイルのサンプル

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

.. vim: ft=rst tw=0 et ts=2 sw=2
