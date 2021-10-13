PTLS client verification
========================

Due to limitation of Bluetooth package size on external device, client verify is send in at least two parts. 
Connected device will send each part without waiting for response from Tedee Lock, only after last part, device will receive response from lock.

.. _client_ask_for_verification:

1. Client ask for verification
------------------------------

    #. Get permission certificate via `Tedee API <https://api.tedee.com/>`_
    #. Decode certificate from Base64.
    #. Attach certificate length on 2 bytes.
    #. Attach certificate to the message.
    #. :doc:`Update hash <hash_calculation>` by following frame:

    +-----------------+----------------------+
    | Certificate_len | Certificate_data     |
    +-----------------+----------------------+
    | 2 bytes         | Certificate_len      |
    +-----------------+----------------------+

    6. Generate signature from above hash using **Device Private Key**.
    #. Attach signature to the message.
    #. Attach hash from last step of :ref:`client_server_verification` to the message.
    #. Calculate HMAC from a shared secret, *ptlsc hs traffic* and last hash from hello handshake.
    #. :doc:`Update hash <hash_calculation>` by rest of message and save it:

+---------------+---------------+--------------------+
| Signature_len | Signature     | Server verify hash |
+---------------+---------------+--------------------+
| 2 bytes       | signature_len | 32 bytes           |
+---------------+---------------+--------------------+

    11. Encrypt message using AEAD algorithm (AES GCM 128bit).
    #. Send message in two parts:
        #. With header PTLS_CLIENT_VERIFY_I with up to MTU_SIZE bytes.
        #. Last part with header PTLS_CLIENT_VERIFY_II. 

    Message format before encryption:

+-----------------+----------------------+---------------+---------------+--------------------+
| Certificate_len | Certificate_data     | Signature_len | Signature     | Server verify hash |
+-----------------+----------------------+---------------+---------------+--------------------+
| 2 bytes         | Certificate_len      | 2 bytes       | signature_len | 32 bytes           |
+-----------------+----------------------+---------------+---------------+--------------------+

2. Server verifies client
-------------------------

Response after successfull verification.

+-------------------------+------------+
| PTLS_INITIALIZED (0x08) | Session ID |
+-------------------------+------------+
| 2 bytes                 | 4 bytes    |
+-------------------------+------------+

3. Client handle final response
-------------------------------

    #. Receive PTLS_INITIALIZED frame and save received client session ID (**4bytes**).
    #. Initialize session object and variables:
        #. Calculate HMAC from a shared secret, '*ptlsc ap traffic*' and hash after whole PTLS process (step 10 of :ref:`client_ask_for_verification`).
        #. From above HMAC use first 16 bytes as encryption key and next 12 bytes as iv for encryption process. Save two last bytes of iv for further communication.
        #. Calculate HMAC from a shared secret, '*ptlss ap traffic*' and hash after whole PTLS process (step 10 of :ref:`client_ask_for_verification`). 
        #. From above HMAC use first 16 bytes as decryption key and next 12 bytes as iv for decryption process. Save two last bytes of iv for further communication.
        #. Init send and received message counters (**2bytes** each) to 0.