.. index:: hds, aix, odm

.. _hds-midrange-aix-odm:

HDS AIX ODM Install
===================

Для включения мультипасинга для массива AMS2000 на AIX требуется установка ODM. Если требуется склейка путей самим AIX'ом, то ставим с поддержкой MPIO, если потом диски будут переданы в сторонний софт мультипасинга (VxDMP, например) - ставим БЕЗ MPIO.

Пакеты доступны на HDS TUF: https://tuf.hds.com/gsc/bin/view/Main/AIXODMUpdates

.. tip::

  | Апдейт ODM можно производить только последовательно - т.е. 5.4.0.0 --> 5.4.0.1 --> 5.4.0.2 --> 5.4.0.3...
  | Или же положить все апдейты в один каталог и поставить сразу за один раз через smit installp