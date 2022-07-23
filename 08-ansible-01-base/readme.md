## Основная часть
1. Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.
```bash
ansible-playbook -i inventory/test.yml site.yml

PLAY [Print os facts] ************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************
ok: [localhost]

TASK [Print OS] ******************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ****************************************************************************************************************************************************
ok: [localhost] => {
    "msg": 12
}

PLAY RECAP ***********************************************************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.  
```bash
/devops-netology/08-ansible-01-base/playbook$ cat group_vars/all/examp.yml
---
  some_fact: "all default fact"
```
После изменения:
```bash
~/devops-netology/08-ansible-01-base/playbook$ ansible-playbook -i inventory/test.yml site.yml

PLAY [Print os facts] ************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************
ok: [localhost]

TASK [Print OS] ******************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ****************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}

PLAY RECAP ***********************************************************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
4. Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host.
Тестовый стенд: [docker-compose.yaml](docker-compose.yaml). Используем сторонний образ ubuntu, т.к. в основном образе нет python нужной версии  
```bash
PLAY [Print os facts] ************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ******************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ****************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}

PLAY RECAP ***********************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

5. Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.  
```bash
~/netology/08-ansible-01-base/playbook/group_vars$ cat deb/examp.yml && cat el/examp.yml
---
  some_fact: "deb default fact"
---
  some_fact: "el default fact"
```

6. Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.
```bash
~/netology/08-ansible-01-base$ ansible-playbook -i playbook/inventory/prod.yml playbook/site.yml

PLAY [Print os facts] ************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ******************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ****************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP ***********************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

7. При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.
```bash
~/netology/08-ansible-01-base$ ansible-vault encrypt playbook/group_vars/deb/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful
~/netology/08-ansible-01-base$ ansible-vault encrypt playbook/group_vars/el/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful
```

8. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.
```bash
~/netology/08-ansible-01-base$ ansible-playbook -i playbook/inventory/prod.yml playbook/site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ******************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ****************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP ***********************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

9. Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.  
Для подключения к control node нужен plugin "local"  

10. В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.
```yaml
local:
  hosts:
    localhost:
      ansible_connection: local
```
11. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.
```bash
~/netology/08-ansible-01-base$ ansible-playbook -i playbook/inventory/prod.yml playbook/site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************
ok: [ubuntu]
ok: [localhost]
ok: [centos7]

TASK [Print OS] ******************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ****************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [localhost] => {
    "msg": "all default facts"
}

PLAY RECAP ***********************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

## Ответы на вопросы:
* Где расположен файл с some_fact из второго пункта задания?
```bash
cat playbook/group_vars/all/examp.yml
---
  some_fact: "all default facts"
```
* Какая команда нужна для запуска вашего playbook на окружении test.yml?
```bash
ansible-playbook -i playbook/inventory/test.yml playbook/site.yml
```
* Какой командой можно зашифровать файл?
```bash
ansible-vault encrypt group_vars/deb/examp.yml
```
* Какой командой можно расшифровать файл?
```bash
ansible-vault dencrypt group_vars/deb/examp.yml
```
* Можно ли посмотреть содержимое зашифрованного файла без команды расшифровки файла? Если можно, то как?
```bash
ansible-vault view group_vars/deb/examp.yml
```
* Как выглядит команда запуска playbook, если переменные зашифрованы?
```bash
ansible-playbook -i playbook/inventory/prod.yml playbook/site.yml --ask-vault-pass
```
* Как называется модуль подключения к host на windows?
winrm/psrp  
* Приведите полный текст команды для поиска информации в документации ansible для модуля подключений ssh
```bash
ansible-doc -t connection ssh
```
* Какой параметр из модуля подключения ssh необходим для того, чтобы определить пользователя, под которым необходимо совершать подключение?
```bash
remote_user
```