Hash calculation
================

For hash calculation during PTLS session establishement we are using **SHA256**.

Every time hash calculation is mentioned it means:

- discard :doc:`message headers <../ble/message_frame>`
- calculate/update hash send and received packets till that stage of PTLS session establishement,
- calculate it before encryption or after decryption, depending on that message will be send or is received.