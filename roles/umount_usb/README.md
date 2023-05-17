# Unmount USB drive role

This very simple role uses the `ansible.posix.mount` module to **unmount** a disk and remove its record in `fstab` so the unmount persists through reboots.
