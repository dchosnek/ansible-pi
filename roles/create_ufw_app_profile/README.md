# Create UFW application profile role

This role creates a profile for an application for UFW (uncomplicated firewall). Application profiles are stored in the folder `/etc/ufw/applications.d` and look something like the following:

```ini
[CIFS]
title=SMB/CIFS server
description=SMB/CIFS server
ports=137,138/udp|139,445/tcp

[NFS]
title=NFS server
description=NFS and portmap server. Will also need access to mountd, statd and possibly others
ports=2049,111/tcp|2049,111/udp
```

This role will simply copy the specified `.ini` file from the role's `files` folder to each host. To start, I just created a file for `vsftpd` that specifies the TCP protocol on ports 20 and 21. No additional port ranges are specified for passive mode.

The `app` variable must contain the base name of a file in the `files` folder of this role. In the example below, it is expected that there be a file called `vsftpd.ini` locally in this role.

```yaml
- role: create_ufw_app_profile
    vars:
      app: vsftpd
```

Note: **This role does not configure UFW to use the application profile that it creates**. It just creates the profile.