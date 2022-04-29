Ansible Role: openssh
=================

Install and configure the [openssh](https://www.openssh.com/) server.

Project goals:
- Secure with minimal configuration.
- Pubkey only authentication by default.
- Use only modern elliptic curve crypto.
- Accept only ed25519 and ecdsa keys.
- Automatically detect the openssh version and apply settings.
- Compatible with most popular Linux distros and FreeBSD.
- Pass [ssh-audit](https://github.com/jtesta/ssh-audit) with flying colors.

Requirements
------------

No other Ansible roles are required.  If you are running this role remotely, be sure you already have an ecdsa or ed25519 key in your `~/.ssh/authorized_keys` file.

Role Variables
--------------

| Variable Name                   | Default Value | Description                                                                        |
|---------------------------------|---------------|------------------------------------------------------------------------------------|
| openssh_port                    | 22            | TCP port for the SSH server.                                                       |
| openssh_listen_address          | empty         | If provided, specify a list of IP addresses to listen on.                          |
| openssh_address_family          | any           | Permit IPv4 or IPv6 by default.                                                    |
| openssh_allowed_groups          | ssh           | SSH access will be restricted to members of these groups.                          |
| openssh_primary_group           | ssh           | Users in the `openssh_allowed_users` group will be added to this group for access. |
| openssh_allowed_users           | empty         | Users to be added to the `openssh_primary_group` for access.                       |
| openssh_forwarding_users        | empty         | Allow TCP forwarding and SSH agent forwarding for users in this list.              |
| openssh_show_banner             | true          | Display the warning banner on login.                                               |
| openssh_auth_methods            | publickey     | Authentication methods required for successful login.                              |
| openssh_pubkey_auth             | yes           | Allow pubkey authentication.                                                       |
| openssh_password_auth           | no            | Allow password based authentication.                                               |
| openssh_kerberos_auth           | no            | Allow kerberos authentication.                                                     |
| openssh_gssapi_auth             | no            | Allow gssapi authentication.                                                       |
| openssh_permit_root_login       | no            | Permit logins directly as root.                                                    |
| openssh_max_tries               | 5             | Maximum number of authentication attempts.                                         |
| openssh_max_sessions            | 3             | Maximum number of sessions per user.                                               |
| openssh_loglevel                | VERBOSE       | Log level.                                                                         |
| openssh_syslog_facility         | AUTH          | Syslog facility.                                                                   |
| openssh_use_dns                 | no            | Use DNS reverse lookup checks for incoming connections.                            |
| openssh_enable_forwarding       | false         | Enable or disable all connection tunneling / forwarding.                           |
| openssh_enable_agent_forwarding | no            | Enable or disable agent forwarding for all users.                                  |
| openssh_enable_x11_forwarding   | no            | Enable or disable x11 forwarding for all users.                                    |

The variables are used in the default configuration template `templates/etc/ssh/sshd_config_default.j2`.  If you'd like to use your own template for a particular host, create a new template in your playbook template directory with the host name instead of "default" (e.g. `templates/etc/ssh/sshd_config_myhost.j2`).

Example Playbook
----------------

Minimal playbook:
```yaml
---
- name: Install openssh
  hosts: all

  roles:
    - role: wesmarcum.openssh
```

Limit the users allowed to use ssh with the `openssh_allowed_users` variable.  Enable tcp and agent forwarding with `openssh_forwarding_users`:
```yaml
---
- name: Install openssh
  hosts: all

  roles:
    - role: wesmarcum.openssh
      vars:
        openssh_port: 22
        openssh_allowed_users:
          - myuser
        openssh_forwarding_users:
          - myuser
```

Compatibility
-------------

| OS         | Version      |
|------------|--------------|
| Arch Linux | all          |
| Debian     | 10, 11       |
| FreeBSD    | 13           |
| Ubuntu     | 20.04, 22.04 |

License
-------

MIT

Author Information
------------------

https://github.com/wesmarcum/
