Software for Gigatron ROM
=========================

Files
=====
```
theloop.py                      Video/audio/io/interpreter loops
                                Built-in vCPU applications and data
                                Execute theloop.py to build ROM files
gcl.txt                         Gigatron Control Language explanation
gtemu.c                         Emulator / executable instruction set definition
LICENSE                         2-Clause BSD License
Makefile                        Marcel's Makefile
README.md                       This file...
```

Files processed by theloop.py
=============================
```
Reset.gcl                       System reset code
Main.gcl *.gcl                  Application code
gcl.py                          Module: GCL to vCPU compiler
asm.py                          Module: Assembler functions
font.py                         Module: Gigatron font definition
Images/Baboon-160x120.rgb       Raw RGB image file (source: Baboon-160x120.png)
Images/Jupiter-160x120.rgb      Raw RGB image file (source: Jupiter-160x120.png)
Images/Parrot-160x120.rgb       Raw RGB image file (source: Parrot-160x120.png)
```

Files generated by theloop.py
=============================
```
theloop.asm                     Annotated disassembly, with labels and comments
theloop.0.rom                   ROM file for 28C256 #0 (breadboard)
theloop.1.rom                   ROM file for 28C256 #1 (breadboard)
theloop.2.rom                   ROM file for 27C1024 (PCB versions)
```

Memory map (RAM)
================
```
             +------------------------------------+---------------------+
page 0       | System and program variables     <-|-> vCPU stack at top |
             +------------------------------+-----+--+------------------+
page 1       | Video table               239| vReset | Channel 1 at top |
             +------------------------------+--------+------------------+
page 2       | Shift table for sound                 | Channel 2 at top |
             +---------------------------------------+------------------+
page 3       | vCPU code                          248| Channel 3 at top |
             |                                       +------------------+
page 4       |                                       | Channel 4 at top |
             |                                       +------------------+
page 5-7     |                                                          |
             =                                                          =
             |0                       159 160                        255|
             +---------------------------+------------------------------+
page 8-127   | 120 lines of 160 pixels   | Extra video/code/data at top |
             | Default video memory      |                              |
             |                           |                              |
             =                           =                              =
             |                           |                              |
             +---------------------------+------------------------------+
page 128-255 | Not used in the 32K system: mirror of page 0-127         |
             +----------------------------------------------------------+
              0                                                      255

Address   Name          Description
--------  ------------- -----------
0000      0
0001      memSize
0002      channel
0003      sample
0004      bootCount     0 for cold boot
0005      bootCheck     Checksum
0006      entropy
0009      returnTo
000b      videoY        Counts up from 0 to 238 in steps of 2 (odd in vBlank)
000c      frameX        Starting byte within page
000d      frameY        Page of current pixel row (updated by videoA)
000e      nextVideo     Jump offset to scan line handler (videoA, B, C...)
000f      videoDorF     Handler for every 4th line (videoD or videoF)
0010      vBlank        1=first scanline, start of vblank interval, 0=other line
0011      frameCount
0012      serialRaw     New raw serial read
0013      serialLast    Previous serial read
0014      buttonState   Clearable button state
0015      resetTimer    After 2 seconds of holding 'Start', do a soft reset
0016      xout
0017      xoutMask      The blinkenlights and sound on/off state
0018      vPC           Interpreter program counter, points into RAM
001a      vAC           Interpreter accumulator, 16-bits
001c      vLR           Return address, for returning after CALL
001e      vSP           Stack pointer
001f      vTicks        Interpreter ticks are units of 2 clocks
0020      vTmp
0021      sysPos
0023      sysData
0025      sysArgs
002d      soundTimer
002e      ledTimer      Number of ticks until next LED change
002f      ledState      Current LED state
0030      ledTempo      Next value for ledTimer after LED state change
0031-007f -             Program variables
0080      1             Constant
0081-.... -             Program variables
....-00ff <stack>
0100-01ef videoTable
01f0-01f8 vCpuBoot
01f9      wavX[1]       Sound channel 1
01fa      keyL[1]
01fb      keyH[1]
01fc      -
01fd      wavA[1]
01fe      oscL[1]
01ff      oscL[1]
0200-02f8 audioTable
02f9      wavX[2]       Sound channel 2
02fa      keyL[2]
02fb      keyH[2]
02fc      audioTable
02fd      wavA[2]
02fe      oscL[2]
02ff      oscL[2]
0300-03f8 -             vCPU code/data (standard start address)
03f9      wavX[3]       Sound channel 3
03fa      keyL[3]
03fb      keyH[3]
03fc      -
03fd      wavA[3]
03fe      oscL[3]
03ff      oscL[3]
0400-04f8 -             vCPU code/data
04f9      wavX[4]       Sound channel 4
04fa      keyL[4]
04fb      keyH[4]
04fc      -
04fd      wavA[4]
04fe      oscL[4]
04ff      oscL[4]
0500-05ff -             vCPU code/data
0600-06ff -             vCPU code/data
0700-07ff -             vCPU code/data
0800-089f pixel line 0
08a0-08ff -             vCPU code/data
...
7f00-7f9f pixel line 119
7fa0-7fff -             vCPU code/data
--------  ------------- -----------
```
