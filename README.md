Role Name
========

Ansible role which installs and configures PostgreSQL, extensions, databases and users.

Requirements
------------

CentOS 6.x / RedHat 6.x


Role Variables
--------------
```yaml
# Basic settings
postgresql_version: 9.3
postgresql_encoding: 'UTF-8'
postgresql_locale: 'en_US.UTF-8'

postgresql_admin_user: "postgres"
postgresql_default_auth_method: "trust"

postgresql_cluster_name: "main"
postgresql_cluster_reset: false

# List of databases to be created (optional)
postgresql_databases:
  - name: foobar
    hstore: yes         # flag to install the hstore extensions on this database (yes/no)

# List of users to be created (optional)
postgresql_users:
  - name: john
    pass: pass
    encrypted: no       # denotes if the password is already encrypted.

# List of user privileges to be applied (optional)
postgresql_user_privileges:
  - name: john         # user name
    db: foobar         # database
    priv: "ALL"        # privilege string format: example: INSERT,UPDATE/table:SELECT/anothertable:ALL
```

Dependencies
------------

No other roles are needed.

Example Playbook
-------------------------
```yaml
# file: localrepo.yml
- hosts: vagrant
  user: vagrant
  sudo: yes
  sudo_user: root
  roles:
    - PostgreSQL
```

License
-------

MIT

Author Information
------------------

* Patrik Uytterhoeven
* patrik( at )open-future.be
* [www.open-future.be](http://www.open-future.be)
* Role based on Galaxy Ansibles.postgresql role for Debian 
