Pull spring calibration finished
================================

PULL_CALIBRATION_FINISHED code is 0xBF

Lock sends this notification when leaves calibration process of pull spring positions. 

Parameter
---------

| param[0]: result 
 
    * 0 - SUCCESS - lock successfully calibrated pull spring position.
    * 1 - ERROR - something unexpected happened. For example lock is jammed.
    * 3 - ERROR_TIMEOUT - Timeout in calibration process.
    * 4 - CANCELLED - Lock received `Calibration cancel <../../commands/pullcalibration/pull-calibration-cancel.html>`_ command.
