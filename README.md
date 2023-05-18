# Ansible playbooks and roles for Raspberry Pi

This repo contains playbooks and associated roles for configuring the Raspberry Pi running Debian 11 (bullseye) with Ansible 2.14.4 for different purposes. The playbooks contained at the root of this repository mostly call **roles** in this repository rather than executing tasks themselves.

# Playbooks

## Configure file server

The `configure_fileserver.yml` playbook configures the following services:
* FTP
* Samba
* VNC
* UFW (uncomplicated firewall)

It will also mount the attached USB drive and make that mount persistent across reboots. Check the [README](./CONFIGURE_FILESERVER.md) for more information.

## Configure Plex media server

The `configure_plex.yml` playbook installs the Plex Media Server and configures UFW to allow the Plex management port so it can be managed from another host. This is a simple playbook so there is no additional documentation required.

# Roles

Each role has its own README, so I will not duplicate that documentation here.

# Inventory file

The inventory file is standard, but should include the `ansible_user` property to ensure proper connectivity to the remote host. Here is an example of what the inventory file might look like.

```properties
[pi]
192.168.1.100  ansible_user=ubuntu
```
