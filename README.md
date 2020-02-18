# Ansible Role: nsd

An Ansible role that installs and configures nsd and also manages zone files.

## Requirements

None.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `nsd_user` | User name of `nsd` | `{{ __nsd_user }}` |
| `nsd_group` | Group name of `nsd` | `{{ __nsd_group }}` |
| `nsd_service` | Service name of `nsd` | `{{ __nsd_service }}` |
| `nsd_package` | Package name of `nsd` | `{{ __nsd_package }}` |
| `nsd_conf_dir` | Path to directory where `nsd.conf` resides | `{{ __nsd_conf_dir }}` |
| `nsd_zones_dir` | Path to directory where zone files reside | `{{ __nsd_zones_dir }}` |
| `nsd_db_dir` | Path to directory where `nsd` database file reside | `{{ __nsd_db_dir }}` |
| `nsd_conf_file` | Path to `nsd.conf` | `{{ nsd_conf_dir }}/nsd.conf` |
| `nsd_bin` | Path to `nsd-checkconf` | `{{ __nsd_bin }}` |
| `nsd_flags` | Addtional flags to `nsd` daemon | "" |
| `nsd_zones_inputdir` | Local path to directory where your zone files reside | "" | 
| `nsd_role` | Either `standalone`, `master` or `slave` | `standalone` |
| `nsd_zone` | A list of zones | see below |
| `nsd_keys` | A list of keys for AXFR | see below |

### `nsd_zone`

This variable is a list of dict of zone files this role will handle.

| Variable | Description | Mandatory? |
|----------|-------------|---------|
| name | Zone name | yes |
| state | Either `absent` or `present` | no |
| key | Key to use if `nsd_role` is not `standalone` | depends |

### `nsd_keys`

This variable is a list of dict describing keys for `nsd`.

| Variable | Description | Mandatory? |
|----------|-------------|---------|
| name | Key name | yes |
| algorithm | Key algorithm | no, default to `hmac-sha256` |
| secret | Key content | yes |

### OpenBSD

| Variable | Default |
|----------|---------|
| `__nsd_user` | `_nsd` |
| `__nsd_group` | `_nsd` |
| `__nsd_service` | `nsd` |
| `__nsd_package` | "" |
| `__nsd_conf_dir` | `/var/nsd/etc` |
| `__nsd_zones_dir` | `/var/nsd/zones` |
| `__nsd_db_dir` | `/var/nsd/db` |
| `__nsd_bin` | `/usr/sbin/nsd-checkconf` |

## Dependencies

None.

## Example Playbooks

```
$ ls zones/
example.org example.com bogus.domain
```

```
- hosts: ns
  roles:
  - role: ansible-role-nsd
    nsd_ip4: "{{ ansible_default_ipv4.address }}"
    nsd_zones_inputdir: zones/
    nsd_zones:
      - name: example.org
      - name: example.com
      - name: bogus.domain
        state: absent
```

## License

ISC