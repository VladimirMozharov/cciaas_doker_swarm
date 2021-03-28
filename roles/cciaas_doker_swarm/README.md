Role Name
=========

Control free disk space and deploy docker swarm on CentOS VMs

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
  - role: cciaas.doker_swarm

License
-------

BSD
