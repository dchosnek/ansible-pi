# Install and configure Samba file server via Ansible

This repository installs and configures samba file server and configures the UFW (uncomplicated firewall) to allow samba traffic.

This playbook configures samba by replacing the host's `smb.conf` file with the file in the `configure_samba` role in this repository.

## Running the playbook

This playbook will run on `all` hosts in the specified inventory file. The playbook is executed with the following command.

```
ansible-playbook -i inventory main.yml
```

### Inventory file

The inventory file is fairly standard, but should include the `ansible_user` property to ensure proper connectivity to the remote host.

```properties
[ftp]
192.168.1.100  ansible_user=ubuntu
```