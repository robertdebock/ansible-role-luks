# [luks](#luks)

Manage encrypted devices using luks.

|GitHub|GitLab|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![github](https://github.com/robertdebock/ansible-role-luks/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-luks/actions)|[![gitlab](https://gitlab.com/robertdebock/ansible-role-luks/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-luks)|[![quality](https://img.shields.io/ansible/quality/)](https://galaxy.ansible.com/robertdebock/luks)|[![downloads](https://img.shields.io/ansible/role/d/)](https://galaxy.ansible.com/robertdebock/luks)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-luks.svg)](https://github.com/robertdebock/ansible-role-luks/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.luks
      luks_devices:
        - device: /dev/loop0
          name: /dev/luks-disk
          keyfile: /etc/luks_keyfile
```

The machine needs to be prepared in CI this is done using `molecule/resources/prepare.yml`:
```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap

  tasks:
    - name: place keyfile
      copy:
        content: "C0mpl3x1t7"
        dest: /etc/luks_keyfile
        owner: root
        group: root
        mode: "0400"

    - name: make disk.img
      command: truncate -s 10M /disk.img
      args:
        creates: /disk.img
      notify:
        - losetup

    - name: make /dev/loop0
      command: mknod -m 0660 /dev/loop0 b 7 8
      args:
        creates: /dev/loop0

    - name: remove /dev/loop0
      command: losetup -d /dev/loop0
      args:
        removes: /dev/loop0

  handlers:
    - name: losetup
      command: losetup -P /dev/loop0 /disk.img
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for luks

# luks_devices:
#   - name: /dev/luks-sda1
#     device: /dev/sda1
#     passphrase: "C0mpl3x1t7"
#   - name: /dev/luks-sda2
#     device: /dev/sda2
#     keyfile: /etc/luks_keyfile
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/robertdebock/ansible-role-luks/blob/master/requirements.txt).

## [Status of requirements](#status-of-requirements)

The following roles are used to prepare a system. You may choose to prepare your system in another way, I have tested these roles as well.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
| [robertdebock.bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bootstrap/actions) | [![Build Status GitLab ](https://gitlab.com/robertdebock/ansible-role-ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-bootstrap)

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/ansible-role-luks/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|alpine|all|
|amazon|Candidate|
|el|7, 8|
|debian|all|
|fedora|all|
|opensuse|all|
|ubuntu|focal, bionic|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.



If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-luks/issues)

## [License](#license)

Apache-2.0


## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
