# Mount USB drive role

This very simple role uses the `ansible.posix.mount` module to mount a disk and create a record in `fstab` so the mount persists through reboots.

The `passno` field indicates the order of file systems to be checked. File systems with a lower pass number are checked first. Common values used in the "passno" field are shown below. For my purposes, I set it to `0`.
* 0: The file system is not checked.
* 1: The file system is checked first.
* 2: The file system is checked after the file systems with a pass number of 1.

## Variables

The variables do not include any sensitive information, so there is no issue specifying them in the playbook. The two variables specify the friendly name for the mount point for the disk and the Linux device number. I originally created these variables when the playbook was more complex. Now it seems unnecessary to use variables, but I elected to leave them in the event that more tasks are added to this plabyook in the future.

```yaml
vars:
  disk_name: sammy500
  device: /dev/sda1
```

## Future work

It is possible that the Linux device could change from `/dev/sda1` if only one disk is plugged in. It is also possible that this could be confusing if more than one disk is plugged in. It would be ideal to have some other mechanism for identifying the disk. In the meantime, this solution is adequate for my needs but it is something I might investigate later.

## Manual methods

Before mounting a USB drive via Ansible, I did this manually. To mount and then unmount the disk:

```bash
sudo mkdir /media/doron/sammy500
sudo chown doron:doron /media/doron/sammy500
sudo mount -t ext4 /dev/sda1 /media/doron/sammy500/
sudo umount /dev/sda1
```

For the disk to be mounted upon reboot, I added the following line to `/etc/fstab`.

```
/dev/sda1  /media/doron/sammy500 ext4 defaults,nofail 0 1
```

The `ansible.posix.mount` module makes all of this manual effort unnecessary.
