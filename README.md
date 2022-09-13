# Playbooks and settings to setup a full node in proxmox

**NOTE:** This setup is tested on Ubuntu 20.04 LTS as of August 2022, **DO NOT EXPECT** any updates by my side, if you find an error and want to fix it, clone the repository and make the changes on your own, then make a pull request and I will evaluate to make the change available for everyone.

## Target

This ansible playbook collection is intended to setup a simple IT node that will cover most of the services used in an samll/medium/large? enterprise. This node covers:

- **Active Directory Domain controller (ADDC)**, using Samba and this will be the centralized authentication authority in the domain for all (really, **ALL**) the other services, this include:
  - Kerberos (authentication)
  - DNS (only for the domain, non recursive for security reasons)
  - LDAP (server for the active directory)
  - Domain Policy service (for the domain policies)
- **Mail server**, using [MailAD](https://github.com/stdevPavelmc/mailad) solution, but as an ansible role. Users came from the ADDC.
- **Caching DNS server**, as a proxy caching DNS server for the external address, caching will screen us from external DNS outages. This server must be only used/exposed to/by DMZ servers that require external DNS resolution, but the DC and the LAN users.
- **Windows File Server**, a dedicated windows file server beside the domain controller, this is usefull to store user profiles, personal maped drives, public/shared folders, etc.
- **Web server**, in the form of a unified virtual hosting solution. Using the full featured Nginx web server, it also has a webmail.tld service in place to access the email service (requires extra configuration). With a simple syntax you can create more virtual hosting for www, cloud, etc.
- **XMPP/Jabber chat server**, using ejabberd server solution. Users came from the AD DC.
- **Database server**, A MyQSL compatible MariaDB server with a own instance of PhpMyAdmin to manage the DB server.
- **Squid proxy**, a squid proxy server with user autentication and basic access control via LDAP groups.

This deployment may be setup on a host using bare LXC or on a proxmox host.

Each service setup and features will be described later on.

## Preparations for installation

Did I mentioned I tested this on Ubuntu 20.04 LTS, other systems are not supported? (not supported like: It may not receive any updates or correction from me, beside the ones the community can provide via merge-request)

### In your PC

0. Clone this repository on a known folder.
0. Once cloned do a `git submodule update` inside the created directory to get the roles we use under the hood.
0. Install ansible from repository or via pip `sudo apt install ansible` or `sudo pip3 install ansible ansible-core ansible-lint` this later is preffered.
0. Install ansible galaxy community "roles" `ansible-galaxy collection install community.general`
0. Generate a ssh key to manage the proxmox server (Only if you use proxmox, for LXC it's not needed)

### Proxmox Server (preffered)

0. Install proxomox
0. Config proxmox to access internet and update it (proxy, etc.)
0. Config community repository and do a full upgrade.
0. Install python3-proxmoxer `apt install python3-proxmoxer`.
0. Get sure your proxmox server can access the outside works via ntp port (123 tcp/udp) to get time sync; or setup your own timesync alternative (ntp, chrony, etc.)
0. If more than one server/Cluster, do the above steps for all of them.
0. If you plan to use several servers (three or more is recommended, not less) crate the cluster and join all servers.
0. For now we will create all CTs on **one** server, you will need to move them to other servers on the cluster at the end to blance the load.

### Your PC & Proxmox server linking

0. Login using ssh on each server as root (this to accept fingerprints)
0. Copy your ssh key onto every server `ssh-copy-id root@[server.ip/name]`.
0. Test ssh password less login on each one.

### Bare LXC (alternative for testing or minimal systems)

You can deploy this services on the host PC with LXC/LXD services, this is mainly used for testing & demostrations.

But don't diminish its power, it's fully functional and can work as a fast solution under a crysis or principal system crash to provide fast business recovery; that providing enough resources to support all the services.

Setting up the LXC is out of the scope of this explanation, please refer to [ubuntu community](pending_link) lxc page for details

## Configuration

Almost all configuration issues are in a few files, that's the way we planed, please take special care to read the comments on those files as the comments has clues and tips for the configuration.

### Passwords

All passwords will be stored on the var file secrets.yml `vars/secrets.yml`, in the distribution it will be showed with dummy passwords but right after you set your own passwords please use ansible vault to crypt the file to protect your credentials.

**Note:** if you use an Ansible vault, you must provide the command line parameters with a appropiate switch to ask the vault password.

### Configuration and planning

You need to have at least a minumim knoledge of networking and linux server administration to undestand the following configurations, as this is a complex setup. All config steps are described on the [CONFIGURATION](CONFIGURATION.md) file.

## This is free software!

All contents of this repository is licended under GPLv3 (see [LICENSE.GPLv3](LICENSE.GPLv3) file for details), the uathor will thanks any citation on derived works.

## Contribution & donations.

If you want to contribute with code, fixes, improvements, you are welcomed! please use the issues/pull request for that.

Donations of any type are welcommed, Crypto, CUP/MLC/USD/USDT/BITCOIN, cell topups, hardware, etc. Give me a call at @pavelmc in Telegram or @co7wt in Twitter for details.

**Thank you very much in advance!**
