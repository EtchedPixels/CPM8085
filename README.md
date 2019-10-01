# Banked CP/M 3 for the RC2014 80C85 Development Platform

## WARNING

This is alpha test code. It may eat your disk. The on disk format might also
change depending upon feedback

## Introduction

This CP/M 3 build requires you are running the 8085/MMU card or compatible
devices, 512K/512K flat memory card, and a 16x50 UART card at 0xC0 or an ACIA at
0xA0. The disk I/O support at this point is for 4 x 8MB drives in the first
32MB of a CF adapter or PPIDE.

Note that the CF adapter does *not* appear to be reliable for higher speeds
with the 80C85.

## Building

With the CP/M toolchain available (on either CP/M 2.x or CP/M 3, or RunCPM
or similar), and ZMAC

ZMAC LDRBIOS.Z80
LINK CPMLDR[L100]=CPMLDR,LDRBIOS

ZMAC BNKBIOS3.Z80
ZMAC SCB.Z80
LINK BNKBIOS3[B]=BNKBIOS3,SCB
GENCPM

(and accept what is offered from the current GENCPM.DAT)

## Not Yet Supported

- RC2014 Real Time Clock
- Using multiple UART devices
- RAMdisc for the rest of memory

## Filesystem

Format the CF adapter for CP/M (eg with cpmtools)
Place the 8085 CP/M bootblock (see the ROM images) on block 0 of the CF card
Place CPMLDR on blocks 1+

CPM3.SYS, CCP.COM and anything else you want then goes on the A: drive which
is the first 8MB. B: C: and D: are the next 8MB chunks.

## Disk Layout

The 8MB file systems currently use the same layout as Bill Shen's CP/M. The
relevant cpmtools definition is

````
diskdef rc8085
  seclen 128
  tracks 64
  sectrk 1024
  blocksize 4096
  maxdir 512
  skew 0
  boottrk 1
  os 3
end
````

