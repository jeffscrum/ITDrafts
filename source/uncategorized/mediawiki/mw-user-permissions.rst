.. index:: mediawiki

.. _mw-user-permissions:


User permissions
================

Вносим изменения в файл *LocalSettings.php*

.. code-block:: none

  ################
  # Restrictions #
  ################
  $wgDisableAnonTalk = false;   // Disable talk pages for anonymous users (IPs)
  $wgShowIPinHeader = false;    // Show the IP in the user bar for anonymous users by default
