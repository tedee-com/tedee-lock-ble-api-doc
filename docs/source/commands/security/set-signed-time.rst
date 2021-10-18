Set signed time
===============

Sets trusted date and time on the device got from `Tedee API <https://tedee-tedee-api-doc.readthedocs-hosted.com/en/latest/endpoints/datetime/get-signed-time.html>`_.

SET_SIGNED_DATETIME code is 0x71

Input parameters
----------------

param[0 .. 7]: datetime received from `Tedee API <https://tedee-tedee-api-doc.readthedocs-hosted.com/en/latest/endpoints/datetime/get-signed-time.html>`_ and decoded from base64,
param[8 .. signature_len+7]: signature received from `Tedee API <https://tedee-tedee-api-doc.readthedocs-hosted.com/en/latest/endpoints/datetime/get-signed-time.html>`_.

Result
------

+------------------------------------------+-----------+-------------------------------------------------------------------------+
| **Result name**                          | **Value** | **Description**                                                         |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| SUCCESS                                  | 0x00      | Operation accepted. Wait for notification signed datetime.              |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| INVALID_PARAM                            | 0x01      | Invalid params passed to lock.                                          |
+------------------------------------------+-----------+-------------------------------------------------------------------------+
| ERROR                                    | 0x02      | Error occured.                                                          |
+------------------------------------------+-----------+-------------------------------------------------------------------------+

Output params
-------------
none

Example
-------

#. Get signed time from `Tedee API <https://tedee-tedee-api-doc.readthedocs-hosted.com/en/latest/endpoints/datetime/get-signed-time.html>`_.
#. Decode datetime and signature from base64.
#. Create such frame:

+----------+---------------+
| Datetime | Signature     |
+----------+---------------+
| 8 bytes  | Signature_len |
+----------+---------------+

4. :doc:`Encrypt <../../ptls/secured_communication>` prepared message,
5. Send it on :ref:`API commands characteristic <api_commands_characteristic>`,
6. Receive response on :ref:`API commands characteristic <api_commands_characteristic>`,
7. Response is set as :ref:`not encrypted <message_headers>` so just discard that header byte,
8. Parse response

+----------------+
| **Response**   |
+----------------+
| 0x00 (SUCCESS) |
+----------------+

9. Wait for :doc:`SIGNED_DATETIME <../../notifications/security/signed-datetime>` notification.
