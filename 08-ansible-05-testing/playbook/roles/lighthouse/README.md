Lighthouse
=========

This role install Lighthouse on Cent OS

Requirements
------------

Nginx and git should to be deployed before run this role

Role Variables
--------------

lighthouse_dir - Where to store Lighthouse files "/home/{{ ansible_user_id }}/lighthouse" defaults/main.yml
lighthouse_url - URL of Clickhouse repo "https://github.com/VKCOM/lighthouse.git" vars/main.yml

Dependencies
------------

No dependencies

Example Playbook
----------------

---
- name: Install Lighthouse
  hosts: lighthouse
  handlers:
    - name: Nginx reload
      become: true
      ansible.builtin.service:
        name: nginx
        state: restarted
  pre_tasks:
    - name: Install git for Lighthouse
      become: true
      ansible.builtin.yum:
        name: git
        state: present
    - name: Install epel-release for Lighthouse
      become: true
      ansible.builtin.yum:
        name: epel-release
        state: present
    - name: Install nginx for Lighthouse
      become: true
      ansible.builtin.yum:
        name: nginx
        state: present
    - name: Apply nginx config for Lighthouse
      become: true
      ansible.builtin.template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        mode: 0644
  roles:
    - lighthouse

License
-------

MIT

Dmitriy Laykom
------------------
