# Odoo

Ansible role to install and configure odoo.
Based on the role by osiell (https://github.com/osiell)

# Inventario

Inventory files are not inclueded here, please configure your hosts before
trying this role. See ansible documentation to configure your inventory.

## Systems supported

- Ubuntu Trusty (14.04)
- Ubuntu Xenial (16.04)
- CentOS 7 

## Variables

These are the role variables and their default values. 
Passwords should be changed and stored securely with Ansible Vault or your
prefered secrets manager.

```yaml
odoo_service: odoo
odoo_version: 8.0
odoo_user: odoo
odoo_user_passwd: odoo
odoo_logdir: "/var/log/{{ odoo_user }}"
odoo_workdir: "/home/{{ odoo_user }}/odoo"
odoo_rootdir: "/home/{{ odoo_user }}/odoo/server"
odoo_init: True
odoo_databases: []      # [{name: prod, locale: 'en_US.UTF-8'}]
odoo_config_file: "/home/{{ odoo_user }}/{{ odoo_service }}.conf"
odoo_force_config: False
odoo_repo_type: git     # git or hg
odoo_repo_url: https://github.com/odoo/odoo.git
odoo_repo_dest: "{{ odoo_rootdir }}"
odoo_repo_rev: 8.0

# Odoo parameters
odoo_config_addons_path: "/home/{{ odoo_user }}/odoo/addons,/home/{{ odoo_user }}/odoo/server/openerp/addons"
odoo_config_admin_passwd: admin
odoo_config_auto_reload: False
odoo_config_csv_internal_sep: ','
odoo_config_data_dir: "/home/{{ odoo_user }}/.local/share/Odoo"
odoo_config_db_host: False
odoo_config_db_host_user: "{{ ansible_ssh_user }}"
odoo_config_db_maxconn: 64
odoo_config_db_name: False
odoo_config_db_passwd: False
odoo_config_db_port: False
odoo_config_db_template: template1
odoo_config_db_user: odoo
odoo_config_dbfilter: '.*'
odoo_config_debug_mode: False
odoo_config_pidfile: None
odoo_config_proxy_mode: False
odoo_config_email_from: False
odoo_config_geoip_database: /usr/share/GeoIP/GeoLiteCity.dat
odoo_config_limit_memory_hard: 805306368
odoo_config_limit_memory_soft: 671088640
odoo_config_limit_time_cpu: 60
odoo_config_limit_time_real: 120
odoo_config_list_db: True
odoo_config_log_db: False
odoo_config_log_level: info
odoo_config_logfile: None
odoo_config_logrotate: False
odoo_config_longpolling_port: 8072
odoo_config_osv_memory_age_limit: 1.0
odoo_config_osv_memory_count_limit: False
odoo_config_max_cron_threads: 2
odoo_config_secure_cert_file: server.cert
odoo_config_secure_pkey_file: server.pkey
odoo_config_server_wide_modules: None
odoo_config_smtp_password: False
odoo_config_smtp_port: 25
odoo_config_smtp_server: localhost
odoo_config_smtp_ssl: False
odoo_config_smtp_user: False
odoo_config_syslog: False
odoo_config_timezone: False
odoo_config_translate_modules: ['all']
odoo_config_unaccent: False
odoo_config_without_demo: False
odoo_config_workers: 0
odoo_config_xmlrpc: True
odoo_config_xmlrpc_interface: ''
odoo_config_xmlrpc_port: 8069
odoo_config_xmlrpcs: True
odoo_config_xmlrpcs_interface: ''
odoo_config_xmlrpcs_port: 8071

# Extra options
odoo_user_sshkeys: False    # ../../path/to/public_keys/*
```


## Example (Playbook)

```yaml
- name: Odoo Server
  hosts: odoo_server
  sudo: yes
  roles:
    - odoo
  vars:
    - odoo_config_db_host: pg_server
    - odoo_config_db_user: odoo
    - odoo_repo_type: git
    - odoo_repo_url: https://github.com/odoo/odoo.git
    - odoo_repo_dest: /home/odoo/odoo/server
    - odoo_repo_rev: 8.0
```

When deploying, you can set the passwords with the `--extra-vars` option:

```sh
$ ansible-playbook -i servers servers.yml -l odoo_server --extra-vars "odoo_user_passwd=pAssWorD odoo_config_admin_passwd=SuPerPassWorD odoo_config_db_passwd=PaSswOrd"
```
