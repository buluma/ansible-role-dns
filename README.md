# [Ansible role dns](#ansible-role-dns)

Install and configure dns on your system.

|GitHub|GitLab|Downloads|Version|
|------|------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-dns/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-dns/actions)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-dns/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-dns)|[![downloads](https://img.shields.io/ansible/role/d/buluma/dns)](https://galaxy.ansible.com/buluma/dns)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-dns.svg)](https://github.com/buluma/ansible-role-dns/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-dns/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- become: true
  gather_facts: true
  hosts: all
  name: Converge
  roles:
  - dns_port: 5353
    role: buluma.dns
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-dns/blob/master/molecule/default/prepare.yml):

```yaml
---
- become: true
  gather_facts: false
  hosts: all
  name: Prepare
  roles:
  - role: buluma.bootstrap
  - role: buluma.core_dependencies
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-dns/blob/master/defaults/main.yml):

```yaml
---
dns_allow_recursion:
- none
dns_caching_dns: true
dns_options_listen_on:
- any
dns_options_listen_on_v6:
- any
dns_pid_file: /run/named/named.pid
dns_port: 53
dns_zones:
- expire: 2419200
  name: localhost
  records:
  - name: "@"
    type: NS
    value: localhost.
  - name: "@"
    value: 127.0.0.1
  - name: "@"
    type: AAAA
    value: ::1
  refresh: 604800
  retry: 86400
  serial: 1
  soa: localhost
  ttl: 604800
- name: 127.in-addr.arpa
  records:
  - name: "@"
    type: NS
    value: localhost.
  - name: 1.0.0
    type: PTR
    value: localhost.
  ttl: 604800
- name: 0.in-addr.arpa
  records:
  - name: "@"
    type: NS
    value: localhost.
- name: 255.in-addr.arpa
  records:
  - name: "@"
    type: NS
    value: localhost.
- mx:
  - name: mail1.example.com.
    priority: 10
  - name: mail2.example.com.
    priority: 20
  name: example.com
  ns:
  - name: dns1.example.com.
  - name: dns2.example.com.
  records:
  - name: dns1
    value: 127.0.0.1
  - name: dns2
    value: 127.0.0.1
  - name: www
    value: 127.0.0.1
  - name: dns1
    value: 127.0.0.1
  - name: dns2
    value: 127.0.0.1
  - name: mail1
    value: 127.0.0.1
  - name: mail2
    value: 127.0.0.1
  ttl: 604800
- dns_zone_forwarders:
  - 1.1.1.1
  - 8.8.8.8
  name: forwarded.example.com
  type: forward
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-dns/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Build Status GitHub](https://github.com/buluma/ansible-role-core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-core_dependencies/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-core_dependencies)|

## [Context](#context)

This role is part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-dns/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Alpine](https://hub.docker.com/r/buluma/alpine)|all|
|[Amazon](https://hub.docker.com/r/buluma/amazonlinux)|all|
|[EL](https://hub.docker.com/r/buluma/enterpriselinux)|all|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|

The minimum version of Ansible required is 2.12, tests have been done on:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them on [GitHub](https://github.com/buluma/ansible-role-dns/issues).

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-dns/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)

