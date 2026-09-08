.. index:: ibm, aix, ssh, openssh, install, installp

.. meta::
   :keywords: ibm, aix, ssh, openssh, install, installp

.. _ibm-virtualization-aix-install-sshd:

Installing OpenSSH on an AIX
============================

.. code-block:: bash

  # mount 1 dvd iso
  # mount -V cdrfs -o ro /dev/cd0 /mnt
  # cd /mnt/usr/sys/inst.images/
  # installp -ac -Y -d . openssh.base openssl.base openssl.man.en_US openssh.man.en_US
  # lssrc -s sshd