Set signed time
===============

Sets trusted date and time on the device got from `Tedee API <https://api.tedee.com/>`_.

SET_SIGNED_DATETIME code 0x71

#. Decode datetime and signature from base64.
#. Create such frame:

+----------+---------------+
| Datetime | Signature     |
+----------+---------------+
| 8 bytes  | Signature_len |
+----------+---------------+

3. Send on :ref:`api_commands_characteristic` with :doc:`unencrypted header <../../ble/message_frame>`.

Possible results:

+--------------------------+------+
| API_RESULT_SUCCESS       | 0x00 |
+--------------------------+------+
| API_RESULT_INVALID_PARAM | 0x01 |
+--------------------------+------+
| API_RESULT_ERROR         | 0x02 |
+--------------------------+------+
