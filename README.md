Ansible Role: Logrotate
===================

![GitHub](https://img.shields.io/github/license/nycrecords/ansible-role-logrotate)
[![Build Status](https://travis-ci.com/nycrecords/ansible-role-logrotate.svg?branch=master)](https://travis-ci.com/nycrecords/ansible-role-logrotate)
[![Galaxy](https://img.shields.io/badge/galaxy-nycrecords.logrotate-blue.svg)](https://galaxy.ansible.com/nycrecords/logrotate)
![Ansible](https://img.shields.io/ansible/role/d/45553)
![Ansible](https://img.shields.io/ansible/quality/45553)

Installs logrotate and provides an easy way to setup additional logrotate scripts by
specifying a list of directives.

Requirements
------------

Ansible 2.4 or higher

Role Variables
--------------

**logrotate_scripts**: A list of logrotate scripts and the directives to use for the rotation.

* name - The name of the script that goes into /etc/logrotate.d/
* path - Path to point logrotate to for the log rotation
* paths - A list of paths to point logrotate to for the log rotation.
* options - List of directives for logrotate, view the logrotate man page for specifics
* scripts - Dict of scripts for logrotate (see Example below)

```yaml
logrotate_scripts:
  - name: rails
    path: "/srv/current/log/*.log"
    options:
      - weekly
      - size 25M
      - missingok
      - compress
      - delaycompress
      - copytruncate
```

```yaml
logrotate_scripts:
  - name: rails
    paths:
        - "/srv/current/scare.log"
        - "/srv/current/hide.log"
    options:
      - weekly
      - size 25M
      - missingok
      - compress
      - delaycompress
      - copytruncate
```

Dependencies
------------

None

Example Playbook
----------------

Setting up logrotate for additional Nginx logs, with postrotate script.

```yaml
- hosts: all
  vars:
    logrotate_scripts:
      - name: nginx-options
        path: /var/log/nginx/options.log
        options:
          - daily
          - weekly
          - size 25M
          - rotate 7
          - missingok
          - compress
          - delaycompress
          - copytruncate

      - name: nginx-scripts
        path: /var/log/nginx/scripts.log
        options:
          - daily
          - weekly
          - size 25M
        scripts:
          postrotate: "echo test"

  roles:
    - ansible-logrotate
```

Testing locally
---------------

This role is already configured to run on travis CI within a test playbook but it's useful to be able to run and debug a role locally.

To run the test playbook locally within a Vagrant virtual machine:

```shell
pipenv install --dev
pipenv run molecule test -s default
```

License
-------

[BSD](https://raw.githubusercontent.com/nickhammond/logrotate/master/LICENSE)

Author Information
------------------

* [nickhammond](https://github.com/nickhammond) | [www](http://www.nickhammond.com) | [twitter](http://twitter.com/nickhammond)
* [bigjust](https://github.com/bigjust)
* [steenzout](https://github.com/steenzout)
* [jeancornic](https://github.com/jeancornic)
* [duhast](https://github.com/duhast)
* [kagux](https://github.com/kagux)
* [joelbcastillo](https://github.com/joelbcastillo)
