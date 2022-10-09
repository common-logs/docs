---
layout: post
title: "Writing CAN BSP Driver using CAN Framework in RTEMS"
date:   2022-10-05 12:15:18 +0530
---

<h1>Topics covered in this blog</h1>

* CAN Framework in RTEMS.
  * Registering the CAN BSP driver.
  * Handling Tx and Rx fifo.
  * Concurrency and Synchronization Handling.
  * Tx and Rx data flow.
* Writing CAN BSP driver using CAN framework.
  * Registering with the CAN Framework.
  * Tx and Rx with CAN Framework.

<h2>CAN Framework in RTEMS</h2>
The generic handling of the CAN protocol is implemented in cpukit/dev/can directory.

![](/docs/CAN-Framework.png)

<h3>Registering the CAN BSP driver</h3>
<!-- FIXME: The device node path name should be decided by the CAN framework not by CAN BSP driver -->
Each CAN BSP driver should register with the CAN framework by calling can_bus_register. The CAN BSP driver should pass two arguments can_bus data structure and CAN device node path.

can_bus_register function creates and initializes three things:
  * Creates the device node (Example: /dev/can0).
  * Creates the necessary tx and rx queues for the corresponding CAN BSP driver.
  * Initializes the required concurrency handling mechanisms (mutex).

It supports a maximum of 255 CAN BSP driver to be registered.

Each CAN BSP driver is represented by the can_bus data structure in the CAN framework.

<h3>Handling Tx and Rx fifo</h3>
The CAN message structure in RTEMS is declared in cpukit/include/dev/can/can-msg.h

```c                                                                            
struct can_msg {                                                                
  uint32_t id;                                                                  
  uint32_t timestamp;                                                           
  uint16_t flags;                                                               
  uint16_t len;                                                                 
  uint8_t data[CAN_MSG_MAX_SIZE];                                               
};
```

The data structure used for handling CAN Tx and Rx messages between applications and BSP CAN driver is a Circular buffer. The size of the buffer can be configured by CAN_TX_BUF_COUNT.

Note:
To modify the data structure used to handle CAN tx and rx messages can be done in the cpukit/dev/can/can-queue.h
The CAN framework is designed in such a way that only changes in this file is sufficient to use a differnet data structure.

<h3>Concurrency and Synchronization Handling</h3>
The synchronization of Tx and Rx fifos between applications and BSP CAN driver is done using counting semaphores.

The counting semaphore is created and initialized for each BSP CAN driver in can_create_sem function at the time of registration with the CAN framework. The maximum semaphore count is CAN_TX_BUF_COUNT.

The instances waiting for the semaphore are dequeued on First In First Out basis.

<h3>Tx and Rx data flow</h3>

<h4>Tx Data Path</h4>
The circular buffer is synchronized between application and the BSP CAN driver by using counting semaphore. At any time, the value of the semaphore count says the number of free
buffers avaliable.

![CAN TX data path](/docs/CAN-tx-data-path.jpg)

<h4>Rx Data Path</h4>
The circular buffer is synchronized between application and the BSP CAN driver by using counting semaphore. At any time, the value of the semaphore count says the number of buffers
to be read from the receive fifo.

![CAN RX data path](/docs/CAN-rx-data-path.jpg)

Note: Ideally, the tx and rx data path should contain a tx and rx fifo individually to each open call.

Need to be implemented: <br>
-> Each open function call from the application creates a tx, rx fifos (based on the flags) and counting semaphore for tx and rx fifos. <br>
-> Ioctl calls.

<h2>Writing CAN BSP driver using CAN framework</h2>
<h3>Registering with the CAN Framework</h3>
Every BSP CAN driver must register itself with CAN framework to use its services.

A successful call to can_bus_register will register to the CAN framework and creates a node (/dev/can{0, 1, \*}) for the application to access the CAN controller.
The can_bus_register takes can_bus data structure as argument, which BSP driver should populate the nescessary fields (can_dev_ops, index, priv).

<h3>Tx and Rx with CAN Framework</h3>
Once registered with CAN framework, the Tx and Rx of CAN messages between the application and the CAN controller can be availed.

* After every successful Tx messages sent in the physical CAN bus can_tx_done should be called. This call will notify the CAN framework that the device's Tx buffers are
free, which calls can_xmit to copy the CAN messages to device buffer which needs to be sent.
* After every successful Rx messages received from the CAN bus, the BSP driver can push the messages to the CAN framework by calling can_receive.

<br>
For reference: [BSP CAN Driver for Beaglebone Black](https://gitlab.com/slpp95prashanth/gsoc-2022/-/commit/866850f2e31731e5bc9519864f7676fa626dd31d) <br>
<br>
Note: CAN framework is a work in progress module. Your contribution and suggestions are always welcome.
