---
layout: post
title: "Using stm32f4-discovery with OS X + built in st-link"
date: 2012-04-03 11:26
comments: true
categories: 
---
In me previous post - I talked about using an STM32F103 based Cortex M3 with OS X.  Since that post, I've ordered one of these - <a href="http://www.st.com/internet/evalboard/product/252419.jsp">STM32F4-Discovery</a>.  I am thinking about porting the work done with <a href="http://smoothieware.org/">smoothieware</a> from the LPC17xx platform to the STM32xxxx platform.  The STM32F4-Discovery is a relatively new development board that shows a lot of promise.  At ~$15 it is a bargain platform with a lot of features.

As usual, I need all of my goodies to work on my Mac and Windows boxes since I go back and forth a lot.  The following is a quick guide to getting this up and running with OS X Lion.

First, you'll need to get the stlink software compiled from <a href="https://github.com/texane/stlink">https://github.com/texane/stlink</a>:

``` bash
cd stlink # this is where you've cloned the stlink software
make
make install

```

You will also need to build the Mac driver:

``` bash
cd stlink/stlinkv1_macosx_driver
tar xzvf osx.tar.gz

# Then, install the driver using:
sudo make osx_stlink_shield

```

make sure the gdbserver works:
``` bash
cd stlink/gdbserver
./st_util

```

If it successfully connects - you should see something like this:

```
2012-04-03T13:51:50 INFO src/stlink-usb.c: -- exit_dfu_mode
2012-04-03T13:51:50 INFO src/stlink-common.c: Loading device parameters....
2012-04-03T13:51:50 INFO src/stlink-common.c: Device connected is: F4 device, id 0x20006411
2012-04-03T13:51:50 INFO src/stlink-common.c: SRAM size: 0x30000 bytes (192 KiB), Flash: 0x100000 bytes (1024 KiB) in pages of 16384 bytes
Chip ID is 00000413, Core ID is  2ba01477.
KARL - should read back as 0x03, not 60 02 00 00
init watchpoints
Listening at *:4242...
```

verify that you can load software through GDB (make sure you use a ARM gdb such as yagarto etc...)
Run the following:
``` bash
~/Downloads/STM32F4-Discovery_FW_V1.1.0 $ arm-none-eabi-gdb 
```

You will get something like this if you successfully connect:
```
GNU gdb (GDB) 7.3.1
Copyright (C) 2011 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "--host=i386-apple-darwin10.8.0 --target=arm-none-eabi".
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
```

```
(gdb) load /Users/draphael/Documents/workspace/TestDiscovery/TestDiscovery.elf
```

```
Loading section .isr_vector, size 0x188 lma 0x8000000
Loading section .text, size 0x6750 lma 0x8000188
Loading section .ARM, size 0x8 lma 0x80068d8
Loading section .init_array, size 0x4 lma 0x80068e0
Loading section .fini_array, size 0x4 lma 0x80068e4
Loading section .data, size 0x5d0 lma 0x80068e8
Loading section .jcr, size 0x4 lma 0x8006eb8
Start address 0x800676d, load size 28348
Transfer rate: 3 KB/sec, 3543 bytes/write.
```


```
(gdb) file /Users/draphael/Documents/workspace/TestDiscovery/TestDiscovery.elf
A program is being debugged already.
Are you sure you want to change the file? (y or n) y
Reading symbols from /Users/draphael/Documents/workspace/TestDiscovery/TestDiscovery.elf...done.


```

