Battery state
=============

BATTERY_STATE code is 0xA0

Lock sends this notification whenever battery level changed.

Parameters
----------

| param[0]: battery level in range between 0 and 100 (%)
| param[1]: battery charging status

    * 0 - discharging
    * 1 - charging