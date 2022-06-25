---
layout: post
title:  "Connecting Beaglebone Black through CAN bus"
date:   2022-06-25 12:15:18 +0530
---

<h1>Topics covering in this blog</h1>

* Connecting Beaglebone Black through CAN bus.
	* One node running RTEMS.
	* Other node running Linux.

<h2>Hardware connections for connecting two nodes through CAN bus</h2>
In Beaglebone Black, the controller does not have CAN transceivers. So here we use SN65HVD231DR in a breakout board CJMCU-230.
We need two SN65HVD231DR CAN transceivers for each node.

<h3>Pin Connections</h3>

![](/docs/CAN_transceiver.png)

<h2>Receiving CAN messages in Linux</h2>
* Build and load the linux image to BeagleBone Black.
* Linux has a package CAN-utils, which has cansend, candump commands

> $ sudo config-pin p9.24 can

> $ sudo config-pin p9.26 can

> $ sudo ifconfig can1 down

> $ sudo ip link set can1 up type can bitrate 1000000

> $ sudo ifconfig can1 up

> $ candump can1

<h2>Sending CAN messages in RTEMS</h2>


* Get the repository([RTEMS source link](https://gitlab.com/slpp95prashanth/gsoc-2022/-/tree/can)) and checkout to commit ID: 7a9cc16d.
> $ git checkout 7a9cc16d

* Configure and build the RTEMS source
* [Load the image to BeagleBone Black](https://common-logs.github.io/docs/2022/06/05/Boot-RTEMS-over-network-in-BBB.html)
* The CAN test application runs, which sends CAN messages in the CAN bus.

In Linux, candump command prints the CAN message received in the CAN bus.
