# Configure vsftpd role

This role will install, configure, and enable the `vsftpd` FTP service. The configuration is controlled by [this template](./templates/vsftpd.j2) that will be copied to the host. This allows us to reconfigure the service without having to change any Ansible code.

## Intended configuration

The current configuration file is intended to provide all interactive users read/write access to a shared FTP directory. Anonymous access is disabled. FTP passive mode is also disabled to make firewall configuration more straightforward.

## Variables

This role uses a single variable `ftp_path` to specify the directory that will be shared.
