Lock
=====

LOCK_COMMAND code is 0x50

The command is used to lock the lock.

Input parameters
----------------
param[0]: if not attached to the command then the default value is **NONE**.

+----------------+-----------+----------------------------------------------+
| **Param name** | **Value** | **Description**                              |
+----------------+-----------+----------------------------------------------+
| NONE           | 0x00      | Lock the lock                                |
+----------------+-----------+----------------------------------------------+
| FORCE          | 0x02      | | Forces to lock till jam.                   |
|                |           | | **Should be used only in emergency case**. |
+----------------+-----------+----------------------------------------------+

Result
------
+------------------------------------------+-----------+-----------------------------------------------------------------------------+
| **Result name**                          | **Value** | **Description**                                                             |
+------------------------------------------+-----------+-----------------------------------------------------------------------------+
| SUCCESS                                  | 0x00      | Operation accepted.                                                         |
+------------------------------------------+-----------+-----------------------------------------------------------------------------+
| INVALID_PARAM                            | 0x01      | Invalid params passed to lock.                                              |
+------------------------------------------+-----------+-----------------------------------------------------------------------------+
| ERROR                                    | 0x02      | Error occured.                                                              |
+------------------------------------------+-----------+-----------------------------------------------------------------------------+
| BUSY                                     | 0x03      | Lock is currently performing other operations. Wait for changing state.     |
+------------------------------------------+-----------+-----------------------------------------------------------------------------+
| NOT_CALIBRATED                           | 0x05      | Lock does not have calibration. Please calibrate the lock.                  |
+------------------------------------------+-----------+-----------------------------------------------------------------------------+
| DISMOUNTED                               | 0x09      | Lock is not mounted on doors.                                               |
+------------------------------------------+-----------+-----------------------------------------------------------------------------+

Output parameters
-----------------
none

Example
-------

1. Form message for encryption,

+-------------------+
| **Command Value** |
+-------------------+
| 0x50              |
+-------------------+

2. :doc:`Encrypt <../../ptls/secured_communication>` prepared message,
3. Send it on :ref:`API commands characteristic <api_commands_characteristic>`,
4. Receive response on :ref:`API commands characteristic <api_commands_characteristic>`,
5. :doc:`Decrypt <../../ptls/secured_communication>` received response discarding :ref:`first header byte <message_headers>`,
6. Parse response

+----------------+
| **Response**   |
+----------------+
| 0x00 (SUCCESS) |
+----------------+