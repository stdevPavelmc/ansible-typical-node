# Installation steps

See CONFIGURATION.md file first.

## Pre-requisites

For proxmox, please go to the file `ansible.cfg` and uncomment the line for proxmox and comment the line for lxc

For lxc, please go to the file `ansible.cfg` and uncomment the line for lxc and comment the line for proxmox

## Install, finally!

For proxmox:

```sh
ansible-playbook playbooks/do_proxmox.yml
```

For LXC:

```sh
ansible-playbook playbooks/do_lxc.yml
```
