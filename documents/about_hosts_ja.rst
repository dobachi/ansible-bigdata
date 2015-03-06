hostsについて
=========================
本プレイブック集には2種類のhostsサンプルが付属しています。
主なコンポーネント構成は以下の通りです。

hosts.large_sample:

 * NameNode 2台
 * Zookeeper兼JournalNode 3台
 * ResourceManager 2台
 * Client 1台
 * Slave 10台

hosts.medium_sample:

 * Master 3台
 * Client 1台
 * Slave 5台

hosts.medium_sampleについては、マスタ系サービスが一部同居構成になっています。
