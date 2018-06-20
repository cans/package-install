cans.package-install
====================

[![Build Status](https://img.shields.io/travis/marvinpinto/ansible-role-docker/master.svg?style=flat-square)](https://travis-ci.org/cans/package-install)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-cans.package--install-blue.svg?style=flat-square)](https://galaxy.ansible.com/cans/package-install)
[![License](https://img.shields.io/badge/license-GPLv2-brightgreen.svg?style=flat-square)](LICENSE)

Simple Ansible Role that installs a given list of Debian packages
(`.deb`).


The intent of this role is not to come with all the bells and whistles.
It is more a minimalistic, but efficient and reusable, package installation
method. It is basically a procedure that, when given a list of packages,
installs them.


Requirements
------------

This role has no particular pre-requisite. But is assumes the target
server(s) use the [Debian distribution](https://www.debian.org) or a
derivative.


Role Variables
--------------

All variables in this role are namespaced using the `pkginstall_` prefix.

This role also defines variables for its internal use. Those are prefixed
with `_pkginstall_`. You should not use those variables.

### Input variables

For this roles to perform any task, you *must* define either or both of the
`pkginstall_packages_absent` and `pkginstall_packages_present variables.

- `pkginstall_packages`: **DEPRECATED** Use `pkginstall_package_present`
  instead (default: `[]`);
- `pkginstall_packages_present`: a list of package names to ensure are
  installed on the target host(s) (default: `[]`);
- `pkginstall_packages_absent`: a list of package names to ensure are *not*
  installed on the target host(s) (default: `[]`);


### Defaults

- `pkginstall_apt_package_list_cache_directory`: path to the directory that
  stores available package lists and packages content lists. This variable
  is only required if you set `pkginstall_cache_purge` to `true` (see below).
  It is very unlikely you would ever need to change this. (default:
  `"/var/lib/apt/lists"`)
- `pkginstall_cache_ttl`: package cache validity duration, in seconds
  (default: 3600)
- `pkginstall_cache_update`: whether to update the package cache or not
  before installing packages (default: `true`)
- `pkginstall_purge`: when removing packages, also remove their configuration
  files (default: `true`);
- `pkginstall_recommended`: whether to install *recommended* packages
  alongside the packages explicitly listed for installation (default:
  `false`).
- `pkginstall_update_cache`: **DEPRECATED** use `pkginstall_cache_update`
  instead (default: `true`)



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
    - role: cans.package-install
    - role: first_role
    - role: second_role
```

If for some reason you _cannot_ install all the packages at once (_e.g._
because `first_role` installs or configures somethings required before being
able to install the packages required by `second_role`:

```yaml
- hosts: servers
  roles:
    - role: cans.package-install
      pkginstall_packages: "{{ first_role_packages }}"
    - role: first_role
    - role: cans.package-install
      pkginstall_packages: "{{ second_role_packages }}"
    - role: second_role
```

The above examples assume Ansible connects to the target servers with an
identity that has sufficient privileges to install packages. If not you
may need to use either or both of the `remote_user` and `become` keywords:

```yaml
- hosts: servers
  remote_user: "privileged-user"
  vars:
    pkginstall_packages: "{{ first_role_packages + second_role_packages }}"
  roles:
    - role: cans.package-install
      become: yes
    - role: first_role
    - role: second_role
```


License
-------

GPLv2


Author Information
------------------

Copyright © 2017-2018, Nicolas CANIART.
