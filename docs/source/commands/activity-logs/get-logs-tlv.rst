Get Activity Logs in TLV format
===============================

GET_LOGS_TLV code is 0x2D

| This command is used to collect a package of Activity Logs from Lock in a TLV format.
| More about binary format `Type–Length–Value can be found here <https://en.wikipedia.org/wiki/Type%E2%80%93length%E2%80%93value>`_.


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
| VERSION         | 1              | 1         | | Version of the package format. The value of this is always 1.                                                                                   |
|                 |                |           | | Coded as 8bit (1 byte) value.                                                                                                                   |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| SIGNATURE       | 2              | max 78    | | Signature of the Package.                                                                                                                       |
|                 |                |           | | This field has variable length depending on digital signature size.                                                                             |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| EVENT_ID        | 3              | 1         | | Event ID, `see the list of all available events <https://tedee-tedee-api-doc.readthedocs-hosted.com/en/latest/enums/event-type.html>`_          |
|                 |                |           | | Coded as 8bit (1 byte) value.                                                                                                                   |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| EVENT_SOURCE    | 4              | 1         | | Not used. The value is always 0.                                                                                                                |
|                 |                |           | | Coded as 8bit (1 byte) value.                                                                                                                   |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| EVENT_DATE_TIME | 5              | 8         | | Event date and time. This field is `'Epoch time' but im milliseconds <https://en.wikipedia.org/wiki/Unix_time>`_.                               |
|                 |                |           | | Coded as 64bit (8 bytes) value.                                                                                                                 |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| DEVICE_ID       | 6              | 4         | | The Lock's 'Device ID' the event was generated on.                                                                                              |
|                 |                |           | | Coded as 32bit (4 bytes) value.                                                                                                                 |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| USER_ID         | 7              | 4         | | The User ID the event was generated by. This field is optional, and exists only for those events where User ID is relevant.                     |
|                 |                |           | | Coded as 32bit (4 bytes) value.                                                                                                                 |
+-----------------+----------------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
| PIN_ALIAS       | 8              | max 32    | | PIN alias/name if event was generated from Keypad. This field is optional, and exists only for events generated from Keypad                     |
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

