Calibrate closed position
=========================

CALIBRATE_CLOSED code is 0x54

Lock saves position for closed state.

Input parameters
----------------
none

Result
------
+-----------------+-----------+-------------------------------------------------+
| **Result name** | **Value** | **Description**                                 |
+-----------------+-----------+-------------------------------------------------+
| SUCCESS         | 0x00      | Command accepted.                               |
+-----------------+-----------+-------------------------------------------------+
| ERROR           | 0x02      | | Command rejected.                             |
|                 |           | Lock is not mounted or not in calibration state |
+-----------------+-----------+-------------------------------------------------+
| BUSY            | 0x03      | | Command rejected.                             |
|                 |           | Lock is performing operation                    |
+-----------------+-----------+-------------------------------------------------+

Output parameters
-----------------
none

Example
-------

1. Form message for encryption,

+-------------------+
| **Command Value** |
+-------------------+
| 0x54              |
+-------------------+

2. :doc:`Encrypt <../../ptls/secured_communication>` prepared message,
3. Send it on :ref:`API commands characteristic <api_commands_characteristic>`,
4. Receive response on :ref:`API commands characteristic <api_commands_characteristic>`,
5. :doc:`Decrypt <../../ptls/secured_communication>` received response discarding :ref:`first header byte <message_headers>`,
6. Parse response

+----------------+
| **Result**     |
+----------------+
| 0x00 (SUCCESS) |
+----------------+