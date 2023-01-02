# Ansible role: dnscrypt

[![MIT licensed][mit-badge]][mit-link]
[![Galaxy Role][role-badge]][galaxy-link]

DEPRECATION NOTICE
----

*(Left here for reference only)*

Description
----

Ansible role which installs and configures [DNSCrypt-proxy2][dnscrypt-proxy2-link].

The role achieves the following:

 - Installs dnscrypt-proxy 2
 - Configures dnscrypt-proxy to be run as unprivileged user [ for security purposes ]
 - If distro uses systemd init: configures systemd socket activated dnscrypt-proxy.service
 - If distro does not use systemd: configures built in dnscrypt-proxy service

Requirements
------------

NOTE: Role requires Fact Gathering by ansible!

One of the following distros (or derivatives) required:

 - Debian | Raspbian | [Minibian][minibian-link]
    - jessie
    - stretch
    - buster

Role Variables
--------------

| Variable | Description | Default |
|----------|-------------|---------|
| `dnscrypt_arch` | System architecture dictionary, containing two keys: `archive` [architecture name in .tar.gz] and `dir` [arch name of archived directory]. See [available architectures][dnscrypt-arch-link]. Check [`defaults/main.yml`](defaults/main.yml) | `{ archive: linux_arm64, dir: linux-arm64 }` |
| `dnscrypt_default_version` | Fallback version in case the website doesn't have info about latest version | `2.1.12` |
| `dnscrypt_on_pihole` | Is it dnscrypt on [pihole][pihole-link] installation? If `yes` then it will modify pihole config | `no` |
| `dnscrypt_bin` | Where to put dnscrypt-proxy binary | `/usr/local/bin/dnscrypt-proxy` |
| `dnscrypt_config` | Where to store dnscrypt config | `/etc/dnscrypt-proxy/dnscrypt-proxy.toml` |
| `dnscrypt_sysd_socket_only` | Use ONLY socket activation of dnscrypt-proxy.service | `yes` |

### Some config-related vars

These are just basic config vars.

For more detailed dnscrypt proxy configuration please study and modify the *dnscrypt-proxy.toml*

| Variable | Description | Default |
|----------|-------------|---------|
| `dnscrypt_ipv4_address` | IPv4 address for `dnscrypt-proxy` to listen on | `127.0.0.1` |
| `dnscrypt_port` | Port for `dnscrypt-proxy` to listen on | `41` |
| `dnscrypt_ipv4_servers` | Use servers reachable over IPv4 | `'true'` |
| `dnscrypt_ipv6_servers` | Use servers reachable over IPv6. Do not enable if you don't have IPv6 set | `'false'` |
| `dnscrypt_dc_servers` | Use servers implementing the DNSCrypt protocol | `'false'` |
| `dnscrypt_doh_servers` | Use servers implementing the DNS-over-HTTPS protocol | `'true'` |
| `dnscrypt_odoh_servers` | Use servers implementing the Oblivious DNS-over-HTTPS protocol | `'false'` |
| `dnscrypt_require_dnssec` | Require upstream resolver to support DNSSEC validation | `'false'` |
| `dnscrypt_require_nolog` | Server must not log user queries (declarative) | `'true'` |
| `dnscrypt_require_nofilter` | Server must NOT enforce its own blacklist (for parental control, ads blocking) | `'true'` |
| `dnscrypt_force_tcp` | Always use TCP to connect to upstream servers. Useful for TOR. | `'false'` |
| `dnscrypt_bootstrap_resolvers` | non-encrypted fallback resolver. Only used if DNS config doesn't work. `114.114.114.114:53` for people in China | `9.9.9.9:53` |
| `dnscrypt_server_names` | List of upstream servers to use. See server_names option in dnscrypt-proxy documentation |
| `dnscrypt_disabled_server_names` | List of upstream servers to disable. See server_names option in dnscrypt-proxy documentation |

Dependencies
----

None.

Example Playbook
----

```yaml
- hosts: raspberrypi
  gather_facts: yes
  roles: mojibake-umd.dnscrypt
```

License
----

[MIT][mit-link]

Author Information
----

Andrew Shagayev | [e-mail](mailto:drewshg@gmail.com)

[role-badge]: https://img.shields.io/badge/role-drew--kun.dnscrypt-green.svg
[galaxy-link]: https://galaxy.ansible.com/drew-kun/dnscrypt/
[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[mit-link]: https://raw.githubusercontent.com/drew-kun/ansible-dnscrypt/master/LICENSE
[minibian-link]: https://minibianpi.wordpress.com/
[pihole-link]: https://pi-hole.net/
[dnscrypt-proxy2-link]: https://github.com/jedisct1/dnscrypt-proxy
[dnscrypt-arch-link]: https://github.com/jedisct1/dnscrypt-proxy/releases/latest
