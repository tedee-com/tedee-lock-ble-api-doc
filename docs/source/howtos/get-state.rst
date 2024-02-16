How to get Lock State
=====================

| To send specific commands to get Lock State you need to :doc:`establish PTLS session <establish-ptls-session>`.

| After you have successfully established the PTLS session you need to turn on BLE indications on :ref:`API commands characteristic <api_commands_characteristic>`.

| You can get actual Lock state on demand by sending the :doc:`"GET_STATE" command described here <../commands/state/get-state>`.

| The command returns 2 bytes as a response, where first byte indicates the actual state of Lock, and second tells if the last operation was performed successfully.

| For example, the following response...

.. code-block::

    [0x06, 0x00]

| ... means:

    #. the lock is in "locked" position (0x06)
    #. last operation was successfull and the lock is not jammed (0x00).

.. note::
    | The Lock sends :doc:`"LOCK_STATUS_CHANGE" notification <../notifications/operations/lock-state>` every time its state changes. 
    | So, your system does not need to send GET_STATE command periodically if the notification is supported.

| Visit :doc:`"GET_STATE" command <../commands/state/get-state>` and :doc:`"LOCK_STATUS_CHANGE" notification <../notifications/operations/lock-state>` for details.