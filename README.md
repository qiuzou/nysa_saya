nysa-sata-stack
===============

Sata stack written in Verilog

Staus: TLDR Version: Not finished! (longer version read below)

This code was written a long time ago and I've learned much more about verilog and project organization
since then. It has been proven in a Virtex 6 FPGA reading and writing to/from four Sata 2 hard drives at
the same time. There were some issues found when the internal buffers inside the hard drives began to
fill up requiring a lot of starting and stopping of this stack. It lead to some dropped data. There was
a horrible fix for it in the sata_link_write.v involving a small buffer. This made if very difficult to
debug because the stack wouldn't allow me to read the non-scrambled data within the logic analyzer.
There should be a more elegant solution to it.

Perhaps this code should be changed over to employ a Moore style state machines.

Most of the license is MIT but some of the licenses are GPL

TODO: Fix Link layer... there is a small FIFO in there that is used to handle all starting and stopping
of the read, it's a work around and needs to be fixed
TODO: Fix Link layer so that it only instantiates one instance of the scrambler, not two

TODO: Implement a testing framework with Cocotb

Code Organization:

  rtl/
    sata_stack.v (Top File that applications interface with)
    sata_defines.v (Set defines for the stack in here)

    generic/ (small modules used throughout the design)
      blk_mem.v (wraps around an infered block memory generator)
      cross_clock_enable.v (simple module that allows users to
                            send enables across a clock domain)
      debounce.v (debounce)
      ppfifo.v (ping pong FIFO, similar to a ping pong buffer
                except the user doesn't need to track the
                addresses)

    command/
      sata_command_layer.v (Sata Command Layer)

    transport/
      sata_transport_layer.v (Sata Transport Layer)

    link/
      sata_link_layer.v (Sata Link Layer)
      sata_link_layer_read.v (Sata link layer read side)
      sata_link_layer_write.v (Sata link layer write side)
      scrambler.v (scrambles/descrambles primitives)
      crc.v (Cyclical Redundancy Check/ creator)
      cont_controller.v (controls the scrambling of primitives)

    phy/
      sata_phy_layer.v (Sata phy layer)
      oob_controller.v (out of band controller)

    platform/
      sata_platform.v (This is a template file you can use to interface with the gigabit transceivers)


Soapbox:

Although I believe this code should be distributed for free and people should redistribute their software
I leave the ethics up to the user and have licensesed most of the code as MIT but I did use some GPL cores
and if the user desires to use this in their closed source project be warned about the GPL'ed modules in
this stack.
