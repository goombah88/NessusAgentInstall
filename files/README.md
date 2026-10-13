# Agent Installer Files

Place the Nessus Agent installer binaries in this directory before running the role.
Filenames must match the variables defined in `defaults/main.yml` (overridable via extra-vars):

| Variable           | Default filename           | OS      |
|--------------------|----------------------------|---------|
| `nessus_deb_file`  | `nessus-agent-latest.deb`  | Ubuntu  |
| `nessus_rpm_file`  | `nessus-agent-latest.rpm`  | RHEL    |
| `nessus_msi_file`  | `nessus-agent-latest.msi`  | Windows |

Download the appropriate installer from the Tenable downloads portal and drop it here.
These binaries are excluded from version control via `.gitignore`.
