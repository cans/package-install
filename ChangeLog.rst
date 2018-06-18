Change Log
==========

This log tracks all notable changes made to the ``cans.package-install`` role
for Ansible.

The format is based on `Keep a Changelog <http://keepachangelog.com/en/1.0.0/>`_
and this project adheres to `Semantic Versioning <http://semver.org/spec/v2.0.0.html>`_.


Unreleased
----------

Added:
~~~~~~

* Added ability to purge package manager cache after installing
  packages. When using this role to build container images, that can
  drastically reduce image size. Adds some variable to perform
  that tasks (_cf._ documentation for details):

  - ``pkginstall_cache_purge``.
  - ``pkginstall_apt_package_list_cache_directory``.

* Added ability to remove/purge packages. Introduces variables (_cf._
  documentation for details):

  - ``pkginstall_packages_absent``.
  - ``pkginstall_purge``.


Changed:
~~~~~~~~

* Deprecated variable ``pkginstall_update_cache`` in favor of
  ``pkginstall_cache_update``;
* Deprecated variable ``pkginsall_packages`` in favor of
  ``pkginstall_packages_present``;


Version v2.0.0 -- 2018-02-17
----------------------------


Changed:
~~~~~~~~

* Removes ``become`` embedded within the role tasks [BREAK];


Version v1.0.0 -- 2017-09-10
----------------------------


Added:
~~~~~~

* initial release, includes basic functionalities;
