Tedee lock BLE characteristics
==============================

Main service **00000002-4899-489f-a301-fbee544b1db0** contains following characteristics:

#. **00000101-4899-489f-a301-fbee544b1db0**

    Characteristic used for notifying device connected to lock about specific infos described in section Notifications.

#. **00000201-4899-489f-a301-fbee544b1db0**

    It is not used currently. Should be deleted in nearest future.

#. **00000301-4899-489f-a301-fbee544b1db0**

    It is used for PTLS session establishement and maintenance. On that characteristic only lock can publish messages for connected device.

#. **00000401-4899-489f-a301-fbee544b1db0**

    It is used for PTLS session establishement and maintenance. On that characteristic connected device publish messages and lock reads it.

#. **00000501-4899-489f-a301-fbee544b1db0**

    Used for API commands send to lock and described in section Commands.

#. **00000601-4899-489f-a301-fbee544b1db0**

    After turning on dev logs via API_COMMAND device can read dev logs from that characteristic.