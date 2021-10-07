PTLS secured communication
==========================

After successfull PTLS session establishement you can start communicate with lock using encrypted channel.

.. _send_encrypted_message:

Send encrypted message
----------------------

| For sending encrypted messages to Tedee lock use :ref:`api_commands_characteristic` characteristic. 
| Every encrypted message should have :doc:`DATA_ENCRYPTED <../ble/message_frame>` header attached as a first byte of the message.

To encrypt message use encryption key and iv got in last step of :doc:`client_verification` stage.
Before encryption of every message modify last two bytes of iv array via XOR with counter of send messages to lock:

.. code::

    iv[10] = iv_base[0] ^ (counter >> 8)
    iv[11] = iv_base[1] ^ (counter & 0xFF)

Encrypt message using AEAD algorithm (AES GCM 128bit).

Increment counter after every encrypted message sent.

Decryption of received messages
-------------------------------

| You will receive encrypted responses on :ref:`api_commands_characteristic` and :ref:`notifications_characteristic` characteristic.
| You can recognize encrypted message via :doc:`DATA_ENCRYPTED <../ble/message_frame>` header attached as a first byte of the message.

It is done is same way as described in :ref:`send_encrypted_message` using decryption key and iv got in last step of :doc:`client_verification` stage.

Increment received messages counter after every decrypted message.
