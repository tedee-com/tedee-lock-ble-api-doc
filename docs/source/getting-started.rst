Getting started
===============

Tedee Lock BLE API exposes resources that enable you to work with tedee lock. By calling relevant command user is able, among others, to manipulate lock, get battery level or read it's activities. This guide aims to help you to get started with Tedee Lock BLE API.

What you need?
--------------

Starting working with the lock's BLE API does require some prerequisites.
You'll need:

* Device that support's BLE 5.0 module to communicate with tedee lock correctly.
* :ref:`Security algorithms <ptls_algorithms>` support.

Registration and authentication
-------------------------------

Before you can use the BLE API you must establish PTLS session first. The process of establishing that session is described in dedicated :doc:`section <../howtos/establish-ptls-session>`.

BLE API commands
----------------

To interact with the Tedee Lock BLE API, you can send:

* :doc:`open lock <commands/operations/lock-open>`,
* :doc:`close lock <commands/operations/lock-close>`,
* :doc:`pull-spring <commands/operations/pull-spring>`.

Example request
^^^^^^^^^^^^^^^^

1. Form message for encryption,

+-------------------+-------------+
| **Command Value** | **param**   |
+-------------------+-------------+
| 0x51 (LOCK_OPEN)  | 0x00 (NONE) |
+-------------------+-------------+

2. :doc:`Encrypt <../../ptls/secured_communication>` prepared message,
3. Send it on :ref:`API commands characteristic <api_commands_characteristic>`,

BLE API Response
-----------------

On every command send on :ref:`API commands characteristic <api_commands_characteristic>` you will get response.
You can also receive data from lock asynchronuosly via :ref:`Notifications characteristic <notifications_characteristic>`.

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