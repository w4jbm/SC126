# SC126
Various notes and files associated with the SC126

## Modifications

I ended up replacing R11 with a 1K resistor instead of the 470 ohm resistor provided. The yellow LED power indicator was fairly bright and distracted from the green user LED in position 0.

## Z180 I/O Compatability Issue

The built-in I/O devices on the Z180 chip can be mapped into four different locations and are mapped to $C0-$FF on the SC126 in RomWBW. The problem is that for the Z180, a full 16 bit I/O address is decoded and the upper 8 bits must be zero to access the onboard devices. On most legacy systems, only an 8 bit I/O address space is used and the upper bits are ignored.

To maintain compatability, the Z180 does place the contents of the B register on the upper 8 bits in the same manner as the Z80. The problem is that the traditional IN and OUT instructions assume an 8 bit I/O address space. (I'm sure there are nuances I'm missing in that.)

The immediate issue is that you can't use BASIC's IN or OUT commands to access the Z189's onboard I/O devices and you also can't use things like Martin Eberhard's XMODEM config file in it's simplist form. (XMODEM does have the ability to support up to 8 bytes of customer code through the /I option, but I haven't worked through that yet.)

New opcodes are provides with IN0 and OUT0 forcing the upper byte to zero and allowing access to the on-chip devices. I did find information on the machine code for those opcode--they use an extended two-byte format (which, unfortunately, means there isn't a simple one-byte patch for MBASIC). The machine code is:

```
$ED $08   IN0  C,(n)
$ED $09   OUT0 (n),C
```

These are both are said to function as NOPs on the original Z80.

## RomWBW
RomBWB comes with a set of tools allowing it to be built under either Windows or Linux. There was mention that the results would match "byte-by-byte" whichever method was used. When I first did a build under Linux, I compared the MD5SUM of the ROM image originally provided to the one I had generated and found that they were different. Digging a bit deeper, the next day I tried this on a different computer and got yet another MD5SUM for the ROM. I did this using the command:

```
$ md5sum SCZ180*.*
ffb0cd2c36a403350b961da292da700e  SCZ180_126_new.rom
ec9d88a11fc9274e7d31cc1b87710b52  SCZ180_126_orig.rom
$ _
```

Using `hexdump -C filename.ext` I could look at the files and at first glance they seemed largely the same.

I found a quick way to compare the binary results to help understand where the difference was coming from by creating hex dumps that could be compared using `diff`.

```
$ xxd SCZ180_126_new.rom > new.hex
$ xxd SCZ180_126_orig.rom > orig.hex
$ diff new.hex orig.hex
10c10
< 00000090: 3032 302d 3038 2d30 3300 5742 5700 524f  020-08-03.WBW.RO
---
> 00000090: 3032 302d 3034 2d30 3400 5742 5700 524f  020-04-04.WBW.RO
815c815
< 000032e0: 3032 302d 3038 2d30 3324 5343 3132 3624  020-08-03$SC126$
---
> 000032e0: 3032 302d 3034 2d30 3424 5343 3132 3624  020-04-04$SC126$
$ _
```
From this, you can see that the date of the original build of the ROM image is April 4th (04-04) while my new build was on August 3rd (08-03). Other than the difference in the build dates, the original ROM image (which, from the documentation) which was built under Windows does match my ROM image built under Linux.
