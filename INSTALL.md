# Installation instructions

- Read README.md
- Read an apply CONFIGURATION.md settings, really! did you read it?

## Generate SSL certs or setup LE ones

If you will use a self signed cert just run:

- `ansible-playbook playbooks/gen_ss_certs.yml`

## Create CTs

### Proxmox version

- `ansible-playbook playbooks/create_pmx_cts.yml`

### LXC version

- `ansible-playbook playbooks/create_lxc_cts.yml`

## Set repo, update & upgrade

- `ansible-playbook playbooks/system_update.yml`

## Install the ADDC

This will ERASE previous Domain data, creating a new Domain. This must be run just once, this is not indempotent! (Lazy me, I know...)

- `ansible-playbook playbooks/dc.yml`

## Create users of the domain

This need a previous work, you must get the users lists and copy to the `data/users.xlsx` excel file preserving column names, etc

**Warning:** username field must not contains characters with accents, tilde, etc, you must replace them for it's simple representations (áéíóúñü by aeiounu)

Then export that file to a .cvs format and check it looks like the users.csv file, once you confirmed that, replace the users.csv file for your file.

- `ansible-playbook playbooks/create_domain_users.yml`

This will create the users and the groups, populating the groups and so on...

## Mail server

- `ansible-playbook playbooks/mail.yml`

## Virtual Hosting Server

- `ansible-playbook playbooks/vh.yml`

### Rainloop web server

- `ansible-playbook playbooks/rainloop_on_vh.yml`

The rainloop is set as vanilla, you must configure and link it with the mailserver

## Jabber chat server

- `ansible-playbook playbooks/jabber.yml`

## Squid proxy server

- `ansible-playbook playbooks/squid.yml`

## File server

- `ansible-playbook playbooks/fileserver.yml`

And so on... use your imagination and add your common server config here... 