# [openvpn](#openvpn)

Install and configure openvpn server or client on your system.

|Travis|GitHub|GitLab|Quality|Downloads|Version|
|------|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-openvpn.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-openvpn)|[![github](https://github.com/robertdebock/ansible-role-openvpn/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-openvpn/actions)|[![gitlab](https://gitlab.com/robertdebock/ansible-role-openvpn/badges/master/pipeline.svg)](https://gitlab.com/robertdebock/ansible-role-openvpn)|[![quality](https://img.shields.io/ansible/quality/37845)](https://galaxy.ansible.com/robertdebock/openvpn)|[![downloads](https://img.shields.io/ansible/role/d/37845)](https://galaxy.ansible.com/robertdebock/openvpn)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-openvpn.svg)](https://github.com/robertdebock/ansible-role-openvpn/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: create openvpn server
      include_role:
        name: robertdebock.openvpn
      vars:
        openvpn_role: "server"

    - name: copy certificates and keys from the server to the client
      copy:
        src: /etc/openvpn/easy-rsa/pki/{{ item }}
        dest: /etc/openvpn/client/{{ item | basename }}
        remote_src: yes
      loop:
        - ca.crt
        - issued/client.crt
        - private/client.key
        - ta.key

    - name: create openvpn client
      include_role:
        name: robertdebock.openvpn
      vars:
        openvpn_role: "client"
        openvpn_client_server: 127.0.0.1
```

The machine needs to be prepared in CI this is done using `molecule/resources/prepare.yml`:
```yaml
---
- name: Prepare server
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: robertdebock.bootstrap
    # - role: robertdebock.buildtools
    - role: robertdebock.epel
    # - role: robertdebock.python_pip
    # - role: robertdebock.openssl
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for openvpn

# You can setup both a client and a server using this role.
# Use `server` or `client` for `openvpn_role`.

openvpn_role: server

# If you are configuring a client, setup these variables:
# openvpn_role: client
# openvpn_client_server: vpn.example.com
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/robertdebock/ansible-role-openvpn/blob/master/requirements.txt).

## [Status of requirements](#status-of-requirements)

The following roles are used to prepare a system. You may choose to prepare your system in another way, I have tested these roles as well.

| Requirement | Travis | GitHub |
|-------------|--------|--------|
| [robertdebock.bootstrap](https://galaxy.ansible.com/robertdebock/bootstrap) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-bootstrap.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-bootstrap) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-bootstrap/actions) |
| [robertdebock.epel](https://galaxy.ansible.com/robertdebock/epel) | [![Build Status Travis](https://travis-ci.com/robertdebock/ansible-role-epel.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-epel) | [![Build Status GitHub](https://github.com/robertdebock/ansible-role-epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-epel/actions) |

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/openvpn.png "Dependency")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|amazon|Candidate|
|debian|buster, bullseye|
|el|7, 8|
|fedora|all|
|debian|buster, bullseye|
|ubuntu|focal, bionic|

The minimum version of Ansible required is 2.9, tests have been done to:

- The previous version.
- The current version.
- The development version.



## [Testing](#testing)

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-openvpn) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-openvpn/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## [License](#license)

Apache-2.0


## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
