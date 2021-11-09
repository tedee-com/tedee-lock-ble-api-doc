BLE basics
==========

To communicate with our lock you need a device that supports wireless BLE connectivity.

Requirements
------------

- BLE 5.0

Preferred Connection Parameters
-------------------------------

- MTU size should be as much as possible ideally **247 bytes**.
- Connection interval set in range **7.5ms - 60ms**.
- Slave latency **0**
- Supervision timeout **4s**.

| Lock communicates using **1Mbps** physical layer.
| Lock allows for 3 simultaneous BLE connections.

.. note::

    - When the connection is established (open Bluetooth communication), the Tedee lock does not send any information (it does not start transmitting any data, including notifications).
    - Connected device should initiate the PTLS session by sending a "hello" message within **10s** (if not, Lock will disconnect).
