PTLS hello messages
===================

Client side is an external device that is connected to lock and want to establish PTLS session.
Server side is a Tedee lock.

1. Client hello
---------------
    
    #. Set PTLS_VERSION to 0x02 value,
    #. Get BLE connection MTU size,
    #. Generate **random** data (**32 bytes**),
    #. Acquire client **ECDH public key** (**65 bytes**),
    #. Concatenate above to form Hello message frame:
    
+---------------+----------+----------+------------------+----------------------+-----------------+
| PTLS_VERSION  | MTU_SIZE | RESERVED | RANDOM_DATA (1.) | PUBLIC_ECDH_Key (2.) | PTLS_CACHE_DATA |
+---------------+----------+----------+------------------+----------------------+-----------------+
| (0x02) 1 byte | 1 byte   | 1 byte   | 32 bytes         | 65 bytes             | 52 bytes        |
+---------------+----------+----------+------------------+----------------------+-----------------+
    
    6. :doc:`Calculate hash <hash_calculation>` from above frame,
    #. Send it to Tedee Lock with PTLS_HELLO header.

    where:

    - PTLS_CACHE_DATA - is used during establishement of PTLS session from cached data after previous successfull PTLS session. For new session it should be 52bytes of zeros.

.. _server_hello_response:

2. Server hello reponse
-----------------------

Unencrypted response:

+---------------+---------------+----------+----------+------------------+----------------------+
| PTLS_HELLO    | PTLS_VERSION  | MTU_SIZE | RESERVED | RANDOM_DATA (1.) | PUBLIC_ECDH_KEY (2.) |
+---------------+---------------+----------+----------+------------------+----------------------+
| (0x03) 1 byte | (0x02) 1 byte | 1 byte   | 1 byte   | 32 bytes         | 65 bytes             |
+---------------+---------------+----------+----------+------------------+----------------------+

.. _client_hello_final:

3. Client hello final
---------------------

    #. Receive :ref:`hello response <server_hello_response>` from server.
    #. :doc:`Calculate hash <hash_calculation>` after discarding PTLS_HELLO header.
    #. Calculate shared secret using PUBLIC_ECDH_KEY and ECDH algorithm.
