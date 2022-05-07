# SpeedScript 3.0, cartridge edition

This is the SpeedScript word processor for Atari 8-bit computers,
adapted to run from cartridge.

The program was originally published in the May 1985 issue of COMPUTE!
magazine.  The source code used in this version, including comments, is
taken from the SpeedScript book, which was also published by COMPUTE! in
1985.

Assembly requires [ATasm](https://atari.miribilist.com/atasm/).  Two
targets are provided, an Atari DOS executable and a cartridge (.rom)
file.


## Changes from the published version

There are no major changes in this version of SpeedScript, but there are
some minor modifications.

### Support for 1200XL function keys

The Atari 1200XL has four function keys, F1-F4, which were mostly used
for cursor movement.  These keys are mapped to page up/page
down/home/end on the [TK-II PS/2 keyboard
adapter](https://ataribits.weebly.com/tk-ii.html), and have been given
SpeedScript functions as follows:

| F key | TK-II key | Function | SpeedScript native key |
| ----- | --------- | -------- | ---------------------- |
|Shift-F1|page up	|Paragraph up	|SHIFT-minus|
|Shift-F2|page down	|Paragraph down	|SHIFT-=|
|Shift-F3|home	|Top of document	|START\*|
|Shift-F4|end	|End of document	|CTRL-Z|

\* Shift-F3 differs from the START key in that it jumps directly to the
top of the document.  START moves the cursor to the top of the current
page the first time it's pressed, then jumps to the top of the document.

### Memory layout

SpeedScript's load address is at \$1F00.  The originally published
version uses parts of \$1F00-\$1FFF for its startup code, wasting the
rest of the space before the custom character set at \$2000.

Because code runs from cartridge in this version, all of \$1F00-\$1FFF
would be wasted if it wasn't used for something else. Fortunately there
is a 256-byte buffer (`PRBUFF`) that can be moved here instead.

### Self-modifying code (and data)

SpeedScript uses self-modifying code for its memory move routines.  In
the cartridge build, these routines are copied to RAM during
initialisation.

The character set is also modified to make non-printing space characters
visible.  To support this feature, the character set is copied to RAM in
the cartridge build.

### RESET handling

Cartridges are handled differently from DOS-loaded executables during
reset, so the cartridge version has a slightly different reset routine.

### Non-disk loading

SpeedScript can use more memory for text and the copy buffer when DOS is
not present.  The published version tests for cassette load but (for
obvious reasons) does not consider the possibility that it was loaded
from cartridge; this version instead tests that it was not loaded from
disk before enabling this feature.

## Building

If you have Ruby installed, the `rake` command can be used to build
SpeedScript.  Otherwise use the following commands:

### Cartridge

```
atasm -r -DCARTRIDGE speed0.m65 -ospeedcar.rom
```

### DOS executable

```
atasm speed0.m65 -ospeedcar.exe
```
