Get Activity Logs
=================

GET_LOGS_TLV code is 0x2D

| This command is used to collect a package of Activity Logs from Lock in a TLV format.
| More about binary format `Type–Length–Value can be found here <https://en.wikipedia.org/wiki/Type%E2%80%93length%E2%80%93value>`_.
| Visit :doc:`How to: Get Activity Logs <../../howtos/activity-logs>` and check the attached example.

Input parameters
----------------
none

Result
------
+-----------------+-----------+------------------------------------------------------+
| **Result name** | **Value** | **Description**                                      |
+-----------------+-----------+------------------------------------------------------+
| SUCCESS         | 0x00      | | Response contains a package of Activity Logs       |
|                 |           | | AND there are more packages to collect so,         |
|                 |           | | the GET_LOGS_TLV command should be sent again      |
+-----------------+-----------+------------------------------------------------------+
| NOT_FOUND       | 0x04      | | Response MAY contain a package of Activity Logs    |
|                 |           | | BUT it does NOT have to. This result is sent in two|
|                 |           | | cases:                                             |
|                 |           | | 1) When last package of logs is returned or        |
|                 |           | | 2) there are no Activity Logs to transfer          |
+-----------------+-----------+------------------------------------------------------+
| BUSY            | 0x03      | | A package with Activity Logs is in preparation.    |
|                 |           | | The GET_LOGS_TLV command should be called again    |
|                 |           | | with a short delay (100 - 500ms)                   |
+-----------------+-----------+------------------------------------------------------+
| ERROR           | 0x02      | | Package of Activity Logs cannot be sent due to     |
|                 |           | | insufficient BLE message size. Check the MTU size. |
|                 |           | | A minimum size of single Activity Log package is   |
|                 |           | | 98 bytes (plus the PTLS session overhead)          |
+-----------------+-----------+------------------------------------------------------+
| NO_PERMISSION   | 0x07      | | No permission to transfer Activity Logs.           |
+-----------------+-----------+------------------------------------------------------+

Output parameters
-----------------

| The output is a binary data (in TLV format) using the following tags (fields):

+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| **Tag Name**    | **Tag Number** | **Length**| **Description**                                                                                                                                   |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| VERSION         | 0x01           | 1         | | Version of the package format. The value of this is always 1.                                                                                   |
|                 |                |           | | Coded as 8bit (1 byte) value.                                                                                                                   |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| SIGNATURE       | 0x02           | max 78    | | Signature of the Package.                                                                                                                       |
|                 |                |           | | This field has variable length depending on digital signature size.                                                                             |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| EVENT_ID        | 0x03           | 1         | | Event ID, see the list of all available event ID's in the table below.                                                                          |
|                 |                |           | | Coded as 8bit (1 byte) value.                                                                                                                   |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| EVENT_SOURCE    | 0x04           | 1         | | Not used. The value is always 0.                                                                                                                |
|                 |                |           | | Coded as 8bit (1 byte) value.                                                                                                                   |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| EVENT_DATE_TIME | 0x05           | 8         | | Event date and time. This field is `'Epoch time' but im milliseconds <https://en.wikipedia.org/wiki/Unix_time>`_.                               |
|                 |                |           | | Coded as 64bit (8 bytes) value.                                                                                                                 |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| DEVICE_ID       | 0x06           | 4         | | The Lock's 'Device ID' the event was generated on.                                                                                              |
|                 |                |           | | Coded as 32bit (4 bytes) value.                                                                                                                 |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| USER_ID         | 0x07           | 4         | | The User ID the event was generated by. This field is optional, and exists only for those events where User ID is relevant.                     |
|                 |                |           | | Coded as 32bit (4 bytes) value.                                                                                                                 |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| PIN_ALIAS       | 0x08           | max 32    | | PIN alias/name if event was generated from Keypad. This field is optional, and exists only for events generated from Keypad                     |
|                 |                |           | | This field has variable length depending on PIN alias length.                                                                                   |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+

| The layout of fields in an Activity Logs package looks as shown below:

.. code-block::

    VERSION,
    [EVENT_ID, EVENT_SOURCE, EVENT_DATE_TIME, DEVICE_ID, USER_ID(optional), PIN_ALIAS(optional)]
    [EVENT_ID, EVENT_SOURCE, EVENT_DATE_TIME, DEVICE_ID, USER_ID(optional), PIN_ALIAS(optional)]
    ...
    [EVENT_ID, EVENT_SOURCE, EVENT_DATE_TIME, DEVICE_ID, USER_ID(optional), PIN_ALIAS(optional)]
    SIGNATURE


| First is the VERSION tag, and last is the SIGNATURE tag. Those fields appears only ONCE in the entire package.
| Other fields, describing individual log records, may appear multiple times, but always in the groups as shown above.
| You can assume the EVENT_ID tag is the first field of each log record.


| The table below describes all possible events with their ID's

