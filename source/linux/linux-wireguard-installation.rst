.. index:: linux, vpn, wireguard

.. meta::
   :keywords: linux, vpn, wireguard

.. _linux-wireguard-installation:

Установка Wireguard на Debian
=============================

.. code-block:: bash

   # Устанавливаем пакет WireGuard, утилиту для qr-кодов, iptables
   sudo apt install wireguard qrencode iptables-persistent
   
   
   # Вносим изменения в настройки ядра
   sudo echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
   sudo echo "net.ipv4.conf.all.accept_redirects = 0" >> /etc/sysctl.conf
   sudo echo "net.ipv4.conf.all.send_redirects = 0" >> /etc/sysctl.conf
   sudo echo "net.ipv4.conf.all.rp_filter = 1" >> /etc/sysctl.conf
   sudo echo "net.ipv4.conf.default.proxy_arp = 0" >> /etc/sysctl.conf
   sudo echo "net.ipv4.conf.default.send_redirects = 1" >> /etc/sysctl.conf
   sudo sysctl -p
   
   
   # Генерируем приватный и публичный ключи сервера
   sudo mkdir -p /etc/wireguard/keys
   wg genkey | sudo tee /etc/wireguard/keys/server.key | wg pubkey | sudo tee /etc/wireguard/keys/server.key.pub
   
   
   # Генерируем приватный и публичный ключи клиентов
   wg genkey | sudo tee /etc/wireguard/keys/user.key | wg pubkey | sudo tee /etc/wireguard/keys/user.key.pub

   
   # Генерируем ключ шифрования (PresharedKey)
   wg genpsk | tee /etc/wireguard/presharedkey


   # Настройка подключений к серверу (секций [Peer] может быть несколько). Имя интерфейса смотрим в `ip a` (в примере ens3)
   ---- /etc/wireguard/wg0.conf ----
   [Interface]
   Address = 172.16.34.1/24 # адрес сервера в сети VPN
   ListenPort = 51820	# порт WireGuard (udp)
   PrivateKey = <PRIVATE_SERVER_KEY>
   PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE
   PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o ens3 -j MASQUERADE

   [Peer]
   PublicKey = <PUBLIC_CLIENT_KEY>
   PresharedKey = <PRESHARED_KEY>
   AllowedIPs = 172.16.34.2/32	# адрес клиента в сети VPN
   ---- end file ----
    
   # Запускаем сервер и добавляем интерфейс в автозапуск
   sudo wg-quick up wg0
   sudo systemctl enable wg-quick@wg0
   
   
   # Проверяем что интерфейс поднялся 
   sudo wg show
   
   
   # Если требуется, очищаем настройки iptables
   sudo iptables -P INPUT ACCEPT
   sudo iptables -P FORWARD ACCEPT
   sudo iptables -F
   sudo iptables -Z
   
   
   # Настройка iptables (вместо ens3 вставить свой внешний интерфейс)
   sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
   sudo iptables -A INPUT -i lo -j ACCEPT
   sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
   sudo iptables -I INPUT -p udp --dport 51820 -j ACCEPT
   sudo iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE
   sudo iptables -A FORWARD -s 172.16.34.0/24 -j ACCEPT
   sudo iptables -A FORWARD -i wg0 -j ACCEPT
   sudo iptables -A INPUT -j DROP
   
   # Сохраняем настройки и перезагружаем фаервол
   netfilter-persistent save
   netfilter-persistent reload
    
   # Настройки клиентов
   # Ключи мы сгененрировали ранее, теперь они нам снова понадобятся
   mkdir -p /etc/wireguard/configs/user.conf
   ---- /etc/wireguard/configs/user.conf ----
   [Interface]
   Address = 172.16.34.2/32 # адрес клиента в VPN (см /etc/wireguard/wg0.conf)
   DNS = 8.8.8.8,8.8.4.4	# DNS которыми будет пользоваться клиент
   PrivateKey = <PRIVATE_CLIENT_KEY>

   [Peer]
   PublicKey = <PUBLIC_SERVER_KEY>
   PresharedKey = <PRESHARED_KEY>
   AllowedIPs = 0.0.0.0/0
   Endpoint = <SERVER_IP_OR_FQDN>:51820
   PersistentKeepalive = 25 # Проверять доступность сервера каждые 25 секунд
   ---- end file ----
   
   # Чтобы не переносить конфиг вручную на телефон, сгенерируем qr-код и выведем его в консоль
   sudo qrencode -t ansiutf8 < /etc/wireguard/configs/user.conf
   
   
   # Если требуется передать qr-код другому человеку, то его можно просто сфотографировать или сгенерировать PNG
   sudo qrencode -t png -o user.png < /etc/wireguard/configs/user.conf
   
   
   # Теперь достаточно открыть приложение WireGuard на телефоне и отсканировать полученный код
