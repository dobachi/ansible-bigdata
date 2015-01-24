ganglia_slave
===============

This is a role to configure Ganglia gmond.

Requirements
------------
* You can use multicast network

Role Variables
--------------

Parameters | Default value | notes
ganglia_slave_cluster_name | hadoop_cluster | -
ganglia_slave_cluster_owner | hadoop_owner | -
ganglia_slave_cluster_latlong | unspecified | -
ganglia_slave_cluster_url | unspecified | -
ganglia_slave_location | hadoop_place | -
ganglia_slave_mcast_join | 239.2.11.71 | This is a multicast addresss
ganglia_slave_port | 8649 | -
ganglia_slave_dev | eth1 | This is a device used by gmond

Dependencies
------------
* You can use this role with ganglia_master, which is a role to configure Ganglia gmetad.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: dobachi.ganglia_slave }

License
-------

BSD

Author Information
------------------
dobachi
