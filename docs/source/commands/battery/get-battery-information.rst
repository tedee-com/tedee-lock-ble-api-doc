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