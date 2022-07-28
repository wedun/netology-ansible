Плейбук скачивает и устанавливает на хост объявленный в inventory пакеты для clickhouse и vector.  
Компоненты Clickhouse версии `22.3.3.44`:    
* clickhouse-client  
* clickhouse-server  
* clickhouse-common-static  
Компоненты версии Vector `0.22.0`  

Параметрый playbook:  
Файл [playbook/inventory/prod.yml](./inventory/prod.yml. В этом файле указываются параметры подключения к хосту.  
Файле [playbook/group_vars/clickhouse/vars.yml](./group_vars/clickhouse/vars.yml) список компонент Clickhouse для установки и их версии. Также в этом файле указана версия Vector  
Теги в playbook не используются