Message frame
==============

Each channel uses a frame to transmit messages. 
The frame consists of a header and data. 
The header is 1 additional byte (positioned before the data) that indicates the type of message being sent.
All fields/parameters are sent in **big-endian** format.

.. _message_headers:

Message headers
---------------

On characteristics: :ref:`notifications_characteristic` and :ref:`api_commands_characteristic`

.. note::

    Please ommit upper half byte of message header as it is reserved.

    .. code::

        data[0] = data[0] & 0x0F

+-----------------------+-------+---------------------------------------------------------------------------------------+
| Header                | Value | Description                                                                           |
+-----------------------+-------+---------------------------------------------------------------------------------------+
| DATA_NOT_ENCRYPTED    | 0x00  | | Indicates a message that contains unencrypted data.                                 |
|                       |       | | The header is used by several special API commands described below.                 |
+-----------------------+-------+---------------------------------------------------------------------------------------+
| DATA_ENCRYPTED        | 0x01  | Indicates a message that contains encrypted data using PTLS session.                  |
+-----------------------+-------+---------------------------------------------------------------------------------------+

On characteristics: :ref:`ptls_tx_characteristic` and :ref:`ptls_rx_characteristic`

+-----------------------+-------+---------------------------------------------------------------------------------------+
| Header                | Value | Description                                                                           |
+-----------------------+-------+---------------------------------------------------------------------------------------+
| PTLS_HELLO            | 0x03  | Indicates a "hello" message of the PTLS protocol when establishing the session.       |
+-----------------------+-------+---------------------------------------------------------------------------------------+
| PTLS_ALERT            | 0x04  | | Indicates an "alert" message of the PTLS protocol, during the session establishment |
|                       |       | | and secure communication. It is used to inform the other side of communication      |
|                       |       | | about an error. Message parameters are error codes (described below)                |
+-----------------------+-------+---------------------------------------------------------------------------------------+
| PTLS_SERVER_VERIFY    | 0x05  | | Indicates a message verifying the "server" side in the PTLS protocol                |
|                       |       | | during session establishment.                                                       |
+-----------------------+-------+---------------------------------------------------------------------------------------+
| PTLS_CLIENT_VERIFY_I  | 0x06  | | Indicates a message verifying the "client" side in the PTLS protocol                |
|                       |       | | during session establishment. Stage 1                                               |
+-----------------------+-------+---------------------------------------------------------------------------------------+
| PTLS_CLIENT_VERIFY_II	| 0x07  | | Indicates a message verifying the "client" Side in the PTLS protocol                |
|                       |       | | during session establishment. Stage 2                                               |
+-----------------------+-------+---------------------------------------------------------------------------------------+
| PTLS_INITIALIZED      | 0x08  | | Indicates a message informing the other side that the PTLS session                  |
|                       |       | | was established successfully.                                                       |
+-----------------------+-------+---------------------------------------------------------------------------------------+
