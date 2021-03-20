# CCIaaS_Kiratech
Code Challenge IaaS Kiratech

Two VM CentOS 8 (for beginning there is just one for cost's reduce)

1. check_disk.yml & create_lvm.yml
Ensure that the free space is more than 40 Gb.
The problem is that it is "impossible" (almost) increase the space on system disk. So it is possible to create lvm and guarantee that all space of lvm is used even if it is added after.

2. docket_deploy.yml
Deploy of Docker CE of VM using https://github.com/geerlingguy/ansible-role-docker docker role instead of recommended https://github.com/nickjj/ansible-docker/tree/v1.9.0 because last one is only for ubuntu and use 'apt' to install packages.