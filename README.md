Ansible Role: samba
===================

[![Build Status](https://travis-ci.org/BaxterStockman/ansible-role-samba.svg?branch=master)](https://travis-ci.org/BaxterStockman/ansible-role-samba)

An Ansible role for managing a Samba server installation

Role Variables
--------------

```yaml
### From defaults/main.yml
samba_user: root
samba_group: root

samba_config_dest: '/etc/samba/smb.conf'

smbd_service_name: smbd
nmbd_service_name: nmbd
smbd_service_enabled: no
nmbd_service_enabled: no

samba_users: {}

### Other variables; most are 'omit' by default

# For the `stat` module
follow: "{{ samba_config_follow | default(omit) }}"

# For the `service` module
arguments: "{{ samba_service_arguments | default(omit) }}"
enabled: "{{ samba_service_enabled | default(omit) | bool }}"
pattern: "{{ samba_service_pattern | default(omit) }}"
runlevel: "{{ samba_service_runlevel | default(omit) }}"
sleep: "{{ samba_service_sleep | default(omit) }}"
# The `service` module is only invoked when this is defined, and the smbd and
nmbd services are only restarted when this is true.
enabled: "{{ samba_service_enabled }}"
```

Example Playbook
----------------

Please see [`test/playbook.yml`](test/playbook.yml) for example usage.

Notes
-----

At least on Arch Linux with Python 2.7.10, the `ini_file` (actually, the
`ConfigParser` library that's being used under the hood) module doesn't like
the default `smb.conf` due to leading whitespace before `option = value` pairs.
If you want to use `ini_file` to twiddle your Samba settings, you'll need to
strip the leading whitespace first.  Here's a suggestion:

```yaml
tasks:
  - name: strip leading whitespace from smb.conf
    replace:
      dest: "{{ samba_config_dest }}"
      # Limit replacement to `option = value` lines with leading whitespace
      regexp: '^\s+(.+=.+)'
      replace: '\1'
```

License
-------

MIT

Author Information
------------------

(BaxterStockman)[https://github.com/BaxterStockman]
