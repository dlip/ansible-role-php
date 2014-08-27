php
========

This role installs and configures the php with cli.

Requirements
------------

This role requires Ansible 1.4 or higher and tested platforms are listed in the metadata file.

Role Variables
--------------

The role uses the following variables:

 - **php_fpm_pools**: The list a pools for php-fpm, each pools is a hash with
   a name entry (used for filename), all the other entries in the hash are pool
   directives (see http://php.net/manual/en/install.fpm.configuration.php).
 - **php_fpm_apt_packages**: The list of packages to be installed by the
  ```apt```, defaults to ```[php5-fpm]```.
   module.
 - **php_fpm_yum_packages**: The list of packages to be installed by the
   ```yum``` module, defaults to ```[php-fpm]```.
 - **php_fpm_ini**: Customization for php-fpm's php.ini as a list of options,
   each option is a hash using the following structure:
     - **option**: The name of the option.
     - **value**: The string value to be associated with the option.
     - **section**: Section name in INI file.
 - **php_fpm_config**: Customization for php-fpm's configuration file as a list
   of options.
 - **php_fpm_apt_packages**: The APT packages to install, defaults to ```[php5-fpm]```.
 - **php_fpm_yum_packages**: The Yum packages to install, defaults to ```[php-fpm]```.
 - **php_fpm_default_pool**:
     - **delete**: Set to a ```True``` value to delete the default pool.
     - **name**: The filename the default pool configuration file.

Example usages
--------------

    - role: php-fpm
      php_fpm_pools:
       - name: foo
         user: www-data
         group: www-data
         listen: 8000
         pm: dynamic
         pm.max_children: 5
         pm.start_servers: 2
         pm.min_spare_servers: 1
         pm.max_spare_servers: 3
         chdir: /
       - name: bar
         user: www-data
         group: www-data
         listen: 8001
         pm: dynamic
         pm.max_children: 5
         pm.start_servers: 2
         pm.min_spare_servers: 1
         pm.max_spare_servers: 3
         chdir: /
       php_fpm_ini:
       # PHP section directives
       - option: "engine"
         section: "PHP"
         value: "1"
       - option: "error_reporting"
         section: "PHP"
         value: "E_ALL & ~E_DEPRECATED & ~E_STRICT"
       - option: "date.timezone"
         section: "PHP"
         value: "Europe/Berlin"
       # soap section directives
       - option: "soap.wsdl_cache_dir"
         section: "soap"
         value: "/tmp"
       # Pdo_mysql section directives
       - option: "pdo_mysql.cache_size"
         section: "Pdo_mysql"
         value: "2000"
       php_fpm_config:
       - option: "log_level"
         section: "global"
         value: "notice"
       - option: "syslog.facility"
         section: "global"
         value: "daemon"

License
-------

BSD

Author Information
------------------

- Pierre Buyle <buyle@pheromone.ca>
- Sergey Fayngold
