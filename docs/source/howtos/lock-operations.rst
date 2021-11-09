How to operate your Tedee lock
==============================

To send specific commands to lock about locking or unlocking you need to :doc:`establish PTLS session <establish-ptls-session>`.

After you have successfully established the PTLS session you need to turn on BLE indications on :ref:`API commands characteristic <api_commands_characteristic>`.

.. note::
    Your lock should be calibrated already before using these endpoints.

You can perform the following actions on lock:

| 1. Using :doc:`Unlock command <../commands/operations/unlock>` you can unlock your lock. 
| In the specific case, you are also able to perform pull spring.
| 
| 2. Using :doc:`Lock command <../commands/operations/lock>` you can unlock your lock.
| 3. Using :doc:`Pull spring command <../commands/operations/pull-spring>` you can pull the spring from an unlocked lock.

When you formed command to be send then :doc:`encrypt <../ptls/secured_communication>` and send on :ref:`API commands characteristic <api_commands_characteristic>`.

Each action can be performed only in specific lock states. Here is the Lock state diagram:

.. image:: ../images/lock-states-diagram.png
    :align: center
    :alt: lock states diagram

| Lock states presented with blue fields and white letters are stable states. 
| States showed using grey fields and blue letters are temporary states.