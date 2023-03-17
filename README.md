ansible_vaultwarden
=========

Роль разворачивает vaultwarden (bitwarden-rs) в контейнере с бекэндом БД postgresql в отдельном контейнере. 

Requirements
------------


Role Variables
--------------

**Переменные для настройки Yubico**
```
yubico_client_id: not_used
yubico_secret_key: not_used
```

**Переменные Vaultwarden/Bitwarden для взаимодействия с внешним миром**
```
signups_allowed: false
smtp_host: smtp.gmail.com
smtp_from: robot@example.com
smtp_port: 465
smtp_security: force_tls
smtp_username: robot@example.com
smtp_password: some_password
domain: "https://bw.example.com"
```

**Переменные для запуска и настройки контейнера с бекендом БД**
```
pg4bw_image: postgres:11
pg4bw_container_name: bitwarden-db
pg4bw_data: /var/bitwarden-db
pg4bw_init_user_db_dir: /var/bitwarden-db-init
pg4bw_postgres_password: some_password
pg4bw_vaultwarden_user: vaultwarden
pg4bw_vaultwarden_password: some_password
```

**Переменные для запуска контейнера с ПО Vaultwarden/Bitwarden**
```
bw_image: vaultwarden/server:1.25.0
bw_data: /var/bitwarden
bw_container_name: bitwarden
bw_network_name: bw_network
```

Dependencies
------------


Example Playbook
----------------
```
  hosts: servers
  become: true
  roles:
    - { role: ansible_vaultwarden }
```
License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
