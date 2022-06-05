---
layout: post
title:  "Boot RTEMS on BBB over network"
date:   2022-06-05 12:15:18 +0530
---

<p>To make the developing environment faster boot the RTEMS image over a network (using tftp).</p>

<h2>Steps to be followed</h2>

* Setting up tftp server in host
* Booting image from u-boot

<h3>Setting up tftp server in host</h3>
<p>Setting up a tftp server can be found in internet</p>

<h3>Booting image from u-boot</h3>
* Copy the build image rtems-hello.img and am335x-boneblack.dtb to the tftp directory (eg: /tftpboot)
* Run the commands in u-boot:

	> $ setenv ipaddr \<target ipaddress\>

	> $ setenv serveraddr \<host ipaddress\>

	> $ tftpboot \<load address\> \<rtems-image\>

	> $ tftpboot \<load address\> \<BBB device tree .dtb file\>

	> $ bootm \<load address of rtems-image\> - \<load address of .dtb\>

Example in case on BBB:

	> $ tftpboot 0x80800000 rtems-hello.img

	> $ tftpboot 0x88000000 am335x-boneblack.dtb

	> $ bootm 0x80800000 - 0x88000000
