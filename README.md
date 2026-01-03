# ansible-role-users

Manage system users and SSH authorized keys.

## Requirements

- Debian/Ubuntu
- Collection: `ansible.posix`

## Role Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `users_create` | `[]` | List of users to create |
| `users_remove` | `[]` | List of usernames to remove |
| `users_default_shell` | `/bin/bash` | Default shell |

### User structure

```yaml
users_create:
  - name: username        # required
    group: primary_group  # optional, default = username
    shell: /bin/bash      # optional
    groups:               # optional
      - sudo
      - docker
    ssh_authorized_keys:  # optional
      - "ssh-rsa ..."
```

## Example

```yaml
- hosts: all
  become: true
  roles:
    - users
```
