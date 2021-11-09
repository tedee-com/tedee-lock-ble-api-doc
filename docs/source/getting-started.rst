Getting started
===============

Tedee Lock BLE API exposes resources that enable you to work with tedee lock. By calling relevant commands the user is able, among others, to manipulate lock, get battery level or read its activities. This guide aims to help you to get started with Tedee Lock BLE API.

What do you need?
-----------------

Starting working with the lock's BLE API does require some prerequisites.
You'll need:

* Device that supports BLE 5.0 module to communicate with tedee lock correctly.
* :ref:`Security algorithms <ptls_algorithms>` support.

Registration and authentication
-------------------------------

Before you can use the BLE API you must:

#. `Register device on backend <https://tedee-tedee-api-doc.readthedocs-hosted.com/en/latest/howtos/connect-device-via-ble.html>`_. 
#. Establish PTLS session. The process of establishing that session is described in dedicated :doc:`section <../howtos/establish-ptls-session>`.

BLE API commands
----------------

To interact with the Tedee Lock BLE API, you can send the following commands:

* :doc:`unlock <commands/operations/unlock>`,
* :doc:`lock <commands/operations/lock>`,
* :doc:`pull spring <commands/operations/pull-spring>`.

Example request
^^^^^^^^^^^^^^^^

1. Form message for encryption,

+--------------------+-------------+
| **Command Value**  | **param**   |
+--------------------+-------------+
| 0x51 (UNLOCK_LOCK) | 0x00 (NONE) |
+--------------------+-------------+

2. :doc:`Encrypt <../../ptls/secured_communication>` prepared message,
3. Send it on :ref:`API commands characteristic <api_commands_characteristic>`,

BLE API Response
-----------------

On every command send on :ref:`API commands characteristic <api_commands_characteristic>` you will get a response.
You can also receive data from lock asynchronously via :ref:`Notifications characteristic <notifications_characteristic>`.

.. note::

    Please omit the upper half byte of the message header as it is reserved on :ref:`API commands characteristic <api_commands_characteristic>` and :ref:`Notifications characteristic <notifications_characteristic>`.

    .. code::

        data[0] = data[0] & 0x0F


Response BLE frame
^^^^^^^^^^^^^^^^^^
+--------------------+---------+
| **Message header** | Content |
+--------------------+---------+
| DATA_NOT_ENCRYPTED | x bytes |
+--------------------+---------+

+--------------------+-------------------+-----------------------------------+
| **Message header** | Encrypted content | MAC (Message authentication code) |
+--------------------+-------------------+-----------------------------------+
| DATA_ENCRYPTED     | x bytes           | 16 bytes                          |
+--------------------+-------------------+-----------------------------------+

Example response
^^^^^^^^^^^^^^^^

1. Receive response on :ref:`API commands characteristic <api_commands_characteristic>`,
2. :doc:`Decrypt <../../ptls/secured_communication>` received response discarding :ref:`first header byte <message_headers>`,
3. Parse response

+----------------+
| **Response**   |
+----------------+
| 0x00 (SUCCESS) |
+----------------+