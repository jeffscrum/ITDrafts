.. index:: ibm, dlpar, rmc, aix, hmc

.. meta::
   :keywords: ibm, dlpar, rmc, aix, hmc

.. _ibm-virtualization-rmc-connection-issue:

Fixing the No RMC Connection Error
==================================

1. Проверяем что есть связь HMC <--> LPAR по IP например командой ping или ssh. Если соединения нет, то сначала решаем проблему с сетью.

2. Проверяем есть ли доступ на HMC по 657 порту, именно на этом порту висит служба RMC: ``telnet <HMC_IP> 657`` или ``netstat -tulpn | grep 657``

3. Далее cмотрим параметр DCaps на НМС: ``lspartition -dlpar | fgrep 214 -A1`` (где вместо 214 указываем последний октет IP-адреса lpar). Его значение должно быть отлично от ``<0x0>``. Если оно все же ``<0x0>``, то значит RMC соединения у вас нет.

4. Со стороны LPAR можно проверить с какими HMC есть соединение: ``lsrsrc IBM.MCP``

5. Пробуем выполнить перезапуск демона RMC

  .. code-block:: bash
  
     /usr/sbin/rsct/bin/rmcctrl -z
     /usr/sbin/rsct/bin/rmcctrl -A
     /usr/sbin/rsct/bin/rmcctrl -p

6) Если через некоторое время RMC соединение не восстановилось, то пробуем реконфигурацию RMC

  .. code-block:: bash
  
     /usr/sbin/rsct/install/bin/recfgct
     /usr/sbin/rsct/bin/rmcctrl -p

7) Сбрасываем соединения RMC на HMC

  .. code-block:: bash
  
     lspartition -dlparreset (use if HMC v7)
     diagrmc --autocorrect -v (Use if HMC v8)

8. Ждем некоторое время и проверяем снова (вместо <SYS_NAME> подставляем имя системы как оно записано в HMC (lssyscfg -r sys -F name), вместо <LPARID> значение ID партиции). Состояние 3 столбца должно стать "active"

  .. code-block:: bash
  
     lssyscfg -r lpar -m <SYS_NAME> -F lpar_id,state,rmc_state,rmc_ipaddr,os_version,dlpar_mem_capable,dlpar_proc_capable,dlpar_io_capable --filter "lpar_ids=<LPARID>"

  Примерный ответ будет выглядеть так
  
  .. code-block:: none
  
    11,Running,active,192.168.1.3,AIX 7.1 7100-03-04-1441,0,0,0

.. seealso::

  - https://www.ibm.com/support/knowledgecenter/en/SGVKBA_3.2.2/com.ibm.rsct.trouble/RMC_connection_diagnosis.htm
  - http://www-01.ibm.com/support/docview.wss?uid=isg3T1020611

