ansible-role-disk
=================

![](https://github.com/kevincoakley/ansible-role-disk/workflows/Molecule%20Test/badge.svg)

Partition, Manage LVM, Format and Mount Disks for CentOS 7, 8 and Ubuntu 18.04, 20.04

Requirements
------------

None

Role Variables
--------------

See defaults/main.yml

Dependencies
------------

None

Example Playbook
----------------

    - name: Partition, Manage LVM, Format and Mount Disks on Linux
      hosts: disk
      become: yes
      become_method: sudo
    
      vars:
        parted:
          - device: /dev/vdc
            number: 1
            label: gpt
            state: present
        lvg:
          - vg: vg_test
            pvs: /dev/vdc1
        lvol:
          - vg: vg_test
            lv: lv_test
            shrink: no
            size: 100%FREE
        filesystem:
          - fstype: xfs
            dev: /dev/vg_test/lv_test
        mount:
          - path: /mnt/volume-test-1
            src: /dev/vg_test/lv_test
            fstype: xfs
            opts: defaults
        
      roles:
        - ansible-role-disk

License
-------

BSD

Author Information
------------------

Kevin Coakley (https://github.com/kevincoakley)