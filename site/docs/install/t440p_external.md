---
title: ThinkPad T440p external flashing
x-toc-enable: true
...

Read the [Ivybridge/Haswell common guide](/docs/install/ivy_has_common.html) if you want more information.
All of the following instructions assume that you've cloned lbmk and are operating from the
root of that project. To do so, run

	git clone https://notabug.org/libreboot/lbmk
	cd lbmk

You can now follow the rest of the instructions.

Preparing a release Rom
-----------------------

You must patch the release rom with the necessary blobs *and then* flash it to your board.

Lbmk includes a script that will automatically inject the necessary blobs into a rom file.
The script can determine the board automatically if you have not changed the name, but you can also manually set the board name with the `-b` flag.

In order to inject the necessary blobs into a rom image, run the script from the root of lbmk and point to the rom image.
For example:

	./blobutil inject -r t440p_libreboot.rom -b t440p_12mb

Optionally, you can use this script to modify the mac address of the rom with the `-m` flag.
For example:

	./blobutil inject -r t440p_libreboot.rom -b t440p_12mb -m 00:f6:f0:40:71:fd

Splitting The Rom
-----------------

You can use `dd` to easily split your rom into the two separate portions for
external flashing.

	dd if=libreboot.rom of=top.rom bs=1M skip=8
	dd if=libreboot.rom of=bottom.rom bs=1M count=8

Flash the top chip with top.rom, and tho bottom chip with bottom.rom.
Don't worry about knowing which chip is which on a standard setup; flashrom will let you know if the 
image size is incorrect for the chip you're flashing.


Disassembly
-----------

Start by removing the back cover screws and the main battery.\
<img tabindex=1 src="https://av.libreboot.org/board/t440p/t440p_back.jpg" /><span class="f"><img src="https://av.libreboot.org/board/t440p/t440p_back_orig.jpg" /></span>

You can then remove the back cover by sliding it off.
Next you need to:

+ Unplug the cmos battery
+ Unplug and unroute the fan cable
+ Unplug and unroute the black LED cable
+ Remove all visible screws

*Note: the ultrabay screw will loosen, but not come out of the assembly*\
<img tabindex=1 src="https://av.libreboot.org/board/t440p/t440p_nocover.jpg" /><span class="f"><img src="https://av.libreboot.org/board/t440p/t440p_nocover_orig.jpg" /></span>

Now you can pull up around the sides of the bottom assembly to release it.
Pull it upwards and lift it open to the front of the machine like a clamshell.
Make sure not to break the wires connecting the assembly to the rest of the machine.\
<img tabindex=1 src="https://av.libreboot.org/board/t440p/t440p_open.jpg" /><span class="f"><img src="https://av.libreboot.org/board/t440p/t440p_open_orig.jpg" /></span>

You should now be able to see the two flash chips near the RAM.\
<img tabindex=1 src="https://av.libreboot.org/board/t440p/t440p_chipLocation.jpg" /><span class="f"><img src="https://av.libreboot.org/board/t440p/t440p_chipLocation_orig.jpg" /></span>

You can now proceed to [flashing](/docs/install/spi.html) this machine.
