Tedee Lock BLE advertisement
============================

In general BLE advertising consist of two packets being sent via the peripheral device.
Each packet contains 31 bytes together with BLE headers.

#. Main advertising packet.
#. Scan response packet.

Main advertising packet
------------------------------

The main packet consist of unique UUID of our devices:

    :ref:`serial_number_service` UUID that specifies particular Tedee Lock 

         *xxxx0000-xxxx-xxxx-xx00-000000000000*

    where each **x** is a digit from a serial number of a particular lock.

Scan response packet
--------------------

Scan response packet consists of:

    1. :ref:`tedee_lock_service` UUID that's the same for all Tedee Lock's

        *00000002-4899-489f-a301-fbee544b1db0*
        
    #. Lock name in the following format:
        Lock-**YYZZ**

    where **YYZZ** are two hex bytes to differentiate locks names in the BLE environment.
