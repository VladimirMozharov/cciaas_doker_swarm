[![Build Status](https://travis-ci.com/VladimirMozharov/CCIaaS_Kiratech.svg?branch=main)](https://travis-ci.com/VladimirMozharov/CCIaaS_Kiratech)

Code Challenge IaaS (cciaas) Docker Swarm for Kiratech
=========

Control free disk space and deploy docker swarm on CentOS 8 VMs.
### How the role works:
1. Setups up were docker root folder and minimum available space.
2. Controls if available space on VM is sufficient. If not - playboor fails with message "...space is not sufficient. Increase it and try again."
3. If docker root folder is on dedicated LV - tries to occupy all free space on VG.
4. Installs iptables (without docker daemon doesn't start)
5. Installs docker-ce by dedicated role
6. Creates docker group and docker user, and copy SSH key to permit remote connection to docker API
7. If necessary, setups through daemon.json file docker root folder.
8. Inits docker swarm by dedicated role

Role Variables
--------------

    min_space_available: 40000000000 # in bytes
    docker_default_dir: "/"
    default_block_device: "/dev/vda"
    use_alternative_docker_root_dir: false
    alternative_docker_dir: '/mnt/docker'
    alternative_block_device: "/dev/sda"
    storage_driver: "overlay2"

Dependencies
------------

Following roles must be installed:
- https://github.com/geerlingguy/ansible-role-docker
- https://github.com/geerlingguy/ansible-role-pip
- https://github.com/fgierlinger/ansible-role-docker-swarm
- https://github.com/kevincoakley/ansible-role-disk

Example Playbook
----------------

    - name: Deploy Docker Swarm on all VM
      hosts: all
      become: yes
      become_user: root

      vars:
        min_space_available: 20000000000

      roles:
      - role: vladimir_mozharov.cciaas_doker_swarm

Example hosts file
----------

    swarm-01 ansible_ssh_host=127.0.0.1
    swarm-02 ansible_ssh_host=127.0.0.2

    [docker_swarm_engine]
    swarm-01
    swarm-02

    [docker_swarm_manager]
    swarm-01

    [docker_swarm_worker]
    swarm-02


License
-------

BSD
