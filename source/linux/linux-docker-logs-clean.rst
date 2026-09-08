.. index:: linux, docker, logs, clean, truncate, json

.. meta::
   :keywords: linux, docker, logs, clean, truncate, json

.. _linux-docker-logs-clean:

Чистим логи Docker
==================

Логи находятся в папках контейнеров тут - ``/var/lib/docker/containers/``
 
Подсчет места занимаемого логами - ``sudo sh -c 'du -ch /var/lib/docker/containers/*/*-json.log'``
 
Обнуление логов - ``sudo sh -c 'truncate -s 0 /var/lib/docker/containers/*/*-json.log'``