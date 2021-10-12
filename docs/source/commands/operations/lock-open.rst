Open lock
=========

OPEN_LOCK code is 0x51.

Command is used to open the lock.

Input parameters
----------------
param[0]: if not attached to command then default value is **NONE**.

+----------------+-----------+----------------------------------------------+
| **Param name** | **Value** | **Description**                              |
+----------------+-----------+----------------------------------------------+
| NONE           | 0x00      | Unlock lock                                  |
+----------------+-----------+----------------------------------------------+
| AUTO           | 0x01      | Unlock from auto unlock feature.             |
+----------------+-----------+----------------------------------------------+
| FORCE          | 0x02      | | Forces lock to unlock lock till jamm.      |
|                |           | | **Should be used only in emergency case**. |
+----------------+-----------+----------------------------------------------+
		
Result
------
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| **Result name**                          | **Value** | **Description**                                                         |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| SUCCESS                                  | 0x00      | Operation accepted.                                                     |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| INVALID_PARAM                            | 0x01      | Invalid params passed to lock.                                          |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| ERROR                                    | 0x02      | Error occured.                                                          |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| BUSY                                     | 0x03      | Lock is currently performing other operation. Wait for change state.    |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| NOT_CALIBRATED                           | 0x05      | Lock do not have calibration. Please calibrate the lock.                |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| ALREADY_CALLED_BY_AUTOUNLOCK             | 0x06      | | Last unlock operation was auto unlock and it happened < 3min          |
|                                          |           | | (current lock state does not matter).                                 |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| ALREADY_CALLED_BY_OTHER_OPERATION        | 0x0A      | | Last unlock operation was different than auto unlock                  |
|                                          |           | | and it happened < 3min (current lock state does not matter).          |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| NOT_CONFIGURED                           | 0x08      | Lock auto pull spring feature is turned off.                            |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| DISMOUNTED                               | 0x09      | Lock is not mounted on doors.                                           |
+------------------------------------------+-----------+-------------------------------------------------------------------------+

Output parameters
----------------- 

Output parameter will indicate number of seconds since last "open lock" operation. 
The value is passed as 4 bytes in Big-endian format. (**UNLOCK_ALREADY_CALLED_BY_AUTOUNLOCK** and **UNLOCK_ALREADY_CALLED_BY_OTHER_OPERATION**).
