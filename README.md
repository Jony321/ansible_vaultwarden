ansible_vaultwarden
=========

Роль разворачивает vaultwarden (bitwarden-rs) в контейнере с бекэндом БД postgresql в отдельном контейнере. 

Requirements
------------
- ansible
- python3-pip
- Для тестирования роли с помощью molecule требуется установка соответствующих пакетов:  
  - `pip install -r requirements.txt`
  - docker, docker-compose (см. Dependencies|Установка docker и необходимых python библиотек)
    - `ansible-galaxy install geerlingguy.pip geerlingguy.docker`
  - Добавить пользвоателя в группу docker

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

❗ **Переменные, которые стоит хранить в зашифрованном виде:**
```
pg4bw_postgres_password: some_password
pg4bw_vaultwarden_user: vaultwarden
pg4bw_vaultwarden_password: some_password

smtp_from: robot@example.com
smtp_username: robot@example.com
smtp_password: some_password

# AWS s3 bucket variables (optional)
pg4bw_aws_key: 'some_aws_access_key'
pg4bw_aws_secret: 'some_aws_secret_key'
pg4bw_aws_region: 'some_aws_region'
pg4bw_s3_bucket: 's3://aws_s3_bucket_name'

# Telegram notification variables
pg4bw_aprise_target: 'some_aprise_target'
```

Dependencies
------------
- Установка docker и необходимых python библиотек:
```
---
# pip.yml
#
- hosts: servers (or localhost if not installed)
  become: true
  vars:
    pip_install_packages:
      - name: docker
      - name: docker-compose
  roles:
    - geerlingguy.pip
    - geerlingguy.docker
```

`$ ansible-playbook pip.yml`

Example Playbook
----------------
Установка molecule и тестирование роли `jony321.ansible_vaultwarden`:
```
$ ansible-galaxy install jony321.ansible_vaultwarden -p [/path/to/roles/folder]
$ pip install -r requirements.txt
$ cd [/path/to/roles/folder]/jony321.ansible_vaultwarden
$ ~/.local/bin/molecule test
```
Развертывание Vaultwarde/Bitwarden с БД postgresql:
```
---
# main.yml
#
- hosts: servers
  become: true
  roles:
    - { role: jony321.ansible_vaultwarden }
```
License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
