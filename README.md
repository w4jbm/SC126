# SC126
Various notes and files associated with the SC126

## RomWBWB
RomBWB comes with a set of tools allowing it to be built under either Windows or Linux. There was mention that the results would match "byte-by-byte" whichever method was used. When I first did a build under Linux, I compared the MD5SUM of the ROM image originally provided to the one I had generated and found that they were different. Digging a bit deeper, the next day I tried this on a different computer and got yet another MD5SUM for the ROM. I did this using the command:

```$ md5sum SCZ180*.*
ffb0cd2c36a403350b961da292da700e  SCZ180_126_created.rom
ec9d88a11fc9274e7d31cc1b87710b52  SCZ180_126_original.rom
$```

I found a quick way to compare the binary results to help understand where the difference was coming from.
