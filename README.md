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

### Default variables

 * Packages to install for each OS: `lighttpd_package`

   ```yaml
   lighttpd_package: "lighttpd"

   lighttpd_packages:
     - "{{lighttpd_package}}"
   ```

 * Service name for lighttpd: `lighttpd_service: lighttpd`

### OS specific variables

 * User used to run the server: `lighttpd_user`

 * Group used to run the server: `lighttpd_group`

 * Base configuration path: `lighttpd_config_path`

 * Available configurations path:

   ```yaml
   lighttpd_config_available_path: "{{lighttpd_config_path}}/conf-available"
   ```

 * Available sites configuration path:

   ```yaml
   lighttpd_sites_available_path: "{{lighttpd_config_available_path}}"
   ```

 * Enabled configurations path:

   ```yaml
   lighttpd_config_enabled_path: "{{lighttpd_config_path}}/conf-enabled"
   ```

 * Enabled sites configuration path:

   ```yaml
   lighttpd_sites_enabled_path: "{{lighttpd_config_enabled_path}}"
   ```

 * Main configuration file:

   ```yaml
   lighttpd_config_file: "{{lighttpd_config_path}}/lighttpd.conf"
   ```

 * Configuration file created by this role:

   ```yaml
   lighttpd_extra_config_file: "{{lighttpd_config_enabled_dir}}/25_ansible.conf"
   ```

 * Configuration modules to add to the configuration:

   ```yaml
   lighttpd_modules:
      - status
      - userdir
   ```

 * Custom configuration options:

   This is an hash with the options that will be, either:
     - replaced in `lighttpd_config_file`, or
     - placed in `lighttpd_extra_config_file` as `key = value`, e.g:


   ```yaml
   lighttpd_config:
      server.port: 8080
      server.document-root: "/tmp/foobar"
      fastcgi.debug: 1
   ```

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