+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| ID (dec)   | ID (hex)   |           Name            |                                                    Description                                                     |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 32         | 0x20       | LockedRemote              | locked via mobile app                                                                                              |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 33         | 0x21       | UnlockedRemote            | unlocked via mobile app                                                                                            |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 34         | 0x22       | LockedButton              | locked by pressing the button on lock device                                                                       |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 35         | 0x23       | UnlockedButton            | unlocked by pressing the button on lock device                                                                     |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 36         | 0x24       | LockedAuto                | successfully performed auto lock feature                                                                           |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 37         | 0x25       | UnlockedAuto              | successfully performed auto unlock feature                                                                         |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 38         | 0x26       | LockedManual              | lock was rotated manually into locked position                                                                     |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 39         | 0x27       | UnlockedManual            | lock was rotated manually into unlocked position                                                                   |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 40         | 0x28       | Jammed                    | lock got stuck during locking/unlocking/pulling action                                                             |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 41         | 0x29       | PowerOff                  | lock registered long button push and it will power off                                                             |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 42         | 0x2A       | PowerOn                   | lock registered power up                                                                                           |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 43         | 0x2B       | Calibration               | user successfully calibrate or recalibrate the lock                                                                |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 46         | 0x2E       | BatteryCharging           | device detect start charging process                                                                               |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 47         | 0x2F       | PartiallyOpenManual       | user rotated the lock from locked or unlocked position into semi-locked position                                   |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 48         | 0x30       | PartiallyOpenButton       | operation of locking or unlocking performed from button failed and lock stopped in semi-locked position            |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 49         | 0x31       | PartiallyOpenAuto         | operation of auto locking failed and lock stopped in semi-locked position                                          |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 50         | 0x32       | BatteryStopCharging       | device detect stop charging process                                                                                |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 51         | 0x33       | PulledRemote              | spring was pulled via mobile app                                                                                   |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 52         | 0x34       | PulledAuto                | spring was pulled automatically after receiving auto unlock command from mobile app                                |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 53         | 0x35       | PulledManual              | lock was rotated manually to perform pull spring action                                                            |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 54         | 0x36       | PartiallyOpenRemote       | mobile app sent open or close request and received lock status changed to partially open                           |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 55         | 0x37       | PulledAutoByRemote        | mobile app sent auto unlock request when lock was already in unlocked position and only pull spring was performed  |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 56         | 0x38       | PostponedLock             | locked by pressing and holding the button on lock device                                                           |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 57         | 0x39       | UnlockedHomeKit           | unlocked via HomeKit app                                                                                           |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 58         | 0x3A       | PartiallyOpenHomeKit      | HomeKit app sent open or close request and received lock status changed to partially open                          |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 59         | 0x3B       | LockedHomeKit             | locked via HomeKit app                                                                                             |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 60         | 0x3C       | PulledHomeKit             | spring was pulled via HomeKit app                                                                                  |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 61         | 0x3D       | UnlockByPin               | unlocked from keypad by pin                                                                                        |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 62         | 0x3E       | IncorrectPin              | incorrect pin typen on keypad                                                                                      |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 63         | 0x3F       | PullSpringByPin           | keypad sent unlock request when pull spring is enabled and lock was open and only pull spring was performed        |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 64         | 0x40       | PartiallyOpenByPin        | keypad sent unlock request and received lock status changed to partially open                                      |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 65         | 0x41       | LockedByKeypadWithPin     | locked from keypad by pin                                                                                          |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 66         | 0x42       | LockedByKeypadWithoutPin  | locked from keypad by button (without pin)                                                                         |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 224        | 0xE0       | FirmwareUpdateByBridge    | device was updated by bridge                                                                                       |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 225        | 0xE1       | FirmwareUpdateByMobile    | device was updated by mobile app                                                                                   |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 226        | 0xE2       | LockedByAccessLink        | lock was locked via access link                                                                                    |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 227        | 0xE3       | UnlockedByBridgeApi       | lock was unlocked via bridge api                                                                                   |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 228        | 0xE4       | UnlockedByAccessLink      | lock was unlocked via access link                                                                                  |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 229        | 0xE5       | UnlockedByBridgeApi       | lock was unlocked via bridge api                                                                                   |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 230        | 0xE6       | PulledByAccessLink        | spring was pulled via access link                                                                                  |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 231        | 0xE7       | PulledByBridgeApi         | spring was pulled via bridge api                                                                                   |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 232        | 0xE8       | PartiallyOpenByAccessLink | access link sent unlock request and received lock status changed to partially open                                 |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 233        | 0xE9       | PartiallyOpenByBridgeApi  | bridge api sent unlock request and received lock status changed to partially open                                  |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 234        | 0xEA       | PulledAutoByAccessLink    | access link sent auto unlock request when lock was already in unlocked position and only pull spring was performed |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+
| 235        | 0xEB       | PulledAutoByBridgeApi     | bridge api sent auto unlock request when lock was already in unlocked position and only pull spring was performed  |
+------------+------------+---------------------------+--------------------------------------------------------------------------------------------------------------------+

