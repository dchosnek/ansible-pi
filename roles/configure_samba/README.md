# Configure Samba role

This role will install, configure, and enable the Samba file service. The [configuration file](./templates/smb.j2) is relatively simple, enabling read/write access to all network users to a single directory.

This role uses a single variable `samba_path` to specify the directory that will be shared.
