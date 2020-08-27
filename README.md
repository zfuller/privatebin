# PrivateBin Role

Role installs and configures PrivateBin https://privatebin.info/

## Requirements

Need to have an SSL Certificate, geerlingguy.certbot ansible galaxy role can be used to generate a Let's Encrypt SSL for the domain, if one does not already exist.
You will also need a webserver with the requirements outlined [here](https://github.com/PrivateBin/PrivateBin/blob/master/INSTALL.md#installation).
ie. a LAMP stack.

## Role Variables
Defaults can be found in [templates/conf.php.j2](templates/conf.php.j2) and [defaults/main.yml](defaults/main.yml)

### Task Variables
```yaml
private_bin_repo: https://github.com/PrivateBin/PrivateBin/
private_bin_version: 1.3.4
private_bin_archive_format: tar.gz
private_bin_web_user: www-data
private_bin_web_group: www-data
private_bin_www_directory: /var/www/html/privatebin
```

### conf.php Variables
These variables are the default ones that are set in the conf.sample.php file from the PrivateBin repo.
* Variables that are set `true` can be set `false` and vise versa.
* Variable defaults are set in the template and can be viewed [templates/conf.php.j2](templates/conf.php.j2)
there are left blank below are commented out in the config by default unless defined.
More deatils on the Variables here https://github.com/PrivateBin/PrivateBin/wiki/Configuration
Variables are fed into the [templates/conf.php.j2](templates/conf.php.j2) file

```yaml
---
# task variables
private_bin_repo: https://github.com/PrivateBin/PrivateBin
private_bin_version: 1.3.4
private_bin_archive_format: tar.gz
private_bin_web_user: www-data
private_bin_web_group: www-data
private_bin_www_directory: /var/www/html/privatebin

# conf.php.j2 template variables
# https://github.com/PrivateBin/PrivateBin/wiki/Configuration
# https://github.com/PrivateBin/PrivateBin/blob/master/cfg/conf.sample.php
private_bin_model_class: Filesystem # Filesystem, MySql, SQLite
private_bin_model_fs_option_directory: data
private_bin_model_mysql_option_dsn:
private_bin_model_mysql_option_tbl:
private_bin_model_mysql_option_usr:
private_bin_model_mysql_option_pwd:
private_bin_model_mysql_option_opt:
private_bin_model_sqlite3_option_path:
private_bin_model_sqlite3_option_usr:
private_bin_model_sqlite3_option_pwd:
private_bin_model_sqlite3_option_opt:
```

### misc variables
If you want to have libsodium installed by this role, set this variable to true
```yaml
private_bin_install_libsodium: false
```

manual [libsodium install](https://doc.libsodium.org/installation/) instuctions

## Dependencies

None

## Example Playbook

```yaml
- name: setup provatebin app
  hosts: privatebinhost
  become: true
  vars:
    private_bin_main_name: "My private bin"
    private_bin_expire_default: "1hour"
    private_bin_main_defaultformatter: "markdown"
  roles:
    - geerlingguy.apache
    - geerlingguy.php
    - geerlingguy.certbot
    - zfuller.privatebin
```

*defaults/main.yml*
```yml
# Set default page name
private_bin_main_name: "My Private Bin"

# different install location than /var/www/html/privatebin
private_bin_www_directory: /home/webuser/privatebin

# Set custom expiration times
private_bin_expire_options:
  - time: 1min
    seconds: 60
  - time: 5min
    seconds: 300
  - time: 15min
    seconds: 900
  - time: 1hour
    seconds: 3600
```

## License

GPLv2
