Get battery information
=======================

GET_BATTERY code is 0x0C

Gets battery level and charging status. Battery level is as percentage.

Input parameters
----------------
none

Result
------
+-----------------+-----------+---------------------+
| **Result name** | **Value** | **Description**     |
+-----------------+-----------+---------------------+
| SUCCESS         | 0x00      | Command accepted.   |
+-----------------+-----------+---------------------+

Output parameters
-----------------
| param[0]: battery level in range between 0 and 100 (%)
| param[1]: battery charging status

    * 0 - discharging
    * 1 - charging

Example
-------

1. Form message for encryption,

+-------------------+
| **Command Value** |
+-------------------+
| 0x0C              |
+-------------------+

2. :doc:`Encrypt <../../ptls/secured_communication>` prepared message,
3. Send it on :ref:`API commands characteristic <api_commands_characteristic>`,
4. Receive response on :ref:`API commands characteristic <api_commands_characteristic>`,
5. :doc:`Decrypt <../../ptls/secured_communication>` received response discarding :ref:`first header byte <message_headers>`,
6. Parse response

+----------------+----------+----------+
| **Result**     | param[0] | param[1] |
+----------------+----------+----------+
| 0x00 (SUCCESS) | 0 - 100% | 0 or 1   |
+----------------+----------+----------+