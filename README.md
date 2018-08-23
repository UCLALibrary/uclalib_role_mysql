uclalib_role_mysql
=========

Ansible role that installs MySQL 5.6 or 5.7 on a RHEL7 host

This role can also configure MySQL databases and users

Requirements
------------

Please take note of the following assumptions:
* the MySQL server uses Red Hat Enterprise Linux 7 as the OS
* project-specific variables for this role can be defined in a __vars__ file with a name following the format of `projectname_envname.yml`
    * an example vars file is available in `vars/exampleproj_test.yml`
    * this vars file will contain sensitive information and should be encrypted with ansible-vault
    * NOTE: if you choose not to use the vars file for including the variable definitions, they should be defined in the playbook file 

Role Variables
--------------

Variables that need to be defined in the **play file** or the **host inventory file** - please note these should match the naming used for the vars file:
* `project_name` - defines the name of the the project that requires one or more MySQL databases and users - there is no default value
* `env_name` - defines the name of the deploy environment (e.g. test, stage, prod) - there is no default value
* `mysql_install_version` - defines the version of MySQL to use (this role only supports installing 5.6 or 5.7) - default is 5.6

Variables that **do** need to be defined in the project vars file:
* `mysql_databases` - this is the list of databases to create
  * `name` - defines the name of the database to create
  * `collation` - defines the type of collation scheme (default is: `utf8_bin`)
  * `encoding` - defines the type of text encoding to use (default is: `utf8`)


* `mysql_users` - this is the list of users to create
  * `name` - defines the username
  * `host` - defines the host where the user is allowed to connect from
  * `password` - defines the password for this user
  * `priv`: - defines the database privileges for this user (syntax should be `dbname`.`*`:PRIV - e.g. `exampledb.*:ALL`)

Dependencies
------------

None.

Example Playbook
----------------

```
---

- name: uclalib_mysqltest.yml
  become: yes
  become_method: sudo
  hosts: all
  user: ansible
  vars:
    project_name: exampleproj
    env_name: test

  roles:
    - { role: uclalib_role_rhel7repos }
    - { role: uclalib_role_epel }
    - { role: uclalib_role_mysql, mysql_install_version: '5.6' }
```
