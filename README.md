# Kallithea

Ansible role to install Kallithea and configure it.

Minimum Ansible Version: 1.8

## Systems supported

* Debian
    - Wheezy    (7)
* Ubuntu
    - Precise   (12.04)
    - Trusty    (14.04)

## Example (Playbook)

Standard installation (SQLite database):

```yaml
- name: Kallithea
  hosts: kallithea_server
  roles:
    - kallithea
  vars:
    - kallithea_admin_user: admin
    - kallithea_admin_password: password
    - kallithea_admin_email: admin@example.net
```

Installation with a PostgreSQL database installed on the local host:

```yaml
- name: Kallithea
  hosts: kallithea_server
  roles:
    - kallithea
  vars:
    - kallithea_admin_user: admin
    - kallithea_admin_password: password
    - kallithea_admin_email: admin@example.net
    - kallithea_db_name: kallithea
    - kallithea_db_host: localhost
    - kallithea_db_port: 5432
    - kallithea_db_user: kallithea
    - kallithea_db_passwd: PaSswOrd
    - kallithea_config_sqlalchemy_db1_url: "postgresql://{{ kallithea_db_user }}:{{ kallithea_db_passwd }}@{{ kallithea_db_host }}:{{ kallithea_db_port }}/{{ kallithea_db_name }}"
```

Installation with a PostgreSQL database installed on a remote host (and
available from your Ansible inventory):

```yaml
- name: Kallithea
  hosts: kallithea_server
  roles:
    - kallithea
  vars:
    - kallithea_admin_user: admin
    - kallithea_admin_password: password
    - kallithea_admin_email: admin@example.net
    - kallithea_db_name: kallithea
    - kallithea_db_host: 192.168.1.10
    - kallithea_db_host_user: "{{ ansible_ssh_user }}"
    - kallithea_db_port: 5432
    - kallithea_db_user: kallithea
    - kallithea_db_passwd: PaSswOrd
    - kallithea_config_sqlalchemy_db1_url: "postgresql://{{ kallithea_db_user }}:{{ kallithea_db_passwd }}@{{ kallithea_db_host }}:{{ kallithea_db_port }}/{{ kallithea_db_name }}"
```

On the first run, you have to tell *Ansible* to initialize the database with
the `kallithea_db_init` option:

```sh
ansible-playbook playbook.yml -i inventory -e "kallithea_db_init=True"
```

## Variables

```yaml
kallithea_user: kallithea
kallithea_version: 0.2.1
kallithea_workdir: "/home/{{ kallithea_user }}"
kallithea_logdir: "/var/log/kallithea"
kallithea_repodir: "{{ kallithea_workdir }}/repos"
kallithea_venv: "{{ kallithea_workdir }}/venv"
kallithea_config_file: "{{ kallithea_workdir }}/kallithea.ini"
kallithea_admin_user: admin
kallithea_admin_password: admin
kallithea_admin_email: admin@example.net
kallithea_repos_path: "{{ kallithea_workdir }}/repos"
kallithea_service_name: kallithea
kallithea_service_init: True

# Database server configuration
kallithea_db_init: False  # 'True' will destroy any existing database!
kallithea_db_name: kallithea
kallithea_db_host: False
kallithea_db_host_user: "{{ ansible_ssh_user }}"
kallithea_db_port: False
kallithea_db_user: "{{ kallithea_user }}"
kallithea_db_passwd: password

# Kallithea configuration
kallithea_config_waitress_threads: 5
kallithea_config_host: 0.0.0.0
kallithea_config_port: 5000
kallithea_config_lang: ''   # cs de fr hu ja nl_BE pl pt_BR ru sk zh_CN zh_TW
kallithea_config_cache_dir: '%(here)s/data'
kallithea_config_index_dir: '%(here)s/data/index'
kallithea_config_archive_cache_dir: '%(here)s/tarballcache'
kallithea_config_app_instance_uuid: {6542ab52-5502-45d1-a84a-c814891ac07d}
kallithea_config_cut_off_limit: 256000
kallithea_config_vcs_full_cache: true
kallithea_config_force_https: false
kallithea_config_use_htsts: false
kallithea_config_commit_parse_limit: 25
kallithea_config_git_path: git
kallithea_config_rss_cut_off_limit: 256000
kallithea_config_rss_items_per_page: 10
kallithea_config_rss_include_diff: false
kallithea_config_show_sha_length: 12
kallithea_config_show_revision_number: false
kallithea_config_gist_alias_url: ''
kallithea_config_api_access_controllers_whitelist: ''
kallithea_config_default_encoding: utf8
kallithea_config_bugtracker: ''
kallithea_config_issue_pat: '(?:\s*#)(\d+)'
kallithea_config_issue_server_link: 'https://myissueserver.com/{repo}/issue/{id}'
kallithea_config_issue_prefix: '#'
kallithea_config_instance_id: ''
kallithea_config_auth_ret_code: ''
kallithea_config_lock_ret_code: 423
kallithea_config_allow_repo_location_change: True
kallithea_config_allow_custom_hooks_settings: True
# Kallithea configuration (CELERY)
kallithea_config_use_celery: false
kallithea_config_broker_host: localhost
kallithea_config_broker_vhost: rabbitmqhost
kallithea_config_broker_port: 5672
kallithea_config_broker_user: rabbitmq
kallithea_config_broker_password: qweqwe
kallithea_config_celery_imports: kallithea.lib.celerylib.tasks
kallithea_config_celery_result_backend: amqp
kallithea_config_celery_result_dburi: amqp://
kallithea_config_celery_result_serialier: json
kallithea_config_celery_send_task_error_emails: false
kallithea_config_celery_amqp_task_result_expires: False
kallithea_config_celeryd_concurrency: 2
kallithea_config_celeryd_log_file: False
kallithea_config_celeryd_log_level: DEBUG
kallithea_config_celeryd_max_tasks_per_child: 1
kallithea_config_celery_always_eager: false
# Kallithea configuration (BEAKER)
kallithea_config_beaker_cache_data_dir: '%(here)s/data/cache/data'
kallithea_config_beaker_cache_lock_dir: '%(here)s/data/cache/lock'
kallithea_config_beaker_session_key: kallithea
kallithea_config_beaker_session_secret: {6542ab52-5502-45d1-a84a-c814891ac07d}
kallithea_config_beaker_session_timeout: 2592000
kallithea_config_beaker_session_httponly: true
kallithea_config_beaker_session_secure: false
kallithea_config_beaker_session_auto: False
# Kallithea configuration (DB)
kallithea_config_sqlalchemy_db1_url: sqlite:///%(here)s/{{ kallithea_db_name }}.db?timeout=60
                                    # "postgresql://{{ kallithea_db_user }}:{{ kallithea_db_passwd }}@{{ kallithea_db_host or 'localhost' }}:{{ kallithea_db_port or 5432 }}/kallithea"
                                    # mysql://{{ kallithea_db_user }}:{{ kallithea_db_passwd }}@{{ kallithea_db_host }}/{{ kallithea_db_name }}
kallithea_config_sqlalchemy_db1_echo: false
kallithea_config_sqlalchemy_db1_pool_recycle: 3600
kallithea_config_sqlalchemy_db1_convert_unicode: true

# Logrotate configuration
kallithea_logrotate: True
kallithea_logrotate_options:
    - rotate 15
    - daily
    - compress
    - copytruncate
    - missingok
    - notifempty
    - "create 0640 {{ kallithea_user }} adm"
    - su root root
```
