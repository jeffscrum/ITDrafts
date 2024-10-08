.. index:: putty, ssh, tunnel

.. meta::
   :keywords: putty, ssh, tunnel

.. _putty-socks5-tunnel:

Putty SOCKS5-proxy туннель в Windows
====================================

.. image:: /images/putty-ssh-tunnels-tunneling.webp
   :scale: 80 %

Настройка SOCKS5-proxy может понадобиться, если, например, нужно попасть по Web на какой-то хост за сервером к которому есть только доступ по SSH.

В этом случае мы можем использовать сервер с SSH–демоном как промежуточный(proxy). Чтобы заставить PuTTY исполнять роль socks5–прокси, нужно настроить параметры SSH–сессии следующим образом:

.. image:: /images/putty-ssh-tunnels-putty.webp
   :scale: 50 %

В результате, после успешной авторизации, на клиенте можно будет наблюдать следующее:

.. code-block:: none

  C:\>netstat -ano | find "1080"
    TCP    127.0.0.1:1080     0.0.0.0:0      LISTENING       2392
  C:\>tasklist | find /i "2392"
  putty.exe

То есть putty, выполняющийся с PID–ом 2392, начинает слушать порт 1080, ожидая подключений. Далее берем любое приложение, умеющее работать с
SOCKS5–прокси, например Firefox, и указываем ему использовать наш прокси:

.. image:: /images/putty-ssh-tunnels-firefox.webp
   :scale: 80 %

Теперь все запросы от браузера будут проходить через SSH, а далее перенаправляться нашим SOCKS-proxy в сторону нужного адресата.
