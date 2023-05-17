# Create FTP directories role

This role creates the directories along with the ownership and permissions required to have a shared FTP directory. The role can use default values to create the following directory structure.

```
/home/
└── ftp            0755 root:root 
    └── shared     0777 ftp:ftp
```

This requires that the `ftp` user and `ftp` group exist prior to running this role with the defaults. Those are both created when `vsftpd` is installed.

You can also choose to provide values for the variables `parent` and `child` if you prefer to name the directories to something other than the defaults.


```
/home/
└── parent         0755 root:root 
    └── child      0777 ftp:ftp
```