The main purpose of this playbook is to create an FTP share and samba share that point to the same location. This enables a network scanner to publish its scans via FTP and users to retrieve those scans with a simple SMB share.

# Desired FTP configuration

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

ChatGPT also gave me instructions on creating folders with the right ownership and permissions. I later added a line to the configuration file to disable passive mode.

# Desired samba configuration

This configuration will allow read/write access to the `/home/ftp/shared` directory. I asked ChatGPT to write this configuration file for me as well.

# Running the playbook

Each of the following steps is actually a *role* in this repository.
1. `mount_usb`
1. `configure_vsftpd`
1. `create_users`
1. `create_ftp_directories`
1. `configure_samba`
1. `create_ufw_app_profile`
1. `configure_ufw`

This playbook will run on `all` hosts in the specified inventory file. The playbook is executed with the following command assuming the credentials for the single user to be added to the host are specified in `sample_user.yml`. Ansible requires the use of the `@` if the extra vars are passed as a Yaml file.

```
ansible-playbook -i inventory main.yml -e "@sample_user.yml"
```

The variable file specifies the username and password for each user account to create. See the [`sample_user.yml`](./sample_user.yml) file in this directory as well as the README in the `create_users` role for more details about the format of this file.
