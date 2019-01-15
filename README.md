lighttpd
========

This role installs and configures [lighttpd](http://www.lighttpd.net/).

Requirements
------------

No special requirements; 

Note, however that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:

    - hosts: webserver
      roles:
        - role: bithium.lighttpd
          become: yes

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml` and `vars/main.yml`):

 * Packages to install for each OS: `lighttpd_package`

```yaml
---
lighttpd_package: "lighttpd"

lighttpd_packages:
  - "{{lighttpd_package}}"
```

 * Service name for lighttpd: `lighttpd_service: lighttpd`

 * Configuration folder: `lighttpd_config_dir: "/etc/lighttpd"`

 * Configuration folder for available module:

        lighttpd_config_available_dir: "{{lighttpd_config_dir}}/conf-available"

 * Configuration folder for enabled modules:

        lighttpd_config_enabled_dir: "{{lighttpd_config_dir}}/conf-enabled"

 * Global configuration file:

        lighttpd_config_file: "{{lighttpd_config_dir}}/lighttpd.conf"

 * Configuration file created by this role:

        lighttpd_extra_config_file: "{{lighttpd_config_enabled_dir}}/99_ansible.conf"

 * Configuration modules to add to configuration:

        lighttpd_modules:
           - status
           - userdir

 * Custom configuration options:

   This is an hash with the options that will be, either:
     - replaced in `lighttpd_config_file`, or
     - placed in `lighttpd_extra_config_file` as `key = value`, e.g:

        lighttpd_config:
           server.port = 8080
           server.document-root: "/tmp/foobar"
           fastcgi.debug: 1

  * User used in each OS for lighttpd:
```yaml
lighttpd_users:
  Alpine: 'lighttpd'

lighttpd_user: "{{ lighttpd_users[ansible_os_family] | default('www-data') }}"

```

  * Group used in each OS for lighttpd:

        lighttpd_group: "www-data"

Dependencies
------------

No dependencies.

Example Playbook
----------------

This example playbook enables the `fastcgi` module and starts the server on port 3000.

```yaml
---
- hosts: webserver
  become: yes
  vars:
    lighttpd_modules:
       - fastcgi   # Enable FastCGI
    lighttpd_config:
       server.port: 3000
  roles:
    - lighttpd
```

License
-------

Apache 2.0

Author Information
------------------

[Bithium S.A.](https://www.bithium.com/)
