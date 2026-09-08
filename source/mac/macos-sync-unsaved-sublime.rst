.. index:: macos, mac, sublime, session, sync, dropbox

.. meta::
   :keywords: macos, mac, sublime, session, sync, dropbox

.. _macos-sync-unsaved-sublime:

Sync sublime un-saved windows
=============================

Я использовал Dropbox для синхронизации текстов между компьютерами. Файл ``Session.sublime_session`` переложил в Dropbox, а на компьютерах создал symlink.

.. code-block:: none

   Sublime Text 3:
   ~/Library/Application Support/Sublime Text 3/Local/Session.sublime_session
    
   Sublime Text 2:
   ~/Library/Application Support/Sublime Text 2/Settings/Auto Save.sublime_session