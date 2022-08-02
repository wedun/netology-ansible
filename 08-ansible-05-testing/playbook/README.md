Playbook устанавливает Clickhouse, Vector, Lighthouse на хосты, указанные в inventory файле  

Переменные из group_vars  
* clickhouse_version - версия Clickhouse
* clickhouse_packages - Название пакетов Clickhouse, с которыми работает playbook
* vector_version - версия пакета vector
* lighthouse_url - ссылка на репозиторий Lighthouse
* lighthouse_dir - каталог для файлов Lighthouse
* lighthouse_nginx_user - пользователь, из-под которого будет работать веб сервер Nginx

Inventory файл  
* "clickhouse" устанавливается на хост `clickhouse-08-03-01`
* "vector" устанавливается на хост `vector-08-03-01`
* "lighthouse" устанавливается на хост `lighthouse-08-03-01`

Playbook  
Play "Install Clickhouse" применяется на группу хостов "Clickhouse" и предназначен для установки и запуска Clickhouse  

* Get clickhouse distrib - скачивает пакеты для clickhouse.
* Install clickhouse packages - устанавливает clickhouse. 
* Create database` - создаёт базу для clickhouse.

Play "Install Vector" применяется на группу хостов "Vector" и предназначен для установки и запуска vector  

* *Get Vector distrib - Скачивание пакетов.
* Install vector - Устанавливает пакеты.
* Vector config template - Применяем шаблон конфига vector.

Play "Install lighthouse" применяется на группу хостов "lighthouse" и предназначен для установки и запуска lighthouse  

* Install git for Lighthouse- Устанавливаем git
* Install nginx for Lighthouse - Устанавливаем Nginx
* Apply nginx config for Lighthouse - Применяем конфиг Nginx

Template  
Шаблон vector.yml.j2 используется для настройки конфига vector.  
Шаблон nginx.conf.j2 используется для первичной настройки nginx.  
Шаблон lighthouse_nginx.conf.j2 настраивает nginx на работу с lighthouse.  
