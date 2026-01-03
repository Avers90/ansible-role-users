# ansible-role-users

Manage system users, groups, and SSH authorized keys.

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
  - name: username              # required
    uid: 1500                   # optional
    gid: 1500                   # optional
    group: primary_group        # optional, default = username
    shell: /bin/bash            # optional, default = users_default_shell
    password: "$6$hash..."      # optional, sha512 hash
    update_password: on_create  # optional: on_create (default) | always
    groups:                     # optional, additional groups
      - sudo
      - docker
    ssh_authorized_keys:        # optional
      - "ssh-ed25519 AAA..."
```

## Password hash generation

```bash
# Option 1: mkpasswd
mkpasswd --method=sha-512 'your_password'

# Option 2: openssl
openssl passwd -6 'your_password'

# Option 3: Python
python3 -c "from passlib.hash import sha512_crypt; print(sha512_crypt.using(rounds=5000).hash('your_password'))"
```

## Examples

### Basic user with SSH key

```yaml
users_create:
  - name: deploy
    groups:
      - sudo
    ssh_authorized_keys:
      - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5..."
```

### User with password and custom UID/GID

```yaml
users_create:
  - name: appuser
    uid: 1500
    gid: 1500
    password: "{{ vault_appuser_password }}"
    groups:
      - docker
    ssh_authorized_keys:
      - "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5..."
```

### Remove users

```yaml
users_remove:
  - olduser
  - testuser
```

## Usage

```yaml
- hosts: all
  become: true
  roles:
    - users
```

## License

MIT
