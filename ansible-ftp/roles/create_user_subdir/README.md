# Create user subdirectory role

This role creates a single subdirectory under each **interactive non-root** user account. The role determines the list of users who have a login but are not root by reading `/etc/passwd`. The name of the new subdirectory is determined by the variable `newdir`.

For example, if `newdir` is set to "ftp", directories are created:

```
/home/user1/ftp
/home/user2/ftp
/home/user3/ftp
 .
 .
 .
```

This role was created when the approach of this playbook was for each user to have his/her own FTP directory. That is no longer the approach, so this role is not needed. It is included here for reference.
