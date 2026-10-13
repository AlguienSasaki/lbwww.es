---
title: Install Libreboot on *Device_Name*
x-toc-enable: true
...

**[!!BACKUP everything!!](https://libreboot.org/docs/install/#howto-readwriteerase-the-boot-flash) just in case something fails.**

<div class="specs">
<center>
<img tabindex=1 alt="*device_image*" class="p" src="https://av.libreboot.org/*devicename/picture.jpg*" /><span class="f"><img src="https://av.libreboot.org/*devicename/picture.jpg*" /></span>
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | *Vendor*                                          |
| **Name**                   | *Device_Name*                               |
| **Variants**               | *If_there_are*                                |
| **Released**               | *Year*                                           |
| **Chipset**                | *Model*                                     |
| **CPU**                    | *Socket_type_if_replacable,_Supported_processors*                    |
| **Graphics**               | *integrated_gpu_device* |
| **Memory**                 | *ram_gen* SODIMMs (max_*capacity_usually_in_GB*,_*avaliable_slots_x_max_per_module*)    |
| **Architecture**           | *Device_Arch*                                         |
| **Original boot firmware** | *Vendor_firmware_name_with_UEFI_and_or_bios*                             |
| **Intel ME/AMD PSP**       | *Is_it_present_in_vendor_firmware_and_removable_or_disableable*      |
| **Flash chip**             | *type_and_size*           |


```
W+: Works without vendor firmware; 
N: Doesn't work; 
W*: Works with vendor firmware; 
U: Untested; 
P+: Partially works; 
P*: Partially works with vendor firmware
?: UNKNOWN AT THIS TIME
```

| ***Features***                                    |    |
|---------------------------------------------------|----|
| **Internal flashing with original boot firmware** | *Select_one_from_above* |
| **Display (if Intel GPU)**                        | *Select_one_from_above* |
| **Audio**                                         | *Select_one_from_above* |
| **RAM Init**                                      | *Select_one_from_above* |

| ***Payloads supported***   |           |
|----------------------------|-----------|
| **GRUB (libgfxinit only)** | *Works?*     |
| **SeaBIOS**                | *Works?*     |
</div>


### Buy pre-installed (only include if Minifree sells it)

** *Device_name* is available to purchase with Libreboot preinstalled.
See: <https://minifree.org/product/*device_name*/>** 


### Get the ROM image

**[Download](https://libreboot.org/download.html) from release**

If you're using a release ROM, please ensure that you've inserted extra firmware
required refer to the [guide](../install/ivy_has_common.md) for that (, failure
to adhere to this advice will result in a bricked machine):

- *include the needed firmware blobs, like Intel ME, Intel FSP for skylake =<*
- 

**[Build](https://libreboot.org/docs/build/) from source**

	./mk -b coreboot *device_name_in_lbmk*

*Include_more_if_there_are_different_variants_buildable*


### Flash a ROM image

Vendor/Libreboot, **always have a working previous firmware working** in case of failure.

**Software**

*Before internal flashing, [Disable /dev/mem protection](devmem.md) first.*

*In short add this kernel parameter: iomem=relaxed(linux distro)/kernel.securelevel=-1(bsd distro)*

**THEN REMOVE THIS KERNEL PARAMETER TO PREVENT UNAUTHORIZED/MALICIOUS ACCESS TO YOUR SYSTEM AFTER FLASHING LIBREBOOT.**

(An important reminder for those who might forget to delete it, even better, replace it with lockdown=integrity / lockdown=confidentiality)


Backup the previous (*working*) firmware (atleast) twice (to check and compare if reading it gone wrong and backups differs.)

	flashprog -p internal -r Backup1.bin

	flashprog -p internal -r Backup2.bin

Check and compare those if reading from the flash were successful:

	diff Backup1.bin Backup2.bin

For more than 2:

	shasum Backup1.bin Backup2.bin Backup3.bin

If those differs, something gone wrong and proceeding further may **(DON'T) leave your device unrecoverable** (without a reliable backup) if flashing fails as well.


*Description*


**Hardware**

For general information, please refer to [25xx NOR flash
instructions](../install/spi.md) - that page refers to use of socketed flash.


Backup the previous (*working*) firmware twice (to check and compare if reading it gone wrong and backups differs.)

	flashprog -p serprog:dev=/dev/ttyACM0,spispeed=16M -r Backup1.bin

	flashprog -p serprog:dev=/dev/ttyACM0,spispeed=16M -r Backup2.bin

Check and compare those if reading from the flash were successful:

	diff Backup1.bin Backup2.bin

For more than 2:

	shasum Backup1.bin Backup2.bin Backup3.bin

If those differs, something gone wrong and proceeding further may **(DON'T) leave your device unrecoverable** (without a reliable backup) if flashing fails as well.


*Description_+_picture_if_needed*


### Device Unique Properties

*Quirks_and_warnings_and_problems_and_more_info_below*


### Expanded_Specifications

*Include_expansion_slots_and_io_ports_and_extras_if_deemed_necessary*

*Different_variants_should_have_different_specs_if_not_exactly_the_same*

<div class="expandedspecs">
<center>
<img tabindex=1 alt="*device_image_with_io+expansion_slots*" class="p" src="https://av.libreboot.org/*devicename/io+exp.jpg*" /><span class="f"><img src="https://av.libreboot.org/*devicename/io+exp.jpg*" /></span>
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | *Vendor*                                          |
| **Name/Variant**           | *Device_Name/Variant*                               |
| **Released**               | *Year*                                           |
| **Chipset**                | *Model*                                     |
| **CPU**                    | *Socket_type_if_replacable,_Supported_processors_and_power_limit*                    |
| **Memory**                 | *Ram_gen_and_type* (max *capacity_usually_in GB*, *avaliable_slots x max_per_module*)  |
| **Graphics**               | *Supported iGPU* |
| **Audio**                  | *Integrated audio chip* |
| **Networking**             | *Integrated_network_chip_model*                                         |
| **Expansion Slots**        | *List_pcie_pci_m.2_and_others* |
| **Storage connectors**     | *List_avaliable_sata_or_m.2_ports_usually_and_others_if_there_are_more*      |
| **Other Connectors**       | *List_other_common_ones_like_usb_usb3_audio* |
| **I/O Ports on Front**     | *List_ports_found_there* |
| **I/O Ports on Back**      | *List_ports_found_there* |

</div>

*Note here if needed*


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"