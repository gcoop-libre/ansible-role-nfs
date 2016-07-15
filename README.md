Role Name
=========

Installs NFS utilities on RedHat/CentOS or Debian/Ubuntu.

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    nfs_exports: []

A list of exports which will be placed in the `/etc/exports` file. See Ubuntu's simple [Network File System (NFS)](https://help.ubuntu.com/14.04/serverguide/network-file-system.html) guide for more info and examples. (Simple example: `nfs_exports: { "/home/public    *(rw,sync,no_root_squash)" }`).

    nfs_random_port: '63700'

    nfs_random_ports:
      nfs_callback: "{{ (nfs_random_port | int + 0) }}"
      nfs_lockd: "{{ (nfs_random_port | int + 1) }}"
      nfs_mountd: "{{ (nfs_random_port | int + 2) }}"
      nfs_statd: "{{ (nfs_random_port | int + 3) }}"
      nfs_statd_bc: "{{ (nfs_random_port | int + 4) }}"

NFS uses 2 fixed ports (`111` for portmap and `2049` for NFS). Additionally NFS opens 5 random ports to stablish the communication with the clients. This properties fix this ports so you can open them in the hosts firewall. The role uses by default 5 consecutive ports: `63700` to `63704`.

Dependencies
------------

- `geerlingguy.nfs`

Example Playbook
----------------

    - hosts: fileservers
      vars_files:
        - vars/main.yml
      roles:
         - role: gcoop-libre.nfs

*Inside `vars/main.yml`*:

    nfs_random_port: '12345'
    nfs_exports:
      - "/home {{ ansible_eth0.ipv4.network }}/24(rw,sync,no_root_squash)"

License
-------

GPLv2

Author Information
------------------

This role was created in 2016 by [gcoop Cooperativa de Software Libre](http://gcoop.coop).

It extends the functionality of the role created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
