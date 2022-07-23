---
layout: post
title:  "MidTerm Evaluation"
date:   2022-07-23 12:15:18 +0530
---

<h1>MidTerm Evaluation</h1>

* Adding CAN support for BeagleBone Black in RTEMS
	* Added CAN TX support.
	* Added CAN Minimal RX support.
  * Ported DCAN support from AM335x StarterWare.
  * Added CAN Test Applications.

<h3>Added CAN TX support</h3>
The functions are defined in the [cpukit/dev/can/can.c](https://gitlab.com/slpp95prashanth/gsoc-2022/-/blob/can-review/cpukit/dev/can/can.c) file.

<h3>Important Functions</h3>
* can\_bus\_write
* can\_xmit
* can\_tx\_done

<h4>can_bus_write:</h4>
<p>The application's call to write function comes to the can_bus_write function, where the application buffer is copied to the driver fifo's buffer
based on the availability (else goes to sleep). After copying can_xmit is called.</p>

<h4>can_xmit:</h4>
<p>can_xmit function loops over the driver fifo and calls device tx function, where the CAN message is copied to the device fifo.</p>

<h4>can_tx_done:</h4>
<p>After the CAN message is sent in the bus, TX interrupt calls can_tx_done which wakes up the applications instance which are sleeping
for the buffer avaliability.</p>

<h3>Added CAN Minimal RX support</h3>
The functions are defined in the [cpukit/dev/can/can.c](https://gitlab.com/slpp95prashanth/gsoc-2022/-/blob/can-review/cpukit/dev/can/can.c) file.
<h3>Important Functions</h3>
* can_bus_Read
* can_receive

<h4>can_bus_read:</h4>
<p>The application's call to read function comes to the can_bus_read function, where the application waits (rtems_message_queue_receive) for any CAN message to be
received.</p>

<h4>can_receive:</h4>
<p>can_receive is called from the interrupt handler, when a valid CAN message is received. Then the messages in broadcasted (rtems_message_queue_broadcast) to the application instances which are
waiting (rtems_message_queue_receive) for the CAN message</p>

<h3>Ported DCAN support from AM335x StarterWare</h3>
The porting is work in progress.
The files are taken from Am335x StaterWare, which has DCAN support.

<h3>Added CAN Test Applications</h3>
The applications files are in [testsuites/cantests/can-tx-rx](https://gitlab.com/slpp95prashanth/gsoc-2022/-/tree/can/testsuites/cantests/can-tx-rx) link.

<h4>can-two-threads-tx.c</h4>
This file can be configured to create n threads, which sends different length CAN messages followed by read.
Note: The file name deos not match the functionality of the file.

Note: All the above files are work in progress, Please give your suggestions and feedbacks.
