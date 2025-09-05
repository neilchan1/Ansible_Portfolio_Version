# Ansible (Portfolio Version)

This repository showcases Ansible Playbooks for both `Linux` and `Windows` systems,
executed via a **Jenkins** build (Can also be ran manaully).

## Disclaimer
As part of this showcase, the playbooks are intentionally **minimal, commented, and safe** (no secrets, no real hosts).

## Key Design Decisions
- **Roles for reuse** (users, packages, services, security, monitoring).
- **Small utility playbooks** for focused actions (start/stop services, download/extract files, manage users).
- **Jenkins integration** using build parameters passed as environment variables.
- **Cross-platform** using `ansible_os_family` to perform tasks on the required OS.

---

## Repo Layout
```
playbooks/           # Runnable, modular playbooks (Jenkins-ready)
roles/               # Reusable roles, with cross-platform (Linux/Windows) patterns
group_vars/          # Global/default variables (optional, demonstrative)
inventory/           # Example for static inventories with dummy hosts
Jenkinsfile          # Example CI/CD pipeline that calls these playbooks
ansible.cfg          # Sensible defaults for a smooth demo
```

---

## Quick Start (Local or Jenkins)

> All playbooks require variables to be passed **via environment variables or `-e` extra-vars**.

### Example: Running a single playbook locally
```bash
# Download File
ansible-playbook -i inventory/dev.yml playbooks/download_file.yml \
  -e "DOWNLOAD_URL=https://example.com/file.tar.gz DOWNLOAD_DEST=/tmp/file.tar.gz"

# Extract Package
ansible-playbook -i inventory/dev.yml playbooks/extract_file.yml \
  -e "ARCHIVE_PATH=/tmp/file.tar.gz EXTRACT_DEST=/opt/demoapp"

# Start Service
ansible-playbook -i inventory/dev.yml playbooks/start_service.yml \
  -e "SERVICE_NAME=nginx"

# Manage Users
ansible-playbook -i inventory/dev.yml playbooks/manage_users.yml \
  -e 'USERS=[{"name":"jenkins","groups":"devops"}]'
```

### Example: Running a single playbook locally
```bash
ansible-playbook -i inventory/dev.yml playbooks/site.yml \
  -e "SERVICE_NAME=nginx \
      DOWNLOAD_URL=https://example.com/file.tar.gz \
      DOWNLOAD_DEST=/tmp/file.tar.gz \
      ARCHIVE_PATH=/tmp/file.tar.gz \
      EXTRACT_DEST=/opt/demoapp \
      USERS=[{\"name\":\"jenkins\",\"groups\":\"devops\"}]"
```

## Jenkins Usage
- The Jenkinsfile defines parameters (`SERVICE_NAME`, `DOWNLOAD_URL`, `DOWNLOAD_DEST`, `ARCHIVE_PATH`, `EXTRACT_DEST`, `USERS`).
- Each playbook is modular; Jenkins can call them independently, or run `site.yml` for full orchestration.
- Stages demonstrate calling each utility playbook independently.

---

## Notes for Reviewers
- Playbooks are designed to be **re-usable** where possible and contain **comments** explaining reasoning.
- Tasks demonstrate **handlers**, **conditionals**, **loops**, and **OS friendly** functions.
- Static inventories are used in this showcase and are intentionally **minimal**. In a production setup we would add `group-specific vars`, `dynamic inventories`, etc.
- Fully parameterized for Jenkins-driven automation.
