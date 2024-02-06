Calibration finished
====================

CALIBRATION_FINISHED code is 0xB3

Lock sends this notification when leaves calibration process of unlocked and locked positions. 

Parameter
---------

| param[0]: result 
 
    * 0 - SUCCESS - lock successfully calibrated unlocked and locked positions.
    * 1 - ERROR - something unexpected happened. For example lock is jammed.
    * 2 - ERROR_POSITIONS - Unlocked and locked positions are too close. Minimum acceptable distance is 60 degrees.
    * 3 - ERROR_TIMEOUT - Timeout in calibration process.
    * 4 - CANCELLED - Lock received `Calibration cancel <../../commands/calibration/calibration-cancel.html>`_ command.
