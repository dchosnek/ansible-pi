# Configure UFW role

This role will install UFW (Ucomplicated firewall) and configure it to block all traffic except SSH (which is required for Ansible to continue to work). It will also set UFW to `allow` traffic for one predefined application.

To see which applications are available on the host:

```bash
$ sudo ufw app list
Available applications:
  AIM
  Bonjour
  CIFS
  CUPS
  DNS
  Deluge
  .
  .
  .
```

You can then use this role to enable one of those roles as shown below or leave the `app` variable undefined (don't include it) if you prefer not to enable any applications through the firewall.

```yaml
- role: configure_ufw
    vars:
      app: CIFS
```

## Prerequisties

This role uses the module `community.general.ufw` so it requires running the following command on the Ansible control host.

```
ansible-galaxy collection install community.general
```