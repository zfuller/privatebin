# Privatebin Role

Role installs and configures PrivateBin https://privatebin.info/

## Requirements

Need to have an SSL Certificate, geerlingguy.certbot ansible galaxy role can be used to generate a Let's Encrypt SSL for the domain, if one does not already exist.

## Role Variables
### Task Variables
```yaml
private_bin_repo: https://github.com/PrivateBin/PrivateBin/
private_bin_version: 1.3.1
private_bin_archive_format: tar.gz
private_bin_web_user: www-data
private_bin_web_group: www-data
private_bin_www_directory: /var/www/html/privatebin
```

### conf.php Variables
These variables are the default ones that are set in the conf.sample.php file from the PrivateBin repo.
* Variables that are set `true` can be set `false` and vise versa.
* Variables that are left blank below are commented out in the config by default unless defined.
More deatils on the Variables here https://github.com/PrivateBin/PrivateBin/wiki/Configuration
Variables are fed into the [templates/conf.php.j2](templates/conf.php.j2) file

```yaml
private_bin_main_name:
private_bin_main_dicussion: "true"
private_bin_main_opendicussion: "false"
private_bin_main_password: "true"
private_bin_main_fileupload: "false"
private_bin_main_burnafterreadingselected: "false"
private_bin_main_defaultformatter: plaintext # plaintext, markdown, syntaxhighlighting
private_bin_main_syntaxhighlightingtheme: # sons-of-obsidian
private_bin_main_paste_sizelimit: 10485760
private_bin_main_template: bootstrap-dark # bootstrap, bootstrap-page, bootstrap-dark, bootstrap-dark-page, bootstrap-compact, bootstrap-compact-page, page
private_bin_main_notice:
private_bin_main_language_selection: "false"
private_bin_main_language_default:
private_bin_main_url_shortener:
private_bin_main_qrcode: "false"
private_bin_main_icon: identicon # identicon, vizhash, none
private_bin_main_cspheader:
private_bin_main_zerobincompatibility: "false"
private_bin_main_httpwarning: "true"
private_bin_main_compression: zlib # none
private_bin_expire_default: 1week # 5min 10min 1hour 1day 1week 1month 1year never

private_bin_expire_options: # add your own custom expire times
  - time: $nicename
    seconds: $seconds
  - time: $nicename_2
    seconds: $seconds_2

private_bin_traffic_limit: 10
private_bin_traffic_header:
private_bin_traffic_directory: data
private_bin_purge_limit: 300
private_bin_batchsize_limit: 10
private_bin_purge_directory: data

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

## Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
 - hosts: privatebinhost
   become: true
   vars_files:
     - vars/main.yml
   roles:
     - geerlingguy.apache
     - geerlingguy.php
     - geerlingguy.certbot
     - zfuller.privatebin
```
*vars/main.yml*
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

License
-------

GPLv2
