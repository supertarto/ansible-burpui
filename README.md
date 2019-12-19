# Ansible BurpUI
[![Build Status](https://travis-ci.org/supertarto/ansible-burpui.svg?branch=master)](https://travis-ci.org/supertarto/ansible-burpui)

Install and configure BurpUI with ansible. This role is meant to suits my need and is inspired by https://github.com/CoffeeITWorks/ansible_burpui_server if you want a more complete role, go check it!

## Requirements
A burp backup server

## Tested plateform
* Debian 10 (Buster)

## Role variables
Information about burpui version, wich template to use (0.6.6 and 0.7.0 provided) as conf file.
```yml
burpui_pip_package_name: "burp-ui"
burpui_version: 0.6.6
burpui_template_config_name: burpui-0.6.6.cfg.j2
burpui_global_port: "5000"
```
Name of the burpui user and group.
```yml
burpui_user: 'root'
burpui_group: 'root'
```
A list of burpui package to install. Some are commented by default.
```yml
burpui_pip_packages:
  - {name: "{{ burpui_pip_package_name }}", version: "{{ burpui_version }}"}
  - {name: "{{ burpui_pip_package_name }}[ldap_authentication]", version: "{{ burpui_version }}"}
  - {name: "{{ burpui_pip_package_name }}[gunicorn]", version: "{{ burpui_version }}"}
# - {name: "{{ burpui_pip_package_name }}[sql]", version: "{{ burpui_version }}"}
# - {name: "{{ burpui_pip_package_name }}[gunicorn-extra]", version: "{{ burpui_version }}"}
# - {name: "{{ burpui_pip_package_name }}[celery]", version: "{{ burpui_version }}"}
# - {name: "{{ burpui_pip_package_name }}[websocket]", version: "{{ burpui_version }}"}
```
List of dependencies needed to install burpUI.
```yml
burpui_pip_present:
  - "cryptography"
  - "cffi>=1.7"
  - "gevent>=1.2"
  - "ujson>=1.35"
  - "urllib3>=1.19"
  - "pyasn1"
  - "six>=1.10.0"
  - "requests[security]>=2.12"
  - "Flask-Session"
  - "Flask-Migrate"
```
Those values are used only in the burpui.cfg file for the version 0.6.6. 
```yml
burpui_standalone: true
burpui_global_version: "2"
burpui_global_prefix: "none"
burpui_ui_extras: []
burpui_backend_bhost: '::1'
burpui_backend_bport: '4972'
```
List of variables used in burpui.cfg Attapt those to your needs.
```yml
############
# [global]
burpui_global_auth: "basic"
burpui_global_acl: "basic"
burpui_global_audit: "basic"
burpui_global_plugins: 'none'
# [UI]
burpui_ui_refresh: "180"
burpui_ui_liverefresh: "60"
burpui_ui_ignore_labels: '"color:.*", "custom:.*"'
burpui_ui_format_labels: '"s/^os:\s*//"'
burpui_ui_default_strip: "0"
# [Production]
# redis or default
burpui_production_storage: "default"
# redis address or default
burpui_production_session: "default"
# redis ou default
burpui_production_cache: "default"
burpui_production_redis: "localhost:6379"
burpui_production_celery: "false"
burpui_production_database: "none"
burpui_production_limiter: "false"
burpui_production_ratio: '60/minute'
burpui_production_prefix: "none"
burpui_production_num_proxies: "0"
burpui_production_proxy_fix_args: "{'x_for': {num_proxies}, 'x_host': {num_proxies}, 'x_prefix': {num_proxies}}"
# [Websocket]
burpui_websocket_enabled: "true"
burpui_websocket_embedded: "false"
# redis, ou <url redis>, ou none
burpui_websocket_broker: "none"
burpui_websocket_url: "none"
burpui_websocket_debug: "false"
# [Security]
burpui_security_includes: "/etc/burp"
burpui_security_enforce: "false"
burpui_security_revoke: "true"
burpui_security_cookietime: "14"
burpui_security_sessiontime: "5"
burpui_security_scookie: "true"
burpui_security_appsecret: "ChangeIT!"
# [Experimental]
burpui_experimental_zip64: "false"
burpui_experimental_noserverrestore: "false"
# [Burp Backend]
burpui_backend_burpbin: "/usr/local/sbin/burp"
burpui_backend_stripbin: "/usr/local/bin/vss_strip"
burpui_backend_bconfcli: "/etc/burp/burp.conf"
burpui_backend_bconfsrv: "/etc/burp/burp-server.conf"
burpui_backend_tmpdir: "/tmp/bui"
burpui_backend_timeout: "15"
burpui_backend_deep_inspection: "false"
# [Parallel]
burpui_parallel_enabled: false
burpui_parallel_host: "::1"
burpui_parallel_port: "11111"
burpui_parallel_timeout: "15"
burpui_parallel_password: "password123456"
burpui_parallel_ssl: "true"
burpui_parallel_concurrency: "2"
burpui_parallel_init_wait: "15"
# [BASIC:AUDIT]
burpui_basic_audit_priority: "100"
burpui_basic_audit_level: "WARNING"
burpui_basic_audit_logfile: "none"
burpui_basic_audit_max_bytes: "30 * 1024 * 1024"
burpui_basic_audit_rotate: "5"
# [LDAP:AUTH]
burpui_ldap_enabled: false
burpui_ldap_version: "TLSv1"
burpui_ldap_priority: "1"
burpui_ldap_host: "127.0.0.1"
burpui_ldap_port: "389"
burpui_ldap_encryption: "ssl"
burpui_ldap_validate: "none"
burpui_ldap_cafile: "none"
burpui_ldap_searchattr: "uid"
burpui_ldap_filter: "(&({0}={1})(burpui=1))"
burpui_ldap_base: '"ou=users,dc=example,dc=com"'
burpui_ldap_binddn: '"cn=admin,dc=example,dc=com"'
burpui_ldap_bindpw: "Sup3rS3cr3tPa$$w0rd"
# [BASIC:AUTH]
burpui_basic_enabled: false
burpui_basic_priority: "2"
# Mixed allow textual password in conf file. Change it in production!
burpui_basic_mixed: "true"
burpui_basic_users:
  - {name: "admin", password: "admin"}
# [LOCAL:AUTH]
burpui_local_enabled: false
burpui_local_priority: "3"
burpui_local_users: "user1,user2"
burpui_local_limit: 1000
# [ACL]
burpui_acl_enabled: false
burpui_acl_extended: "true"
burpui_acl_assume_rw: "true"
burpui_acl_legacy: "false"
burpui_acl_inverse_inheritance: "false"
burpui_acl_implicit_link: "true"
# [BASIC:ACL]
burpui_basic_acl_enabled: false
burpui_basic_acl_priority: 100
burpui_basic_acl_admins: "user1,user2"
burpui_basic_acl_users: []
# burpui_basic_acl_users:
#  - +moderator = user5,user6
#  - @moderator = '{"agents":{"ro":["agent1"]}}'
```
## Installation
```
ansible-galaxy install supertarto.burpui
```
## License
GPL V3.0
