Ansible Role: Docker Swarm
==========================

![CI](https://github.com/fgierlinger/ansible-role-docker-swarm/workflows/CI/badge.svg?branch=master)

An ansible role that configures a docker swarm.

Requirements
------------

* `docker` installed on all hosts
* `python-docker` installed on all hosts

Both requirements can be satisfied as follows:

    - hosts: all
      roles:
        - role: geerlingguy.docker
        - role: geerlingguy.pip
          vars:
            pip_install_packages:
              - name: docker

Role Variables
--------------

Available variables are listed below, along with default values.

    docker_swarm_network_interface: eth0
    docker_swarm_port: 2377

A dedicated master node is used to form the cluster. This node is called primary master and can be set with the `docker_swarm_primary_master_name` variable. The name must match the ansible inventory node name. As default the first member of the current play hosts is taken.

Usage
-----

Include the role in your playbook. All the hosts will become part of the docker
swarm cluster. The first host defined in the inventory wll be used to join the
other hosts.

Example Playbook
----------------

    - hosts: all
      roles:
         - role: fgierlinger.docker_swarm

License
-------

MIT

Author Information
------------------

Frédéric Gierlinger