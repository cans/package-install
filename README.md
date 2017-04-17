cans.package-install
====================

Simple Ansible Role that installs a given list of Debian packages
(`.deb`).

The intent of this role is to come with all the bells and whistles
but to provide basic, but efficient and reusable, package installation
method. It is basically a function that given a list of packages,
installs them.


Requirements
------------

This role has no particular pre-requisite.


Role Variables
--------------

- `pkg_install_cache_ttl`: package cache validity duration, in seconds
  (default: 3600)
- `pkg_install_packages`: the list of packages to install (default:
  `[]`)
- `pkg_install_recommanded`: whether to install *recommanded* packages
  alongside the packages explicitly listed for installation (default:
  no).
- `pkg_install_update_cache`: whether to update the package cache or not
  before installing packages (default: yes)


Dependencies
------------

This role has no formal dependencies. But i


Example Playbook
----------------

Assume you have two roles, `first_role` and `second_roles`, each of
which defines a variable that contains a list of packages to install,
let say `first_role_packages` and `second_role_packages`, respectively.
You can then to use this role, start by assiging the
`pkg_install_packages` variable with the concatenation of both list.
Then just call the role.

    - hosts: servers
      vars:
        pkg_install_packages: "{{ first_role_packages + second_role_packages }}"
      roles:
        - { role: cans.package-install }
        - { role: first_role }
        - { role: second_role }


License
-------

GPLv2


Author Information
------------------

Copyright © Nicolas CANIART <nicolas@caniart.net>
