How to get Activity Logs from Tedee Lock
========================================

To send specific commands to get Activity Logs you need to :doc:`establish PTLS session <establish-ptls-session>`.

After you have successfully established the PTLS session you need to turn on BLE indications on :ref:`API commands characteristic <api_commands_characteristic>`.
To receive notifications about existing Activity Logs turn on notifications on :ref:`notifications_characteristic`.

.. note::
    | If Lock has any Activity Logs ready to collect, it will send the notification every 5 seconds. 
    | See :doc:`Has Logs nofification for details <../notifications/activity-logs/has-activity-logs>`.

