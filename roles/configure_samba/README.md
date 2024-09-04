# Configure Samba role

This role will install, configure, and enable the Samba file service. The configuration is controlled by [this template](./templates/smb.j2) that will be copied to the host. This allows us to reconfigure the service without having to change any Ansible code.

## Intended configuration

The current configuration file is intended to provide read/write access to two shared directories without authentication. Anyone browsing the network will be able to access these shares.

## Variables

This role uses two variables to specify the two directories that will be shared:

* `public_share_path`
* `samba_path`
