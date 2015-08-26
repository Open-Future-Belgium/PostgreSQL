Role Name
========

Ansible role which installs and configures PostgreSQL, extensions, databases and users.

Requirements
------------

CentOS/Rhel 6.x & 7.x / Amazon Linux 2015.03

Updated
-------
25/08/2015

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
                       #   N.B. ALL only provides all *database* privileges, but
                       #   no table privileges

# List of object privileges to be applied
postgresql_user_object_privileges:
  - name: john             # user name
    db: foobar             # database
    type: table            # type of db object on which to set privilege
    priv: ALL              # list of privileges (e.g. INSERT,SELECT)
    objs: 'ALL_IN_SCHEMA'  # list of db objects on which to set privilege
  - name: john             # user name
    db: foobar             # database
    type: sequence         # type of db object on which to set privilege
    priv: ALL              # list of privileges (e.g. INSERT,SELECT)
    objs: 'ALL_IN_SCHEMA'  # list of db objects on which to set privilege
```

Kitchen Testing
---------------
Install test-kitchen: gem install test-kitchen
run : kitchen setup all -p -c 4

This will provision 4vm's, 2 with 9.3 and 2 with 9.4 on Centos 6.6 and 7.1




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

Amazon Linux Example Playbook
-----------------------------
```yaml
- hosts: tag_your_tag_name
  remote_user: ec2-user
  sudo: yes
  roles:
    - { role: PostgreSQL,
        postgresql_version: 93,
        postgresql_data_directory: /var/lib/pgsql93/data,
        postgresql_conf_directory: /var/lib/pgsql93/data,
        postgresql_external_pid_file: "/var/run/postgresql/{{postgresql_version}}-{{postgresql_cluster_name}}.pid",
        postgresql_logging_collector: on,
        postgresql_unix_socket_directories: [ /var/run/postgresql ],
        join_char: "" } # This is used to deal with Amazon's naming convention for the postgreSQL service
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
