Tedee lock BLE characteristics
==============================

Main :ref:`Tedee Lock's service <tedee_lock_service>` contains following characteristics:

.. _notifications_characteristic:

Notifications
-------------

    **UUID: 00000101-4899-489f-a301-fbee544b1db0**

    Characteristic used for notifying device connected to lock about specific infos described in section **Notifications**.
    All notifications (with some exceptions described in **Notifications** section) are sent as encrypted data. They therefore require a prior establishment of a PTLS session.

.. _ptls_tx_characteristic:

PTLS TX
-------

    **UUID: 00000301-4899-489f-a301-fbee544b1db0**

    It is used for PTLS session establishement and maintenance. On that characteristic only lock can publish messages for connected device.

.. _ptls_rx_characteristic:

PTLS RX 
-------

    **UUID: 00000401-4899-489f-a301-fbee544b1db0**

    It is used for PTLS session establishement and maintenance. On that characteristic connected device publish messages and lock reads it.

.. _api_commands_characteristic:

API commands
------------

    **UUID: 00000501-4899-489f-a301-fbee544b1db0**

    Communication via Indications. Used for API commands send to lock and described in section **Commands**.
    API commands (except some described in **Commands** section) are also encrypted and require an active PTLS session.

.. _dev_logs_characteristic:

Dev logs
--------

    **UUID: 00000601-4899-489f-a301-fbee544b1db0**

    After turning on dev logs via API_COMMAND device can read dev logs from that characteristic.
    The channel to which the system logs are sent is not encrypted, but requires to be enabled via the API command (SET_BLE_LOGS) for proper operation.
