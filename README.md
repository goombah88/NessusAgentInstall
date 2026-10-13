# install-tenable-agent

Ansible role that deploys and links the Tenable Nessus Agent on **Ubuntu**, **RHEL**, and **Windows** hosts.

## Requirements

- Ansible ≥ 2.12
- Collections (install via `ansible-galaxy collection install -r requirements.yml`):
  - `community.hashi_vault` ≥ 6.0.0 (wraps `hvac` for Vault integration)
  - `ansible.windows` ≥ 2.0.0

## Role Variables

| Variable             | Default                              | Description                                      |
|----------------------|--------------------------------------|--------------------------------------------------|
| `vault_addr`         | `$VAULT_ADDR` env var                | HashiCorp Vault address                          |
| `vault_token`        | `$VAULT_TOKEN` env var               | Vault auth token                                 |
| `vault_nessus_path`  | `cll/data/global/nessus/link_key`    | KV v2 path to the Nessus linking key secret      |
| `vault_nessus_field` | `link_key`                           | Field name within the Vault secret               |
| `nessus_server`      | `sensor.cloud.tenable.com:443`       | Tenable Cloud sensor endpoint                    |
| `nessus_network`     | `Default`                            | Nessus network name                              |
| `nessus_deb_file`    | `nessus-agent-latest.deb`            | Ubuntu installer filename (must exist in files/) |
| `nessus_rpm_file`    | `nessus-agent-latest.rpm`            | RHEL installer filename (must exist in files/)   |
| `nessus_msi_file`    | `nessus-agent-latest.msi`            | Windows installer filename (must exist in files/)|

## Agent Groups (Location-Based)

Agent group is automatically selected based on the target hostname prefix:

| Hostname prefix | Location | Agent Group                           |
|-----------------|----------|---------------------------------------|
| `sjc17*`        | SJC      | `ipam_tis_LRT0001080-agentgroup-01`   |
| `rcd09*`        | RCD      | `ipam_ndcs_LRT0055903-agentgroup-01`  |

The role will fail if the hostname does not match a known prefix.

## Vault Secret

The Nessus linking key is retrieved from HashiCorp Vault using `community.hashi_vault` (hvac):

- **KV v2 mount point**: `cll`
- **Secret path**: `global/nessus/link_key`
- **Full API path**: `/v1/cll/data/global/nessus/link_key`
- **Field**: `link_key`

Ensure `VAULT_ADDR` and `VAULT_TOKEN` environment variables are set before running, or pass them as extra-vars.

## Agent Installer Files

Place the installer binaries in the `files/` directory before running the role.
See [files/README.md](files/README.md) for details.

## Example Playbook

```yaml
- hosts: all
  roles:
    - role: install-tenable-agent
```

## Supported Platforms

- Ubuntu (all versions)
- RHEL 8 / 9
- Windows Server

## GitLab Repository

`https://gitlab.systems.cll.cloud/infrastructure-ops/install-tenable-agent.git`
