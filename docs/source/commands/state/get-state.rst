Get Lock State
==============

GET_LOGS_TLV code is 0x5A

Gets actual Lock State on demand.


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
| param[0]: state

    * 0x00 - UNCALIBRATED - the Lock is not calibrated
    * 0x01 - CALIBRATION - the Lock is in calibration state
    * 0x02 - OPEN - the Lock is unlocked
    * 0x03 - PARTIALLY_OPEN - the Lock is partially unlocked
    * 0x04 - OPENING - the Lock is opening (changing state from Closed to Open)
    * 0x05 - CLOSING - the Lock is closing (changing state from Opened to Close)
    * 0x06 - CLOSED - the Lock is locked
    * 0x07 - PULL_SPRING - the Lock is pulling the spring (not moving)
    * 0x08 - PULLING - the Lock is moving towards "pull spring" position
    * 0x09 - UNKNOWN - the Lock is in unknown state

| param[1]: jammed

    * 0x00 - Not jammed - the lock is not jammed
    * 0x01 - Is jammed- the lock has jammed