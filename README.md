# CCIaaS_Kiratech
**Code Challenge IaaS Kiratech**

## Steps of project:
1. Two VM CentOS 8 (for beginning there is just one for cost's reduce) hosts on Digitalocean

2. check_disk.yml & create_lvm.yml
Ensure that the free space is more than 40 Gb.
The problem is that it is "impossible" (almost) increase the space on system disk. So it is possible to create lvm and guarantee that all space of lvm is used even if it is added after.

3. docket_deploy.yml
Deploy of Docker CE of VM using https://github.com/geerlingguy/ansible-role-docker docker role instead of recommended https://github.com/nickjj/ansible-docker/tree/v1.9.0 because last one is only for ubuntu and use 'apt' to install packages.

4. docket_deploy.yml
To expose docker REST API in secure way there are lots of different actions: https://docs.docker.com/engine/security/. I'm going to implement TLS certificates.
There is one strange glitch - daemon.json doesn't work. So I use role that put all info (tcp & certificates) in systemd docker conf. Now I'm using this to deploy docker - https://github.com/jgeusebroek/ansible-role-docker.

5. To add docker swarm i needed to rework all code about docker. Looks like all works stable.
To secure the connection instead TLS (that always requires some troubleshooting) I decided to use SSH connection that even simpler. I tried some different roles and choose these three to complete the task:
    - https://github.com/geerlingguy/ansible-role-docker
    - https://github.com/geerlingguy/ansible-role-pip
    - https://github.com/fgierlinger/ansible-role-docker-swarm

6. Transformated all separated scripts in one role (cciaas_doker_swarm) using standart ansible template and fixed the logic. Added some new features to improove functionality.

7. Setup Travis CI ansible lint control.


## How the role works:
1. Setups up were docker root folder and minimum available space.
2. Controls if available space on VM is sufficient. If not - playboor fails with message "...space is not sufficient. Increase it and try again."
3. If docker root folder is on dedicated LV - tries to occupy all free space on VG.
4. Installs iptables (without docker daemon doesn't start)
5. Installs docker-ce by dedicated role
6. Creates docker group and docker user, and copy SSH key to permit remote connection to docker API
7. If necessary, setups through daemon.json file docker root folder.
8. Inits docker swarm by dedicated role

**After each "git push" Travis CI executes ansible lint of role's code and fails if it finds some errore.**