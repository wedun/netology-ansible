Файл prod.yml подготовлен. В файл site.yml добавлен play для установки vector
Запустите ansible-lint site.yml и исправьте ошибки, если они есть.
```bash
~/netology-mnt-homeworks/08-ansible-03-yandex/playbook$ ansible-lint site.yml
~/netology-mnt-homeworks/08-ansible-03-yandex/playbook$
```
Попробуйте запустить playbook на этом окружении с флагом --check
```bash
~/netology-mnt-homeworks/08-ansible-03-yandex/playbook$ ansible-playbook -i inventory/prod.yml site.yml --check

PLAY [Install Clickhouse] ***********************************************************

TASK [Gathering Facts] **************************************************************
ok: [clickhouse-08-03-01]

TASK [Get clickhouse distrib] *******************************************************
changed: [clickhouse-08-03-01] => (item=clickhouse-client)
changed: [clickhouse-08-03-01] => (item=clickhouse-server)
failed: [clickhouse-08-03-01] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "item": "clickhouse-common-static", "msg": "Request failed", "response": "HTTP Error 404: Not Found", "status_code": 404, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] *******************************************************
changed: [clickhouse-08-03-01]

TASK [Install clickhouse packages] **************************************************
fatal: [clickhouse-08-03-01]: FAILED! => {"changed": false, "msg": "No RPM file matching 'clickhouse-client-22.3.3.44.rpm' found on system", "rc": 127, "results": ["No RPM file matching 'clickhouse-client-22.3.3.44.rpm' found on system"]}

PLAY RECAP **************************************************************************
clickhouse-08-03-01        : ok=2    changed=1    unreachable=0    failed=1    skipped=0    rescued=1    ignored=0
```
Запустите playbook на prod.yml окружении с флагом --diff. Убедитесь, что изменения на системе произведены.
Повторно запустите playbook с флагом --diff и убедитесь, что playbook идемпотентен.
```bash
PLAY [Install Vector] ******************************************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
ok: [vector-08-03-01]

TASK [Get Vector distrib] **************************************************************************************************************************************************
ok: [vector-08-03-01]

TASK [Install Vector] ******************************************************************************************************************************************************
changed: [vector-08-03-01]

TASK [Vector config template] **********************************************************************************************************************************************
ok: [vector-08-03-01]

RUNNING HANDLER [Start Vector] *********************************************************************************************************************************************
changed: [vector-08-03-01]

PLAY [Install lighthouse] **************************************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
ok: [lighthouse-08-03-01]

TASK [Install git for Lighthouse] ******************************************************************************************************************************************
ok: [lighthouse-08-03-01]

TASK [Install nginx for Lighhouse] *****************************************************************************************************************************************
ok: [lighthouse-08-03-01]

TASK [Apply nginx config for Lighthouse] ***********************************************************************************************************************************
ok: [lighthouse-08-03-01]

TASK [Clone repository for Lighthouse] *************************************************************************************************************************************
ok: [lighthouse-08-03-01]

TASK [Apply config for Lighthouse] *****************************************************************************************************************************************
ok: [lighthouse-08-03-01]

PLAY RECAP *****************************************************************************************************************************************************************
clickhouse-08-03-01        : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0
lighthouse-08-03-01        : ok=6    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
vector-08-03-01            : ok=5    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Подготовьте [README.md](playbook/README.md) файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.