Get Lock State
==============

GET_LOGS_TLV code is 0x5A

Gets actual Lock State.


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
| param[0]: Actual state

+--------------------+-----------+-------------------------------------------------------------------------------+
| **State name**     | **Value** | **Description**                                                               |
+--------------------+-----------+-------------------------------------------------------------------------------+
| UNCALIBRATED       | 0x00      | Lock does not have any calibration.                                           |
+--------------------+-----------+-------------------------------------------------------------------------------+
| CALIBRATION        | 0x01      | Lock is in calibration mode.                                                  |
+--------------------+-----------+-------------------------------------------------------------------------------+
| UNLOCKED           | 0x02      | Lock is in calibrated unlocked position.                                      |
+--------------------+-----------+-------------------------------------------------------------------------------+
| PARTIALLY_UNLOCKED | 0x03      | Lock is in the middle of calibrated locked/unlocked positions.                |
+--------------------+-----------+-------------------------------------------------------------------------------+
| UNLOCKING          | 0x04      | Lock is rotating to an unlocked position.                                     |
+--------------------+-----------+-------------------------------------------------------------------------------+
| LOCKING            | 0x05      | Lock is rotating to a locked position.                                        |
+--------------------+-----------+-------------------------------------------------------------------------------+
| LOCKED             | 0x06      | Lock is in the calibrated locked position.                                    |
+--------------------+-----------+-------------------------------------------------------------------------------+
| PULL_SPRING        | 0x07      | Lock is in calibrated pull spring position.                                   |
+--------------------+-----------+-------------------------------------------------------------------------------+
| PULLING            | 0x08      | Lock is rotating to pull spring position.                                     |
+--------------------+-----------+-------------------------------------------------------------------------------+
| UNKNOWN            | 0x09      | Lock lost angular position but knows directions of unlocked/locked positions. |
+--------------------+-----------+-------------------------------------------------------------------------------+

| param[1]: Last lock state change status

+----------------------+-----------+-------------------------------------------+
| **Status name**      | **Value** | **Description**                           |
+----------------------+-----------+-------------------------------------------+
| STATUS_OK            | 0x00      | Last lock state change without issues.    |
+----------------------+-----------+-------------------------------------------+
| STATUS_ERROR_JAMMED  | 0x01      | Lock jammed during last state change.     |
+----------------------+-----------+-------------------------------------------+
