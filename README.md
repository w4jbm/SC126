# SC126
Various notes and files associated with the SC126

## RomWBWB
RomBWB comes with a set of tools allowing it to be built under either Windows or Linux. There was mention that the results would match "byte-by-byte" whichever method was used. When I first did a build under Linux, I compared the MD5SUM of the ROM image originally provided to the one I had generated and found that they were different. Digging a bit deeper, the next day I tried this on a different computer and got yet another MD5SUM for the ROM.

I found a quick way to compare the binary results to help understand where the difference was coming from.
