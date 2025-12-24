# SIEM

Стандартный сбор логов nginx настроен на access_json.log и error_json.log 

 /var/log/nginx/access_json.log
 
 /var/log/nginx/error_json.log

Для отслеживания возможных ошибок:

docker logs -f single-node-wazuh.manager-1

Проверка того, что Docker использует актуальный конфиг путем сравнение хеш-сумм

docker exec -it single-node-wazuh.manager-1 bash -lc 'ls -l /var/ossec/etc/ossec.conf /wazuh-config-mount/etc/ossec.conf; sha256sum /var/ossec/etc/ossec.conf /wazuh-config-mount/etc/ossec.conf'

В случае если конфиг в докере не актуальный, его можно насильно поменять на /var/ossec/etc/ossec.conf

docker exec -it single-node-wazuh.manager-1 bash -lc 'cp -f /wazuh-config-mount/etc/ossec.conf /var/ossec/etc/ossec.conf && /var/ossec/bin/wazuh-control restart'

Для проверки проверки алертов (alerts.json) внутри контейнера

docker exec -it single-node-wazuh.manager-1 bash -lc 'tail -n 50 /var/ossec/logs/alerts/alerts.json'

Для проверки корректности обработки логов можно использовать Wazuh logtest (проверка декодинга/кастомных правил/стандартных)

docker exec -it single-node-wazuh.manager-1 /var/ossec/bin/wazuh-logtest


Работа с докером

В директории /wazuh-docker/single-node/ 

Включить:
docker compose up -d

Выключить:
docker compose down


Проверка контейнеров:
docker ps


Для корректной работы сбора логов Nginx необходимо поместить правила local_rules.xml в /var/ossec/etc/rules/local_rules.xml и выдать права 644

Демонстрация

cp /tmp/local_rules.xml /var/ossec/etc/rules/local_rules.xml

chmod 644 /var/ossec/etc/rules/local_rules.xml

Доступ на веб https://localhost:4443

Учетьные данные 

admin:SecretPassword
