Tedee lock BLE services
========================

.. _tedee_lock_service:

Tedee Lock's service
--------------------

    *00000002-4899-489f-a301-fbee544b1db0*

    It's the most important service which contains all characteristics for direct communication with lock.
    In that service you find characteristics needed for establishing PTLS session as well as for communication after that.

.. _serial_number_service:

Serial Number service
---------------------

    *xxxx0000-xxxx-xxxx-xx00-000000000000*

    The only purpose of this service is to present device serial number for identification purposes. No communication is implement using this service.

Other services that are forbidden for usage.