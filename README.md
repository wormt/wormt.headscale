# wormt.headscale

`wormt.headscale` configures a Headscale server in a rootless Podman container. The role renders the Headscale configuration, sets SELinux file contexts under `/opt/podman`, and installs a systemd quadlet through `fedora.linux_system_roles.podman`.

## Requirements

The role targets Fedora. The host must have Podman, `crun`, `systemd-container`, and `policycoreutils-python-utils` installed. The role expects a `podman` user and group with `/opt/podman` as its home directory.

Install the required Ansible collections with:

```bash
ansible-galaxy collection install -r requirements.yml
```

## Usage

```yaml
- name: Configure Headscale
  hosts: headscale
  roles:
    - role: wormt.headscale
```

Set role variables in inventory or group variables. The main settings are:

| Variable | Default | Purpose |
| --- | --- | --- |
| `__headscale_server_url` | `https://head.example.com:443` | Public Headscale URL |
| `__headscale_acme_email` | `wormt@example.com` | Email used for ACME registration |
| `__headscale_base_domain` | `example.com` | MagicDNS base domain |
| `__headscale_derp_ipv4` | empty | Public IPv4 address for the embedded DERP server |
| `__headscale_derp_ipv6` | empty | Public IPv6 address for the embedded DERP server |
| `__headscale_dns_nameservers` | Quad9 IPv4 and IPv6 addresses | DNS resolvers sent to clients |
| `__headscale_dns_extra_records` | `[]` | Static MagicDNS records |
| `__headscale_override_local_dns` | `true` | `dns.override_local_dns`: `true` makes tailnet resolver replace the host's. `false` keeps MagicDNS scoped to the base domain |
| `__headscale_acl_policy` | `{ "acls": [] }` | ACL policy written to `acl.json` |
| `__headscale_config_dest` | `/opt/podman/headscale/config/config.yaml` | Rendered configuration path |

The default ACL contains no allow rules, so clients cannot communicate until you provide a policy. Replace the example hostnames and ACME email address before deployment.
