# Configure UFW role

This role will install, configure, and enable the UFW (Ucomplicated firewall) service. The role will disable IPv6, allow OpenSSH (required for Ansible to continue to work), and allow a list of applications specified through a variable.

This role will *add* rules that `allow` applications. It will not remove or change existing rules. Therefore, this role can be used by multiple playbooks to `allow` many applications on the same host.

Those applications are specified by name and must exist in one of the files in the `/etc/ufw/applications.d/` directory.

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

If the desired application is not listed, use [this role](../create_ufw_app_profile/) to create a new application profile.

## Variables

This role uses a single variable `app_list` to specify a list of applications to allow through the firewall. This variable is **optional**. If it is omitted, the role will only enable OpenSSH.

The following is an example that would allow three applications through UFW. UFW does not seem to be case sensitive.

```yaml
app_list:
  - XMPP
  - dns
  - samba
```

## Prerequisties

This role uses the module `community.general.ufw` so it requires running the following command on the Ansible control host.

```
ansible-galaxy collection install community.general
```
