PTLS server verification
========================

The client side is an external device that is connected to the lock and wants to establish a PTLS session.
The server side is a Tedee lock.

.. _client_challange_server_verify:

1. Client challenge server verification
---------------------------------------

    #. Prepare *Auth data* as a current datetime in ms (**8bytes**),
    #. :doc:`Calculate hash <hash_calculation>` from:

        +-----------------+----------------------+
        | Auth_data_len   | Auth_data (DT in ms) |
        +-----------------+----------------------+
        | 2 bytes         | 8 bytes              |
        +-----------------+----------------------+

    3. Send a message called *Server verify* to get a signature generated by a server side.
    
+-----------------+----------------------+
| PTLS_SRV_VERIFY | Auth_data (DT in ms) |
+-----------------+----------------------+
| (0x05) 1 byte   | 8 bytes              |
+-----------------+----------------------+

.. _server_response:

2. Server response
------------------

    The response is encrypted using the AEAD algorithm (AES GCM 128bit) with header PTLS_SRV_VERIFY.
    Message before encryption:

+-----------------+----------------------+--------+---------------+---------------+--------+---------------------+------------+
| Auth_data_len   | Auth data (DT in ms) | 0x00   | Signature_len | Signature     | 0x00   | Hello_hash_len 0x20 | Hello_hash |
+-----------------+----------------------+--------+---------------+---------------+--------+---------------------+------------+
| 2 bytes         | 8 bytes              | 1 byte | 1 byte        | signature_len | 1 byte | 1 byte              | 32 bytes   |
+-----------------+----------------------+--------+---------------+---------------+--------+---------------------+------------+

.. _client_server_verification:

3. Client server verification
-----------------------------

    #. Calculate HMAC from a shared secret, *ptlss hs traffic* and last hash from hello handshake (:ref:`client_hello_final`).
    #. The first 16 bytes of HMAC are used as the decryption key.
    #. The next 12 bytes of HMAC are used as iv vector.
    #. Decrypt a message using the AEAD algorithm (AES GCM 128bit).
    #. Check if the hash in the decrypted message is the same as after the last hello message (:ref:`client_hello_final`).
    #. Verify compliance of the verification data (*auth data*).
    #. Verify signature from hash in 2. point of :ref:`client_challange_server_verify` using *server* public key (got from (`Tedee API <https://tedee-tedee-api-doc.readthedocs-hosted.com/en/latest/howtos/connect-device-via-ble.html#step-3-get-certificate-for-mobile-device>`_)).
    #. :doc:`Calculate hash <hash_calculation>` from:
    
+--------+---------------+---------------+--------+---------------------+------------+
| 0x00   | Signature_len | Signature     | 0x00   | Hello_hash_len 0x20 | Hello_hash |
+--------+---------------+---------------+--------+---------------------+------------+
| 1 byte | 1 byte        | signature_len | 1 byte | 1 byte              | 32 bytes   |
+--------+---------------+---------------+--------+---------------------+------------+