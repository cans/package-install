cans.package-install
====================

[![Build Status](https://img.shields.io/travis/marvinpinto/ansible-role-docker/master.svg?style=flat-square)](https://travis-ci.org/cans/package-install)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-cans.package--install-blue.svg?style=flat-square)](https://galaxy.ansible.com/cans/package-install)
[![License](https://img.shields.io/badge/license-GPLv2-brightgreen.svg?style=flat-square)](LICENSE)

Simple Ansible Role that installs a given list of Debian packages
(`.deb`).


The intent of this role is not to come with all the bells and whistles.
It is more a minimalistic, but efficient and reusable, package installation
method. It is basically a function that given a list of packages,
installs them.


Requirements
------------

This role has no particular pre-requisite.


Role Variables
--------------

All variables in this role are namespaced using the `pkginstall_` prefix.

- `pkginstall_cache_ttl`: package cache validity duration, in seconds
  (default: 3600)
- `pkginstall_packages`: the list of packages to install (default:
  `[]`)
- `pkginstall_recommended`: whether to install *recommended* packages
  alongside the packages explicitly listed for installation (default:
  `false`).
- `pkginstall_update_cache`: whether to update the package cache or not
  before installing packages (default: `true`)


Dependencies
------------

This role has no formal dependencies. But it requires from you to
override the `pkginstall_packages` variable to actually perform
anything (_cf._ example playbook below).

You may also be interested in using the `cans.package-source` role in
conjunction with this one, to add extra package repositories to those
already known by APT.


Example Playbooks
-----------------

Assume you have two roles, `first_role` and `second_roles`, each of
which defines a variable that contains a list of packages to install,
let say `first_role_packages` and `second_role_packages`, respectively.
You can then, to use this role, start by assigning the
`pkg_install_packages` variable with the concatenation of both list.
Then just call the role.

```yaml
- hosts: servers
  vars:
    pkginstall_packages: "{{ first_role_packages + second_role_packages }}"
  roles:
    - { role: cans.package-install }
    - { role: first_role }
    - { role: second_role }
```

If for some reason you _cannot_ install all the packages at once (_e.g._
because `first_role` installs or configures somethings required before being
able to install the packages required by `second_role`:

```yaml
- hosts: servers
  roles:
    - role: cans.package-install
      pkginstall_packages: "{{ first_role_packages }}"
    - { role: first_role }
    - role: cans.package-install
      pkginstall_packages: "{{ second_role_packages }}"
    - { role: second_role }
```


License
-------

GPLv2


Author Information
------------------

Copyright © 2017, Nicolas CANIART.
