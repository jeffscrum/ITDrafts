.. index:: brocade, bna

.. meta::
   :keywords: brocade, bna

.. _bna-trial-reset:

Сброс trial-периода для Brocade Network Advisor (BNA)
=====================================================

Бывало ли у вас такое, что тестовый период заканчивается, то руководство до сих пор не может принять решение о покупке продукта? Тогда вам поможет дальнейшая инструкция.

.. attention:: После окончания тестового периода обязательно купите или удалите продукт, чтобы не стать пиратом!


Подключение к базе BNA
----------------------

Для подключения нужно воспользоваться любой программой для управления PostgreSQL, например `pgAdmin <https://www.pgadmin.org>`_. Подключаемся к базе со следующими параметрами

.. note::

  - **Hostname:** localhost
  - **Database:** dcmdb
  - **Port:** 5432 или 5431
  - **User:** dcmadmin
  - **Password:** passw0rd (если не меняли при установке)


Изменение данных в базе
-----------------------

-  Выполняем запрос к БД и получим 3 unixtimestamp значения. 
   Это текущие значения даты установки и окончания тестового периода (см скриншот).
    
    .. code-block:: sql
    
       select * from system_property
       where name = 'dcfm.install.time' or name = 'license.lastLicenseCheckTime' or name = 'license.expiryDate'        

    .. image:: /images/bna-postgre.webp
       :width: 800


-  Теперь отправляемся на сервис генерации времени в формате `unix <http://www.unixtimestamp.com>`_ и генерируем 3 новых значения: первая дата - сегодняшний день, вторая дата - +90 дней от текущей, третья дата - равна первой. Для удобного подсчета даты +90 дней можно воспользоваться вот этим сервисом https://calcsoft.ru/calculator-dney.

  .. note:: Новая дата должна быть не более чем +120 дней,иначе BNA будет писать что срок триала истек. Не забудьте прибавить 000 в конце даты которую вы получите выше (например 1472669739\ **000**)

-  После того как мы получили новые значения обновляем их в БД и обязательно перезапускаем сервер.

    .. code-block:: sql

      update system_property set value = 1472669739000 where name ='dcfm.install.time'
      update system_property set value = 1489806350000 where name ='license.expiryDate'
      update system_property set value = 1472669739000 where name ='license.lastLicenseCheckTime' 

- Теперь нужно полностью перезагрузить сервер и проверить дату оканчания испытательного срока