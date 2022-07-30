Vector
=========

This role can install Vector on Centos

Requirements
------------

No requirements

Role Variables
--------------

vector_version - Vector version to install. 0.22.1 for defaults. Can be changed in defaults/main.yml
vector_clickhouse_ip - Addres of Clickhouse instance. localhost for defaults. Can be changed in defaults/main.yml
clickhouse_db_name - Clickhouse DB where to store logs. "logs" for defaults. Can be changed in defaults/main.yml
clickhouse_table_name - Clickhouse table name to write logs. "data_logs" for defaults. Can be changed in defaults/main.yml
vector_url - URL for Vector download. <https://packages.timber.io/vector/>{{ vector_version }}/vector-{{ vector_version }}-1.x86_64.rpm for defaults. Can be changed in vars/main.yml

Dependencies
------------

No dependencies

Example Playbook
----------------

---
- name: Install Vector
  hosts: vector
  roles:
    - vector

License
-------

MIT

Dmitriy Laykom
------------------
