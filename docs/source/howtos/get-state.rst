How to get actual Lock State on demand
======================================

To send specific commands to get Lock State you need to :doc:`establish PTLS session <establish-ptls-session>`.

After you have successfully established the PTLS session you need to turn on BLE indications on :ref:`API commands characteristic <api_commands_characteristic>`.

You can get actual Lock state on demand by sending the :doc:`"GET_STATE" command described here <../commands/state/get-state>`.