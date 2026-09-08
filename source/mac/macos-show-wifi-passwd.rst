.. index:: macos, mac, wifi, password, keychain, security, cli

.. meta::
   :keywords: macos, mac, wifi, password, keychain, security, cli

.. _macos-show-wifi-passwd:

Wi-Fi пароли и Keychain Access через CLI
========================================

Конечно же, можно просто открыть Keychain Access и посмотреть там, а можно это сделать через terminal

.. code-block:: none

   security find-generic-password -wa "НАЗВАНИЕ-ВАЙФАЙ-СЕТИ"