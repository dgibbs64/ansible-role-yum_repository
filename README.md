# yum_repository

An [Ansible](https://www.ansible.com) role that manages yum/dnf repositories on RHEL-based systems.

<p align="center">
<a href="https://app.codacy.com/gh/dgibbs64/ansible-role-yum_repository"><img src="https://img.shields.io/codacy/grade/1a892d499efd4dabb73beffa8d64ed01?logo=codacy&style=flat-square" alt="Codacy grade"></a>
<a href="https://github.com/dgibbs64/ansible-role-yum_repository/actions/workflows/molecule.yml"><img alt="GitHub Workflow Status" src="https://img.shields.io/github/actions/workflow/status/dgibbs64/ansible-role-yum_repository/molecule.yml?label=molecule&logo=ansible&style=flat-square"></a>
<a href="https://galaxy.ansible.com/dgibbs64/yum_repository"><img alt="GitHub tag (latest by date)" src="https://img.shields.io/github/v/tag/dgibbs64/ansible-role-yum_repository?color=EE0000&label=release&logo=ansible&style=flat-square"></a>
<a href="/LICENSE.md"><img src="https://img.shields.io/github/license/dgibbs64/ansible-role-yum_repository?style=flat-square" alt="MIT License"></a>
</p>

## About

This role manages yum/dnf repositories on RHEL-based systems. It supports:

- Adding repositories with full configuration options
- Removing repositories by setting `state: absent`
- Removing specific legacy or unmanaged `.repo` files by name
- Purging all unmanaged `.repo` files from `/etc/yum.repos.d/`

Each repository entry maps directly to the [`ansible.builtin.yum_repository`](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/yum_repository_module.html) module parameters.

## Requirements

None.

## Role Variables

```yaml
---
# List of yum repositories to manage.
# Each entry maps directly to ansible.builtin.yum_repository parameters.
# Set state: absent to remove a repository.
yum_repositories: []

# List of .repo filenames to explicitly remove from /etc/yum.repos.d/.
yum_repository_files_absent: []

# When true, removes all .repo files in /etc/yum.repos.d/ not managed by this role.
yum_repositories_purge: false
```

## Example Playbook

### Add a repository

```yaml
---
- name: Yum Repository
  hosts: all
  vars:
    yum_repositories:
      - name: epel
        description: Extra Packages for Enterprise Linux
        baseurl: https://download.fedoraproject.org/pub/epel/$releasever/$basearch/
        gpgcheck: true
        gpgkey: https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-$releasever
        enabled: true
        state: present
  roles:
    - role: "dgibbs64.yum_repository"
```

### Remove specific legacy files

```yaml
---
- name: Yum Repository
  hosts: all
  vars:
    yum_repository_files_absent:
      - local.repo
      - CentOS-Base.repo
      - CentOS-AppStream.repo
    yum_repositories:
      - name: epel
        description: Extra Packages for Enterprise Linux
        baseurl: https://download.fedoraproject.org/pub/epel/$releasever/$basearch/
        gpgcheck: true
        gpgkey: https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-$releasever
        enabled: true
  roles:
    - role: "dgibbs64.yum_repository"
```

### Purge all unmanaged repos

```yaml
---
- name: Yum Repository
  hosts: all
  vars:
    yum_repositories_purge: true
    yum_repositories:
      - name: epel
        description: Extra Packages for Enterprise Linux
        baseurl: https://download.fedoraproject.org/pub/epel/$releasever/$basearch/
        gpgcheck: true
        gpgkey: https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-$releasever
        enabled: true
  roles:
    - role: "dgibbs64.yum_repository"
```

### Remove a repository

```yaml
---
- name: Yum Repository
  hosts: all
  vars:
    yum_repositories:
      - name: old-repo
        state: absent
  roles:
    - role: "dgibbs64.yum_repository"
```

## Dependencies

None.

## License

MIT

## Author Information

[Daniel Gibbs](https://danielgibbs.co.uk)
