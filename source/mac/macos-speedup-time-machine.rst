.. index:: macos, mac, time-machine, backup, speed, sysctl

.. meta::
   :keywords: macos, mac, time-machine, backup, speed, sysctl

.. _macos-speedup-time-machine:

Speed up Time Machine backups
=============================

.. code-block:: none

   sudo sysctl -a | grep throttle
   sudo sysctl debug.lowpri_throttle_enabled=0
   sudo sysctl debug.lowpri_throttle_enabled=1 (Default value is 1)