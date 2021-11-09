How to find specific Tedee lock
===============================

Serial Number
-------------

To find a specific lock you need to have the Serial Number of the Tedee lock.

Serial Number can be obtained in two ways:
    #. Manually from the sticker on the Tedee Lock box.
    #. Using our `Tedee API <https://api.tedee.com/>`_ to get Serial Number via Activation Code of the Tedee lock.
    
Activation Code can be found as a **QR code** as well as a **string** on the back of the lock or the leaflet inside the Tedee Lock box.

When you have a Serial Number you can start scanning for BLE advertising packets and find such device that advertises :doc:`Serial Number service <../ble/services>`.

Example
-------

Let's say we have a lock with a Serial number.

**12345678-901234**

The service that you should look for in the advertising packet looks like this:

**12340000-5678-9012-3400-000000000000**
