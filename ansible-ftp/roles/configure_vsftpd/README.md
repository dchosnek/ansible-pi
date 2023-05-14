# vsftpd role

This role will install, configure, and enable the `vsftpd` service. Rather than modify the configuration file on the host, this role [contains a configuration file](./files/vsftpd.conf) that will be used to overwrite it.

Steps:
1. Install the `vsftpd` package.
1. Create a backup of the `vsftpd.conf` file only once so that a backup of the default configuration is available on the host.
1. Copy the `vsftpd.conf` file from this role to the host if it differs from what is on the host.
1. Restart the `vsftpd` service only if the configuration file was changed in the previous step.
1. Enable the `vsftpd` service to start automatically on reboot.
