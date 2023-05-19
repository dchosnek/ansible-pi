# Configure Samba role

This role will install, configure, and enable the Samba file service. The configuration is controlled by [this template](./templates/smb.j2) that will be copied to the host. This allows us to reconfigure the service without having to change any Ansible code.

## Intended configuration

The current configuration file is intended to provide read/write access to a shared directory without authentication. Anyone browsing the network will be able to access this share.

## Variables

This role uses a single variable `samba_path` to specify the directory that will be shared.
