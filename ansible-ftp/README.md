# Install and configure FTP via Ansible

This repo will install and configure FTP on a Raspberry Pi running Debian 11 (bullseye) with Ansible 2.14.4. The commands are relatively simple, so this is likely to work with other versions of Debian and Ansible.

The playbook is organized by roles so it's easier to include or remove operations and share code with other projects.

## Desired FTP configuration

This configuration will allow each user to access a folder named `ftp` in their home directory. Each user will be constrained to only their own directory. I did quite a bit of reading through various guides, blogs, and sites like stackoverflow before finally deciding to ask ChatGPT to write the configuration file for me.

**Me**: "Please write for me a vsftpd.conf file that will configure a Linux FTP server to allow read/write access for each user to their own home directory."

**ChatGPT**:

```properties
# Sample vsftpd.conf file
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
chroot_local_user=YES
listen=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
user_sub_token=$USER
local_root=/home/$USER/ftp
pasv_min_port=30000
pasv_max_port=31000
allow_writeable_chroot=YES
```

I changed one line in the file recommended by ChatGPT because I did not want to specify a list of allowed users.

```properties
userlist_enable=NO
```

## Ansible steps

Each of the following steps is actually a *role* in this repository.
1. `add_user` adds the specified user with the specified password.
1. `add_home_subdir` adds an `ftp` directory for every non-root interactive user. Interactive users are those who can actually log into the Linux shell. The new directory will have the appropriate permissions for FTP.
1. `vsftpd` will install the FTP service, set the proper configuration for the FTP server, and restart the service if needed.

## Running the playbook

This playbook will run on `all` hosts in the specified inventory file. The playbook is executed with the following command assuming the credentials for the single user to be added to the host are specified in `sample_user.yml`. Ansible requires the use of the `@` if the extra vars are passed as a Yaml file.

```
ansible-playbook -i inventory main.yml -e "@sample_user.yml"
```

### Inventory file

The inventory file is fairly standard, but should include the `ansible_user` property to ensure proper connectivity to the remote host.

```properties
[ftp]
192.168.1.100  ansible_user=ubuntu
```

### Variable file

The variable file specifies the username, password, and salt for the user to add to the host. The salt could have been generated dynamically, but that would make the role of adding the user non-idempotent. A new salt would be created every time and therefore the user would be updated every time the playbook is executed.

```yaml
---
user_name: myuser
user_psswd: mybadpassword
user_salt: 8x8r7PPzcOvJpm3d
```


# Generating encrypted passwords

https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module

```
pip install passlib
ansible all -i localhost -m debug -a "msg={{ 'mypassword' | password_hash('sha512', 'mysecretsalt') }}"
```


For JSON queries to work in Ansible

```
pip install jmespath
```