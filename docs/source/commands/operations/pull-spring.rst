Pull spring
===========

PULL_SPRING code is 0x52

Commands tells lock to just pull the spring.

Input parameters
----------------
none

Result
------
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| **Result name**                          | **Value** | **Description**                                                         |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| SUCCESS                                  | 0x00      | Operation accepted.                                                     |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| ERROR                                    | 0x02      | Error occured.                                                          |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| BUSY                                     | 0x03      | Lock is currently performing other operation. Wait for change state.    |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| NOT_CALIBRATED                           | 0x05      | Lock do not have calibration. Please calibrate the lock.                |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| NOT_CONFIGURED                           | 0x08      | Lock auto pull spring feature is turned off.                            |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| DISMOUNTED                               | 0x09      | Lock is not mounted on doors.                                           |
+------------------------------------------+-----------+-------------------------------------------------------------------------+

Output parameters
-----------------
none

Example
-------

1. Form message for encryption,

+-------------------+
| **Command Value** |
+-------------------+
| 0x52              |
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