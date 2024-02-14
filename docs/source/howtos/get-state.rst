How to get actual Lock State on demand
======================================

| To send specific commands to get Lock State you need to :doc:`establish PTLS session <establish-ptls-session>`.

| After you have successfully established the PTLS session you need to turn on BLE indications on :ref:`API commands characteristic <api_commands_characteristic>`.

| You can get actual Lock state on demand by sending the :doc:`"GET_STATE" command described here <../commands/state/get-state>`.

| The command returns 2 bytes as a response, where first byte indicates the actual state of Lock, and second tells if the last operation was performed successfully.

| For example, the following response...

.. code-block::

    [0x06, 0x00]

| ... means:

    #. the lock is in "locked" position
    #. last operation was successfull and the lock is not jammed.

| Visit :doc:`"GET_STATE" command <../commands/state/get-state>` for details.