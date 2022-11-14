---
layout: post
title: "GSoC 2022: CAN Framework and Peripheral Support Final Report"
date:   2022-11-05 12:15:18 +0530
---

<h3>CAN Framework and DCAN peripheral support</h3>
This report summarizes the project that I have worked on RTEMS CAN Framework and DCAN
peripheral support throughout the Google Summer of Code 2022.
<!-- To center align text -->
<!-- {: style="color:gray; font-size: 80%; text-align: center;"} -->

<h3>Final commits</h3>
All the feature-specific commits are squashed into two commits (CAN framework and DCAN support).

* [CAN Framework support](https://github.com/RTEMS/rtems/commit/cd91b37dce728b372f164355719a4e601e12e7b3)
* [DCAN peripheral support](https://github.com/RTEMS/rtems/commit/26d50bdfb601b9ef71ec2b30d2d9467c2437f443)

<h3>Project Overview</h3>
The objective of this project is to add CAN Framework and DCAN peripheral support for BeagleBone Black System on Chip in 
RTEMS. CAN protocol is a robust, reliable and multi-master serial communication protocol 
used to achieve real time message transfer between devices within CAN network. 
RTEMS being a real time operating system, CAN peripheral support would increase 
the potential to meet real time demands.
{: style="text-align: justify;"}

<h3>Project Objectives</h3>
* Add CAN Framework.
* Add DCAN peripheral support.

<h3>Mentors</h3>
* Christian Mauderer
* Pavel Pisa
* Gedare Bloom

<h3>Summary</h3>
In this project, we have implemented CAN Framework with Tx, minimal Rx support and 
DCAN peripheral support for BeagleBone Black in RTEMS. I have summarized the work that I have done 
in each phase.
{: style="text-align: justify;"}

Project Proposal: [Google Doc](https://docs.google.com/document/d/1Y5zEsBzDGMaRENj_JCyVGQnAc7dhVZSx-ve06Uawhys/edit) <br>
RTEMS Wiki: [RTEMS Wiki]() <br>
Project Blogs: [Blogs](https://common-logs.github.io/docs/) <br>
Gitlab Repo: [Gitlab Repo](https://gitlab.com/slpp95prashanth/gsoc-2022/-/tree/can-dev-squashed1) <br>

<h3>Phase 1</h3>
In the first phase,

* Design CAN Framework.
* CAN Hardware test setup ([Communication between two BeagleBone Black through CAN bus](https://common-logs.github.io/docs/2022/06/25/Connecting-two-CAN-nodes.html)).
* Adding DCAN driver to RTEMS in loopback mode.

<h3>Phase 2</h3>
In the second phase,

* Implementing CAN framework.
* Adding test applications.
* Adding Tx and Rx FIFO in CAN framework.
* Adding Concurrency handling mechanisms.
* Adding DCAN interrupt support.

<h3>Phase 3</h3>
In the third phase,

* Integrating DCAN and CAN framework.
* Adding CAN loopback driver.
* Testing CAN framework and DCAN driver.
* Adding documentation.

<h3>Output</h3>
<h4>CAN Loopback Test Application</h4>
[CAN-Test-application](https://gitlab.com/slpp95prashanth/gsoc-2022/-/commit/aaf90ce5545b7e8588d10a9cc7cbfc8db34527ab) is the 
test application which creates a CAN loopback interface and does Tx and Rx on the file.

<h4>Output on Beaglebone Black</h4>
![hardware-setup](/docs/CAN_transceiver.png)
This figure shows the hardware setup used for testing CAN between two BeagleBone Blacks.

![Output](/docs/candump-output.png)
<br>

This image shows a CAN message sent from the BeagleBone Black application running on RTEMS with the CAN framework, which is received by another
Beaglebone Black running Linux.
{: style="text-align: justify;"}

<h3>Acknowledgement</h3>
I like to thank my mentors <b>Christian Mauderer</b>, <b>Gedare Bloom</b> and <b>Pavel Pisa</b> for their guidance
and support throughout my project. Especially <b>Christian Mauderer</b> for support and guidance without that
I would have not done this project. I like to thank <b>Oliver Hartkopp</b> for the initial guidance for designing
CAN Framework.
{: style="text-align: justify;"}

I also like to thank the <b>RTEMS community</b> (<b>Joel Sherrill</b>, <b>Karel Gardas</b> and many more) for the opportunity and support throughout the project.
{: style="text-align: justify;"}

<h3>What's Next?</h3>

I like contributing to the RTEMS community and like to continue to add other CAN features
to the CAN framework
