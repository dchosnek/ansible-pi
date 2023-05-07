# Install and configure FTP via Ansible

This repo will install and configure FTP on a Raspberry Pi running Debian 11 (bullseye) with Ansible 2.14.4. The commands are relatively simple, so this is likely to work with other versions of Debian and Ansible.

The playbook is organized by roles so it's easier to include or remove operations and share code with other projects.

## Desired FTP configuration

This configuration will allow each user defined on the host to access a shared FTP folder at `/home/ftp/shared`. I did quite a bit of reading through various guides, blogs, and sites like stackoverflow before finally deciding to ask ChatGPT to write the configuration file for me.

**Me**: "Please give me the complete instructions for configuring the Linux vsftpd service to allow all users to read and write to a shared FTP folder. Please write the configuration file for me and also show me the folder ownership and permissions required to avoid authentication and write errors."

**ChatGPT**:

```properties
# Enable FTP and set the listening port
listen=YES
listen_port=21

# Enable local users to log in
local_enable=YES

# Enable write access for users
write_enable=YES

# Allow users to create new directories
local_umask=022
file_open_mode=0777

# Set the FTP root directory
chroot_local_user=YES
local_root=/home/ftp

# Allow users to see hidden files
force_dot_files=YES
```

ChatGPT also gave me instructions on creating folders with the right ownership and permissions.

## Ansible steps

Each of the following steps is actually a *role* in this repository.
1. `vsftpd` will install the FTP service, set the proper configuration for the FTP server, and restart the service if needed.
1. `create_users` creates the specified local user accounts with the specified passwords.
1. `create_ftp_directories` creates two directories with the proper ownership and permissions as specified by ChatGPT.

## Running the playbook

This playbook will run on `all` hosts in the specified inventory file. The playbook is executed with the following command assuming the credentials for the single user to be added to the host are specified in `sample_user.yml`. Ansible requires the use of the `@` if the extra vars are passed as a Yaml file.

```
ansible-playbook -i inventory main.yml -e "@sample_user.yml"
```

If you don't want to add any users to the host(s), you can remove the `create_users` role from the playbook and run the playbook without specifying extra args.

### Inventory file

The inventory file is fairly standard, but should include the `ansible_user` property to ensure proper connectivity to the remote host.

```properties
[ftp]
192.168.1.100  ansible_user=ubuntu
```

### Variable file

The variable file specifies the username and password for each user account to create. See the `sample_user.yml` file in this directory as well as the README in the `create_users` role for more details about the format of this file.
