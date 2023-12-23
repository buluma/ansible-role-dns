# [Ansible role dns](#dns)

Install and configure dns on your system.

|GitHub|Version|Issues|Pull Requests|
|------|-------|------|-------------|
|[![github](https://github.com/buluma/ansible-role-dns/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-dns/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-dns.svg)](https://github.com/buluma/ansible-role-dns/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-dns.svg)](https://github.com/buluma/ansible-role-dns/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-dns.svg)](https://github.com/buluma/ansible-role-dns/pulls/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-dns/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: buluma.dns
      dns_port: 5353
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-dns/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: buluma.bootstrap
    - role: buluma.core_dependencies
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-dns/blob/master/defaults/main.yml):

```yaml
---
# defaults file for dns

# The port to listen on.
dns_port: 53

# Should the DNS server be a caching DNS server?
dns_caching_dns: yes

# A list of zones and properties per zone.
dns_zones:
  - name: localhost
    soa: localhost
    serial: 1
    refresh: 604800
    retry: 86400
    expire: 2419200
    ttl: 604800
    records:
      - name: "@"
        type: NS
        value: localhost.
      - name: "@"
        value: "127.0.0.1"
      - name: "@"
        type: AAAA
        value: "::1"

  - name: "127.in-addr.arpa"
    ttl: 604800
    records:
      - name: "@"
        type: NS
        value: localhost.
      - name: "1.0.0"
        type: PTR
        value: localhost.

  - name: "0.in-addr.arpa"
    records:
      - name: "@"
        type: NS
        value: localhost.

  - name: "255.in-addr.arpa"
    records:
      - name: "@"
        type: NS
        value: localhost.

  - name: example.com
    ttl: 604800
    ns:
      - name: dns1.example.com.
      - name: dns2.example.com.
    mx:
      - name: mail1.example.com.
        priority: 10
      - name: mail2.example.com.
        priority: 20
    records:
      - name: dns1
        value: "127.0.0.1"
      - name: dns2
        value: "127.0.0.1"
      - name: www
        value: "127.0.0.1"
      - name: dns1
        value: "127.0.0.1"
      - name: dns2
        value: "127.0.0.1"
      - name: mail1
        value: "127.0.0.1"
      - name: mail2
        value: "127.0.0.1"

  - name: forwarded.example.com
    type: forward
    dns_zone_forwarders:
      - "1.1.1.1"
      - "8.8.8.8"

# An optional list of acls to allow recursion. ("any" and "none" are always available.)
dns_allow_recursion:
  - none

# An optional list of IPv4 on which the DNS server will listen. ("any" and "none" are always available.)
dns_options_listen_on:
  - any

# A optional list of IPv6 on which the DNS server will listen. ("any" and "none" are always available.)
dns_options_listen_on_v6:
  - any

# An optional list of IP which are allowed to query the server. ("any" and "none" are always available.)
# Default: "any"
# dns_options_allow_query:
#  - any
#  - "127.0.0.1"

# An optional list of IP which are allowed to run a AXFR query. ("any" and "none" are always available.)
# Default: "none"
# dns_options_allow_transfer:
#   - none
#   - "172.16.0.1"

# An optional setting to configure the path where the pid file will be created.
dns_pid_file: /run/named/named.pid

# An optional setting to forward traffic to other DNS servers.
# dns_options_forwarders:
#   - "1.1.1.1"
#   - "8.8.8.8"

# Another example thanks to @blaisep.
# dns_zones:
#   - name: lab.controlplane.info
#     ttl: 600
#     ns:
#       - name: ns.lab.controlplane.info.
#     mx:
#       - name: mail1.lab.controlplane.info.
#         priority: 10
#       - name: mail2.lab.controlplane.info.
#         priority: 20
#     records:
#       - name: ns
#         value: "192.168.254.27"
#       - name: git
#         value: "192.168.254.19"
#       - name: dl380
#         value: "192.168.254.27"
#       - name: mail1
#         value: "192.168.123.123"
#       - name: mail2
#         value: "192.168.123.123"
#   - name: forwarded.lab.controlplane.info
#     ns:
#       - name: forwarded.lab.controlplane.info.
#     records:
#       - name: ns
#         value: "192.168.254.27"
#       - name: "@"
#         value: "192.168.123.123"
#     dns_zone_forwarders:
#       - "9.9.9.9"
#       - "8.8.8.8"
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-dns/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Build Status GitHub](https://github.com/buluma/ansible-role-core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-core_dependencies.svg)](https://github.com/shadowwalker/ansible-role-core_dependencies)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-dns/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Alpine](https://hub.docker.com/repository/docker/buluma/alpine/general)|all|
|[Amazon](https://hub.docker.com/repository/docker/buluma/amazonlinux/general)|Candidate|
|[EL](https://hub.docker.com/repository/docker/buluma/enterpriselinux/general)|8|
|[Debian](https://hub.docker.com/repository/docker/buluma/debian/general)|all|
|[Fedora](https://hub.docker.com/repository/docker/buluma/fedora/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-dns/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-dns/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-dns/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)


### [Special Thanks](#special-thanks)

Template inspired by [Robert de Bock](https://github.com/robertdebock)
