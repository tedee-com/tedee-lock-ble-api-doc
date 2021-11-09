Hash calculation
================

For hash calculation during PTLS session establishment is used **SHA256**.

Every time hash calculation is mentioned it means:

- discard :doc:`message headers <../ble/message_frame>`
- calculate/update hash of send and received packets till that stage of PTLS session establishment,
- calculate it before encryption or after decryption, depending on the message that will be sent or was just received.