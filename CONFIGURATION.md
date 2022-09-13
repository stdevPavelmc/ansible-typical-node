# This is the configuration how to for the whole repository

The main file is `vars/domain.yml`, it has most of the global configuration setting in there. We use variable nesting to pack specific details under ceirtain variables.

You have also the `vars/secrets.yml` file that store the main passwords to use on the whole configuration. Please use ansible vault to crypt the file once you set up your own passwords.

Other configs can be found on the `vars` folder as usual in Ansible.

**WARNING:** if you don't have experience with ansible, the values with `{{ something }}` are dynamic values, DO NOT TOUCH THEM; just modify the ones with normal values.

## Networking and hosts

This setup is planned with a three leg firewall, separating the LAN network (where the users will reside), the DMZ (where the servers will reside) and the WAN (external network with direct/indirect internet access)

By default the DMZ network will be our target, it has the following parameters (see section net on the `vars/domain.yml` file)
- net: 192.168.0.0/255.255.255.224 (/27 in CIDR, 30 hosts)
- gw/firewall: 192.168.0.1
- etc, see comments

### Inventory

Ansible inventory file to use, as we are setting up a `from scratch` services no DNS will exist, so we need to hardwire the names and IPs of the hosts:

- For proxmox use `inventories/hosts.ini`
- For lxc use `inventories/hosts_lxc.ini`

TIP: to use the correct inventory please do this:

- For proxmox, please go to the file `ansible.cfg` and uncomment the line for proxmox and comment the line for lxc
- For lxc, please go to the file `ansible.cfg` and uncomment the line for lxc and comment the line for proxmox

### proxmox / LXC common details

There is another file in which we need to set the details of the hosts: `vars/cts.yml`, in this file we set the following:

- Proxmox credentials and public ssh key of the configuring PC to setup in the lxc to allow passwordless access `api_pubkey`
- Default proxmox properties for the CTs, if you need to specify a diferent value (lower or greater) do it on the next section
- CTs details, in this we define one by one the cts properties by the names (must match the values of the host.ini file)

### Users handling

In the `data` directory you will find a file called `users.xlsx`, that file must contain the users to be created on the domain servers (Yes this recipe will make that for you)

Fill the file with real data, check the headers comments and follow them.

You must setup at least one user, the domain admin; the users fields will be used like this:

- `username` will be `lower(givenname).firt(surname)` and that will be the email address for it. You are clever to make the username column dynamic, but export it to plain before processing it or you will have errors.
- `ou` is the OU in the AD LDAP tree and the name of the group where the user belongs to.
- `proxy` is a proxy group, see comments.
- NEVER and I mean, NEVER use a comma "," on any field, as it will broke the csv export
- DO NOT CHANGE the headers names on the file, the ones shaded with light blue.

After filling the file, export it as CSV, you have a sample on the `data` folder names `users.csv`. That file in the one the script will use.

Now you can use the `INSTALLATION.md` file to proceed.
