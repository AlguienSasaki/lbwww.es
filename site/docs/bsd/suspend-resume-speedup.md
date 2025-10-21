---
title: Speed up suspend/resume for FreeBSD
x-toc-enable: true
...

# Preamble

The steps, necessary to speed up suspend/resume in FreeBSD are
described in this article. They are not a complete guide, but rather a
description of the principles that should be used to increase suspend
and resume times on your machine.

# Studying the Libreboot log

First, confirm that Libreboot is not a case for long suspend/resume
times. The `cbmem` utility can help you do that. By default, Libreboot
saves the debug log and timestamp for the last startup (or resume)
sequence in the device's RAM. This log can be accessed via the `cbmem`
utility (see the `coreboot/util/cbmem` catalog in the Coreboot
sources).

The command `sudo cbmem -t1` will print the next table with
timestamps:

```
22 entries total:

   0:1st timestamp                                     694 (0)
  11:start of bootblock                                73,218 (72,524)
  12:end of bootblock                                  74,119 (901)
  13:starting to load romstage                         74,119 (0)
  14:finished loading romstage                         74,784 (664)
   1:start of romstage                                 74,849 (65)
   2:before RAM initialization                         77,035 (2,185)
   3:after RAM initialization                          131,185 (54,150)
   4:end of romstage                                   136,623 (5,438)
 100:start of postcar                                  138,149 (1,525)
 101:end of postcar                                    138,149 (0)
   8:starting to load ramstage                         138,150 (0)
  15:starting LZMA decompress (ignore for x86)         138,158 (8)
  16:finished LZMA decompress (ignore for x86)         176,308 (38,149)
   9:finished loading ramstage                         176,486 (177)
  10:start of ramstage                                 176,562 (76)
  30:device enumeration                                176,573 (11)
  31:<unknown>                                         176,578 (4)
  40:device configuration                              183,496 (6,918)
  50:device enable                                     187,102 (3,606)
  60:device initialization                             187,457 (355)
  70:device setup done                                 247,559 (60,101)
  98:ACPI wake jump                                    247,617 (58)

Total Time: 246,915
```

The above table was acquired after a suspend/resume cycle on a
Thinkpad X220 laptop.

The author didn't find any information about the measurement units for
the numbers in the table. Therefore, it was assumed that the units
were microseconds, since Libreboot boots extremely fast.

Based on this assumption, it is clear that Libreboot is not the source
of the long resume times.

# Studying the FreeBSD dmesg log

The next step is to study the FreeBSD `dmesg` log after the
suspend/resume cycle. First, enable the timestamps for the kernel log.
The next line should be added to the `/boot/loader.conf`:

```
kern.msgbuf_show_timestamp=2
```

According to the [man 8 dmesg](https://man.freebsd.org/cgi/man.cgi?dmesg(8)):

> If set to 0, no timetamps are added. If set to 1, then a 1-second
> granularity timestamp will be added to most lines in the message
> buffer. If set to 2, then a microsecond granularity timestamp will be
> added. This may also be set as a bootloader(8) tunable. The timestamps
> are placed at the start of most lines that the kernel generates. Some
> multi-line messages will have only the first line tagged with a
> timestamp.

With this setting, the following `dmesg` lines were obtained for
suspend:

```
[58.431192] acpi_lid0: Lid closed
[61.793763] wlan0: link state changed to UP
[61.793838] wlan0: 2 link states coalesced
[61.793848] wlan0: link state changed to UP
[63.340099] battery1: battery initialization failed, giving up
[68.431390] acpi0: suspend request timed out, forcing sleep now
[69.191132] uhub3: at usbus3, port 1, addr 1 (disconnected)
[69.191160] ugen3.2: <vendor 0x8087 product 0x0024> at usbus3 (disconnected)
[69.191170] uhub6: at uhub3, port 1, addr 2 (disconnected)
[69.244325] ugen3.3: <vendor 0x8087 product 0x0a2b> at usbus3 (disconnected)
[69.244336] ubt0: at uhub6, port 3, addr 3 (disconnected)
[69.354402] ubt0: detached
[69.374292] uhub6: detached
[69.394299] uhub3: detached
[69.413706] uhub0: at usbus2, port 1, addr 1 (disconnected)
[69.413724] ugen2.2: <vendor 0x0424 product 0x2514> at usbus2 (disconnected)
[69.413732] uhub4: at uhub0, port 4, addr 1 (disconnected)
[69.434299] ugen2.3: <Logitech USB Trackball> at usbus2 (disconnected)
[69.434306] ums0: at uhub4, port 4, addr 2 (disconnected)
[69.454252] ums0: detached
[69.474665] uhub4: detached
[69.494650] uhub0: detached
[69.534681] uhub1: at usbus1, port 1, addr 1 (disconnected)
[69.534687] uhub1: detached
[69.574549] wlan0: link state changed to DOWN
[69.634591] uhub2: at usbus0, port 1, addr 1 (disconnected)
[69.634614] ugen0.2: <vendor 0x8087 product 0x0024> at usbus0 (disconnected)
[69.634623] uhub5: at uhub2, port 1, addr 2 (disconnected)
[69.694315] ugen0.3: <AGAN X230> at usbus0 (disconnected)
[69.694325] uhid0: at uhub5, port 1, addr 3 (disconnected)
[69.714328] uhid0: detached
[69.734300] ugen0.4: <UPEK Biometric Coprocessor> at usbus0 (disconnected)
[69.754304] ugen0.5: <Chicony Electronics Co., Ltd. Integrated Camera> at usbus0 (disconnected)
[69.814293] uhub5: detached
[69.834291] uhub2: detached
[70.188163] vgapci0: child drmn0 requested pci_set_powerstate
[73.112055] acpi0: cleared fixed power button status
[73.115446] vgapci0: child drmn0 requested pci_set_powerstate
[73.115451] vgapci0: child drmn0 requested pci_enable_io
[73.115454] vgapci0: child drmn0 requested pci_enable_io
[73.366940] uhub0 on usbus1
[73.366948] uhub0: <(0x1912) XHCI root HUB, class 9/0, rev 3.00/1.00, addr 1> on usbus1
[73.367041] uhub1 on usbus2
[73.367047] uhub1: <(0x1033) XHCI root HUB, class 9/0, rev 3.00/1.00, addr 1> on usbus2
[73.466755] acpi_tz0: switched from NONE to _AC0: 128.1C
[73.779239] uhub1: 4 ports with 4 removable, self powered
[73.779279] uhub0: 4 ports with 4 removable, self powered
[74.232159] ugen2.2: <vendor 0x0424 product 0x2514> at usbus2
[74.232319] uhub2 on uhub1
[74.232325] uhub2: <vendor 0x0424 product 0x2514, class 9/0, rev 2.00/0.00, addr 1> on usbus2
[74.232971] uhub2: MTT enabled
[74.974239] uhub2: 4 ports with 4 removable, self powered
[75.434134] ugen2.3: <Logitech USB Trackball> at usbus2
[75.441205] ums0 on uhub2
[75.441211] ums0: <Logitech USB Trackball, class 0/0, rev 1.10/14.00, addr 2> on usbus2
[75.445507] ums0: 5 buttons and [XY] coordinates ID=0
[77.676786] em0: link state changed to DOWN
[77.676827] lagg0: link state changed to DOWN
```

Clearly, with the "non-tweaked" configuration, the suspend takes
nearly 19.25 seconds (77.68 - 58.43).

The lines for the resume are:

```
[83.322494] acpi_tz0: temperature 128.1C: decreasing clock speed from 1600 MHz to 800 MHz
[83.956774] acpi_tz0: switched from _AC0 to NONE: 55.1C
[91.656767] em0: link state changed to UP
[91.656782] lagg0: link state changed to UP
[94.816837] battery0: battery initialization start
[94.816880] battery1: battery initialization start
[94.846957] uhub3 on usbus3
[94.847022] uhub4 on usbus0
[94.847029] uhub4: <Intel EHCI root HUB, class 9/0, rev 2.00/1.00, addr 1> on usbus0
[94.847033] uhub3: <Intel EHCI root HUB, class 9/0, rev 2.00/1.00, addr 1> on usbus3
[95.746981] acpi_tz0: temperature 58.1C: resuming previous clock speed (1600 MHz)
[96.234309] battery0: rev = ffff
[96.234314] battery0: units = 1
[96.234317] battery0: dcap = 7326
[96.234320] battery0: lfcap = 4538
[96.234324] battery0: btech = 1
[96.234335] battery0: dvol = 11100
[96.234337] battery0: wcap = 226
[96.234339] battery0: lcap = 200
[96.234341] battery0: gra1 = 1
[96.234342] battery0: gra2 = 1
[96.234347] battery0: battery initialization done, tried 1 times
[96.346113] uhub3: 3 ports with 3 removable, self powered
[96.346147] uhub4: 3 ports with 3 removable, self powered
[97.038680] ugen3.2: <vendor 0x8087 product 0x0024> at usbus3
[97.038911] uhub5 on uhub3
[97.038921] uhub5: <vendor 0x8087 product 0x0024, class 9/0, rev 2.00/0.00, addr 2> on usbus3
[97.058393] ugen0.2: <vendor 0x8087 product 0x0024> at usbus0
[97.058624] uhub6 on uhub4
[97.058630] uhub6: <vendor 0x8087 product 0x0024, class 9/0, rev 2.00/0.00, addr 2> on usbus0
[98.064311] uhub6: 6 ports with 6 removable, self powered
[98.304314] uhub5: 8 ports with 8 removable, self powered
[98.541038] ugen0.3: <AGAN X230> at usbus0
[98.544217] uhid0 on uhub6
[98.544228] uhid0: <AGAN X230, class 0/0, rev 1.10/0.00, addr 3> on usbus0
[98.769771] ugen3.3: <vendor 0x8087 product 0x0a2b> at usbus3
[98.981171] ugen0.4: <UPEK Biometric Coprocessor> at usbus0
[99.248246] Jul 28 15:57:08 freebsd wpa_supplicant[20391]: ioctl[SIOCS80211, op=20, val=0, arg_len=7]: Can't assign requested address
[99.445559] ugen0.5: <Chicony Electronics Co., Ltd. Integrated Camera> at usbus0
[99.775758] iwm0: dumping device error log
[99.776015] iwm0: Start Error Log Dump:
[99.776018] iwm0: Status: 0x83, count: 6
[99.776021] iwm0: 0x00003073 | ADVANCED_SYSASSERT
[99.776024] iwm0: 000002F0 | trm_hw_status0
[99.776026] iwm0: 00000000 | trm_hw_status1
[99.776028] iwm0: 0000E9C8 | branchlink2
[99.776030] iwm0: 0002843C | interruptlink1
[99.776032] iwm0: 00000000 | interruptlink2
[99.776034] iwm0: 00000000 | data1
[99.776036] iwm0: 00000001 | data2
[99.776038] iwm0: DEADBEEF | data3
[99.776041] iwm0: 0000180E | beacon time
[99.776043] iwm0: 049FF7F2 | tsf low
[99.776044] iwm0: 00000005 | tsf hi
[99.776046] iwm0: 00000000 | time gp1
[99.776048] iwm0: 019091A1 | time gp2
[99.776050] iwm0: 00000000 | uCode revision type
[99.776053] iwm0: 00000016 | uCode version major
[99.776056] iwm0: 00058404 | uCode version minor
[99.776059] iwm0: 00000201 | hw version
[99.776062] iwm0: 0080FF0F | board version
[99.776064] iwm0: 09480029 | hcmd
[99.776067] iwm0: 00022000 | isr0
[99.776068] iwm0: 00000000 | isr1
[99.776070] iwm0: 08005802 | isr2
[99.776072] iwm0: 404000C2 | isr3
[99.776073] iwm0: 00000000 | isr4
[99.776075] iwm0: 00800110 | last cmd Id
[99.776077] iwm0: 00000000 | wait_event
[99.776078] iwm0: 000000C0 | l2p_control
[99.776080] iwm0: 00000020 | l2p_duration
[99.776082] iwm0: 00000000 | l2p_mhvalid
[99.776084] iwm0: 00000000 | l2p_addr_match
[99.776086] iwm0: 0000000D | lmpm_pmg_sel
[99.776088] iwm0: 03071928 | timestamp
[99.776090] iwm0: 00008898 | flow_handler
[99.776185] iwm0: Start UMAC Error Log Dump:
[99.776187] iwm0: Status: 0x83, count: 7
[99.776189] iwm0: 0x00000070 | ADVANCED_SYSASSERT
[99.776191] iwm0: 0x00000000 | umac branchlink1
[99.776193] iwm0: 0xC008383C | umac branchlink2
[99.776195] iwm0: 0xC008166C | umac interruptlink1
[99.776198] iwm0: 0xC008166C | umac interruptlink2
[99.776200] iwm0: 0x00000800 | umac data1
[99.776202] iwm0: 0xC008166C | umac data2
[99.776203] iwm0: 0xDEADBEEF | umac data3
[99.776205] iwm0: 0x00000016 | umac major
[99.776207] iwm0: 0x00058404 | umac minor
[99.776209] iwm0: 0xC0886280 | frame pointer
[99.776211] iwm0: 0xC0886280 | stack pointer
[99.776213] iwm0: 0x09480029 | last host cmd
[99.776215] iwm0: 0x00000000 | isr status reg
[99.776217] iwm0: driver status:
[99.776219] iwm0:   tx ring  0: qid=0  cur=4   queued=0
[99.776222] iwm0:   tx ring  1: qid=1  cur=0   queued=0
[99.776224] iwm0:   tx ring  2: qid=2  cur=0   queued=0
[99.776227] iwm0:   tx ring  3: qid=3  cur=0   queued=0
[99.776229] iwm0:   tx ring  4: qid=4  cur=0   queued=0
[99.776232] iwm0:   tx ring  5: qid=5  cur=0   queued=0
[99.776234] iwm0:   tx ring  6: qid=6  cur=0   queued=0
[99.776237] iwm0:   tx ring  7: qid=7  cur=0   queued=0
[99.776240] iwm0:   tx ring  8: qid=8  cur=0   queued=0
[99.776242] iwm0:   tx ring  9: qid=9  cur=73  queued=0
[99.776245] iwm0:   tx ring 10: qid=10 cur=0   queued=0
[99.776248] iwm0:   tx ring 11: qid=11 cur=0   queued=0
[99.776250] iwm0:   tx ring 12: qid=12 cur=0   queued=0
[99.776253] iwm0:   tx ring 13: qid=13 cur=0   queued=0
[99.776255] iwm0:   tx ring 14: qid=14 cur=0   queued=0
[99.776258] iwm0:   tx ring 15: qid=15 cur=0   queued=0
[99.776261] iwm0:   tx ring 16: qid=16 cur=0   queued=0
[99.776263] iwm0:   tx ring 17: qid=17 cur=0   queued=0
[99.776266] iwm0:   tx ring 18: qid=18 cur=0   queued=0
[99.776268] iwm0:   tx ring 19: qid=19 cur=0   queued=0
[99.776271] iwm0:   tx ring 20: qid=20 cur=0   queued=0
[99.776273] iwm0:   tx ring 21: qid=21 cur=0   queued=0
[99.776276] iwm0:   tx ring 22: qid=22 cur=0   queued=0
[99.776278] iwm0:   tx ring 23: qid=23 cur=0   queued=0
[99.776281] iwm0:   tx ring 24: qid=24 cur=0   queued=0
[99.776283] iwm0:   tx ring 25: qid=25 cur=0   queued=0
[99.776286] iwm0:   tx ring 26: qid=26 cur=0   queued=0
[99.776288] iwm0:   tx ring 27: qid=27 cur=0   queued=0
[99.776291] iwm0:   tx ring 28: qid=28 cur=0   queued=0
[99.776293] iwm0:   tx ring 29: qid=29 cur=0   queued=0
[99.776296] iwm0:   tx ring 30: qid=30 cur=0   queued=0
[99.776298] iwm0:   rx ring: cur=143
[99.776300] iwm0:   802.11 state 5
[99.776302] iwm0: iwm_intr: controller panicked, iv_state = 5; restarting
[101.237688] ubt0 on uhub5
[101.237699] ubt0: <vendor 0x8087 product 0x0a2b, class 224/1, rev 2.00/0.01, addr 3> on usbus3
[101.333474] Jul 28 15:57:10 freebsd webcamd[86185]: webcamd: Cannot find USB device
[101.407337] Jul 28 15:57:10 freebsd webcamd[98635]: Webcamd is already running for ugen0.5.0
[104.431745] Jul 28 15:57:13 freebsd wpa_supplicant[20391]: ioctl[SIOCS80211, op=20, val=0, arg_len=7]: Can't assign requested address
[104.526921] wlan0: link state changed to UP
[104.526948] wlan0: 2 link states coalesced
[104.526951] wlan0: link state changed to UP
[104.742868] iwm0: code ce, frame 1/153 b800002c unhandled
[104.963470] iwm0: dumping device error log
[104.963652] iwm0: Start Error Log Dump:
[104.963654] iwm0: Status: 0x3, count: 6
[104.963657] iwm0: 0x00003073 | ADVANCED_SYSASSERT
[104.963660] iwm0: 000002F0 | trm_hw_status0
[104.963662] iwm0: 00000000 | trm_hw_status1
[104.963664] iwm0: 0000E9C8 | branchlink2
[104.963666] iwm0: 0002843C | interruptlink1
[104.963668] iwm0: 00000000 | interruptlink2
[104.963670] iwm0: 00000000 | data1
[104.963671] iwm0: 00000001 | data2
[104.963673] iwm0: DEADBEEF | data3
[104.963675] iwm0: 00009EFC | beacon time
[104.963677] iwm0: 04EF2115 | tsf low
[104.963679] iwm0: 00000005 | tsf hi
[104.963680] iwm0: 00000000 | time gp1
[104.963682] iwm0: 0035118B | time gp2
[104.963684] iwm0: 00000000 | uCode revision type
[104.963686] iwm0: 00000016 | uCode version major
[104.963688] iwm0: 00058404 | uCode version minor
[104.963690] iwm0: 00000201 | hw version
[104.963692] iwm0: 0080FF0F | board version
[104.963694] iwm0: 09480029 | hcmd
[104.963696] iwm0: 00022000 | isr0
[104.963698] iwm0: 00000000 | isr1
[104.963700] iwm0: 08001802 | isr2
[104.963701] iwm0: 404000C2 | isr3
[104.963703] iwm0: 00000000 | isr4
[104.963704] iwm0: 00800110 | last cmd Id
[104.963706] iwm0: 00000000 | wait_event
[104.963708] iwm0: 000002D0 | l2p_control
[104.963710] iwm0: 00000020 | l2p_duration
[104.963712] iwm0: 00000000 | l2p_mhvalid
[104.963714] iwm0: 00000020 | l2p_addr_match
[104.963716] iwm0: 0000000D | lmpm_pmg_sel
[104.963718] iwm0: 03071928 | timestamp
[104.963719] iwm0: 00008898 | flow_handler
[104.963788] iwm0: Start UMAC Error Log Dump:
[104.963790] iwm0: Status: 0x3, count: 7
[104.963792] iwm0: 0x00000070 | ADVANCED_SYSASSERT
[104.963794] iwm0: 0x00000000 | umac branchlink1
[104.963796] iwm0: 0xC008383C | umac branchlink2
[104.963798] iwm0: 0xC008166C | umac interruptlink1
[104.963801] iwm0: 0xC008166C | umac interruptlink2
[104.963803] iwm0: 0x00000800 | umac data1
[104.963805] iwm0: 0xC008166C | umac data2
[104.963807] iwm0: 0xDEADBEEF | umac data3
[104.963809] iwm0: 0x00000016 | umac major
[104.963810] iwm0: 0x00058404 | umac minor
[104.963812] iwm0: 0xC0886280 | frame pointer
[104.963814] iwm0: 0xC0886280 | stack pointer
[104.963816] iwm0: 0x09480029 | last host cmd
[104.963818] iwm0: 0x00000000 | isr status reg
[104.963820] iwm0: driver status:
[104.963823] iwm0:   tx ring  0: qid=0  cur=4   queued=0
[104.963825] iwm0:   tx ring  1: qid=1  cur=0   queued=0
[104.963828] iwm0:   tx ring  2: qid=2  cur=0   queued=0
[104.963830] iwm0:   tx ring  3: qid=3  cur=0   queued=0
[104.963833] iwm0:   tx ring  4: qid=4  cur=0   queued=0
[104.963835] iwm0:   tx ring  5: qid=5  cur=0   queued=0
[104.963838] iwm0:   tx ring  6: qid=6  cur=0   queued=0
[104.963841] iwm0:   tx ring  7: qid=7  cur=0   queued=0
[104.963843] iwm0:   tx ring  8: qid=8  cur=0   queued=0
[104.963846] iwm0:   tx ring  9: qid=9  cur=73  queued=1
[104.963849] iwm0:   tx ring 10: qid=10 cur=0   queued=0
[104.963851] iwm0:   tx ring 11: qid=11 cur=0   queued=0
[104.963854] iwm0:   tx ring 12: qid=12 cur=0   queued=0
[104.963857] iwm0:   tx ring 13: qid=13 cur=0   queued=0
[104.963859] iwm0:   tx ring 14: qid=14 cur=0   queued=0
[104.963862] iwm0:   tx ring 15: qid=15 cur=0   queued=0
[104.963864] iwm0:   tx ring 16: qid=16 cur=0   queued=0
[104.963867] iwm0:   tx ring 17: qid=17 cur=0   queued=0
[104.963870] iwm0:   tx ring 18: qid=18 cur=0   queued=0
[104.963872] iwm0:   tx ring 19: qid=19 cur=0   queued=0
[104.963875] iwm0:   tx ring 20: qid=20 cur=0   queued=0
[104.963878] iwm0:   tx ring 21: qid=21 cur=0   queued=0
[104.963881] iwm0:   tx ring 22: qid=22 cur=0   queued=0
[104.963883] iwm0:   tx ring 23: qid=23 cur=0   queued=0
[104.963886] iwm0:   tx ring 24: qid=24 cur=0   queued=0
[104.963888] iwm0:   tx ring 25: qid=25 cur=0   queued=0
[104.963891] iwm0:   tx ring 26: qid=26 cur=0   queued=0
[104.963893] iwm0:   tx ring 27: qid=27 cur=0   queued=0
[104.963896] iwm0:   tx ring 28: qid=28 cur=0   queued=0
[104.963898] iwm0:   tx ring 29: qid=29 cur=0   queued=0
[104.963901] iwm0:   tx ring 30: qid=30 cur=0   queued=0
[104.963903] iwm0:   rx ring: cur=141
[104.963905] iwm0:   802.11 state 5
[104.963907] iwm0: iwm_intr: controller panicked, iv_state = 5; restarting
[105.956818] wlan0: link state changed to DOWN
[109.167505] wlan0: link state changed to UP
[109.452608] iwm0: code ce, frame 1/179 b800002c unhandled
[109.675168] iwm0: dumping device error log
[109.675339] iwm0: Start Error Log Dump:
[109.675341] iwm0: Status: 0x3, count: 6
[109.675344] iwm0: 0x00003073 | ADVANCED_SYSASSERT
[109.675347] iwm0: 000002F0 | trm_hw_status0
[109.675349] iwm0: 00000000 | trm_hw_status1
[109.675351] iwm0: 0000E9C8 | branchlink2
[109.675353] iwm0: 0002843C | interruptlink1
[109.675355] iwm0: 00000000 | interruptlink2
[109.675357] iwm0: 00000000 | data1
[109.675359] iwm0: 00000001 | data2
[109.675360] iwm0: DEADBEEF | data3
[109.675362] iwm0: 00009972 | beacon time
[109.675364] iwm0: 053706A7 | tsf low
[109.675366] iwm0: 00000005 | tsf hi
[109.675368] iwm0: 00000000 | time gp1
[109.675369] iwm0: 003588AB | time gp2
[109.675371] iwm0: 00000000 | uCode revision type
[109.675373] iwm0: 00000016 | uCode version major
[109.675375] iwm0: 00058404 | uCode version minor
[109.675377] iwm0: 00000201 | hw version
[109.675379] iwm0: 0080FF0F | board version
[109.675381] iwm0: 09480029 | hcmd
[109.675383] iwm0: 00022000 | isr0
[109.675385] iwm0: 10000000 | isr1
[109.675386] iwm0: 08001802 | isr2
[109.675388] iwm0: 404000C0 | isr3
[109.675389] iwm0: 00000000 | isr4
[109.675391] iwm0: 00800110 | last cmd Id
[109.675393] iwm0: 00000000 | wait_event
[109.675394] iwm0: 000000C0 | l2p_control
[109.675396] iwm0: 00000020 | l2p_duration
[109.675398] iwm0: 00000000 | l2p_mhvalid
[109.675400] iwm0: 00000000 | l2p_addr_match
[109.675402] iwm0: 0000000D | lmpm_pmg_sel
[109.675404] iwm0: 03071928 | timestamp
[109.675406] iwm0: 00008898 | flow_handler
[109.675475] iwm0: Start UMAC Error Log Dump:
[109.675477] iwm0: Status: 0x3, count: 7
[109.675479] iwm0: 0x00000070 | ADVANCED_SYSASSERT
[109.675482] iwm0: 0x00000000 | umac branchlink1
[109.675484] iwm0: 0xC008383C | umac branchlink2
[109.675486] iwm0: 0xC008166C | umac interruptlink1
[109.675488] iwm0: 0xC008166C | umac interruptlink2
[109.675490] iwm0: 0x00000800 | umac data1
[109.675492] iwm0: 0xC008166C | umac data2
[109.675494] iwm0: 0xDEADBEEF | umac data3
[109.675496] iwm0: 0x00000016 | umac major
[109.675498] iwm0: 0x00058404 | umac minor
[109.675500] iwm0: 0xC0886280 | frame pointer
[109.675502] iwm0: 0xC0886280 | stack pointer
[109.675504] iwm0: 0x09480029 | last host cmd
[109.675506] iwm0: 0x00000000 | isr status reg
[109.675508] iwm0: driver status:
[109.675510] iwm0:   tx ring  0: qid=0  cur=4   queued=0
[109.675513] iwm0:   tx ring  1: qid=1  cur=0   queued=0
[109.675515] iwm0:   tx ring  2: qid=2  cur=0   queued=0
[109.675518] iwm0:   tx ring  3: qid=3  cur=0   queued=0
[109.675520] iwm0:   tx ring  4: qid=4  cur=0   queued=0
[109.675523] iwm0:   tx ring  5: qid=5  cur=0   queued=0
[109.675525] iwm0:   tx ring  6: qid=6  cur=0   queued=0
[109.675528] iwm0:   tx ring  7: qid=7  cur=0   queued=0
[109.675531] iwm0:   tx ring  8: qid=8  cur=0   queued=0
[109.675533] iwm0:   tx ring  9: qid=9  cur=73  queued=1
[109.675536] iwm0:   tx ring 10: qid=10 cur=0   queued=0
[109.675538] iwm0:   tx ring 11: qid=11 cur=0   queued=0
[109.675541] iwm0:   tx ring 12: qid=12 cur=0   queued=0
[109.675544] iwm0:   tx ring 13: qid=13 cur=0   queued=0
[109.675547] iwm0:   tx ring 14: qid=14 cur=0   queued=0
[109.675549] iwm0:   tx ring 15: qid=15 cur=0   queued=0
[109.675552] iwm0:   tx ring 16: qid=16 cur=0   queued=0
[109.675555] iwm0:   tx ring 17: qid=17 cur=0   queued=0
[109.675557] iwm0:   tx ring 18: qid=18 cur=0   queued=0
[109.675560] iwm0:   tx ring 19: qid=19 cur=0   queued=0
[109.675562] iwm0:   tx ring 20: qid=20 cur=0   queued=0
[109.675565] iwm0:   tx ring 21: qid=21 cur=0   queued=0
[109.675567] iwm0:   tx ring 22: qid=22 cur=0   queued=0
[109.675570] iwm0:   tx ring 23: qid=23 cur=0   queued=0
[109.675573] iwm0:   tx ring 24: qid=24 cur=0   queued=0
[109.675575] iwm0:   tx ring 25: qid=25 cur=0   queued=0
[109.675578] iwm0:   tx ring 26: qid=26 cur=0   queued=0
[109.675580] iwm0:   tx ring 27: qid=27 cur=0   queued=0
[109.675583] iwm0:   tx ring 28: qid=28 cur=0   queued=0
[109.675586] iwm0:   tx ring 29: qid=29 cur=0   queued=0
[109.675588] iwm0:   tx ring 30: qid=30 cur=0   queued=0
[109.675590] iwm0:   rx ring: cur=139
[109.675592] iwm0:   802.11 state 5
[109.675595] iwm0: iwm_intr: controller panicked, iv_state = 5; restarting
[110.722578] wlan0: link state changed to DOWN
[113.921614] wlan0: link state changed to UP
[114.203390] iwm0: code ce, frame 1/85 b800002c unhandled
[124.431988] wlan0: link state changed to DOWN
[124.551094] wlan0: link state changed to UP
[135.048478] wlan0: link state changed to DOWN
[135.165593] wlan0: link state changed to UP
[156.288696] battery1: battery initialization failed, giving up
```

The resume takes nearly 72.97 seconds (156.29 - 83.32).

Some changes could be made to reduce these times. After studying
the `dmesg` log, the following sources of slowdown were identified
(specific to the author's configuration):

* Non-existent battery: `battery1`.
* The `iwm` driver for the WiFi card.
* Some unused daemons and devices.
* Waiting for USB devices during suspend.

## Removing the non-existent battery

The laptop in question can be used with two batteries. One battery is
connected as usual, and the other can be connected via the docking
station connector. However, it currently has only one battery, so
FreeBSD shouldn't attempt to use the second, non-existent battery.

```
~ λ acpiconf -i 0
Design capacity:	7326 mAh
Last full capacity:	4538 mAh
Technology:		secondary (rechargeable)
Design voltage:		11100 mV
Capacity (warn):	226 mAh
Capacity (low):		200 mAh
Low/warn granularity:	1 mAh
Warn/full granularity:	1 mAh
Model number:		45N1023
Serial number:		 2101
Type:			LION
OEM info:		SANYO
State:			high
Remaining capacity:	99%
Remaining time:		unknown
Present rate:		0 mA (0 mW)
Present voltage:	12458 mV

~ λ acpiconf -i 1
Design capacity:	0 mWh
Last full capacity:	0 mWh
Technology:		primary (non-rechargeable)
Design voltage:		0 mV
Capacity (warn):	0 mWh
Capacity (low):		0 mWh
Cycle Count:		0
Measurement Accuracy:	0%
Max Sampling Time:	0 ms
Min Sampling Time:	0 ms
Max Average Interval:	0 ms
Min Average Interval:	0 ms
Low/warn granularity:	0 mWh
Warn/full granularity:	0 mWh
Model number:
Serial number:
Type:
OEM info:
State:			not present
Present voltage:	unknown
```

To make FreeBSD forget about the second battery, the following line
should be added to the `/boot/device.hints` file:

```
hint.battery.1.disabled=1
```

This setting will be applied after the reboot.

## Replacing the iwm driver with the iwlwifi driver

The log clearly shows that the system spent some time with `iwm`
driver in an attempt to recover from a (hardware-related?) error. So,
it was replaced with the `iwlwifi` driver, which also supports the
WiFi card from the laptop.

The `iwm` driver was blacklisted in the `/etc/rc.conf`:

```
devmatch_enable="YES"
devmatch_blocklist="if_iwm"
```

The time-consuming issues with the `iwm` driver disappeared after this
change.

## Disabling unwanted daemons

There are lines about the `webcamd` daemon trying to connect to the
webcam during resume. Since the webcam is not in use on this laptop,
the next line in the `/etc/rc.conf` was removed:

```
webcamd_enable="YES"
```

## Turn off unnecessary USB devices

The same log shows that the system spent a lot of time with the USB
devices, as well as some other devices, during the suspend/resume
cycle.

Since there are no Bluetooth devices in use with the laptop in
question, the Bluetooth driver (`ng_ubt`) should not be loaded. To
blacklist the driver, add the following lines to the `/etc/rc.conf`:

```
devmatch_enable="YES"
devmatch_blocklist="if_iwm ng_ubt"
```

And the waiting for USB devices during suspend or shutdown was
disabled in the `/etc/sysctl.conf`:

```
# Don't wait for USB devices on suspend or shutdown:
hw.usb.no_suspend_wait=1
hw.usb.no_shutdown_wait=1
```

USB devices without the attached driver should be completely powered
off. This can be achieved by adding the following line to the
`/boot/loader.conf`:

```
# Poweroff devices without attached drivers:
hw.em.smart_pwr_down=1
```

This may possibly cause problems with some USB devices, but the author
has not encountered any problems using the laptop.

# Results

After all these changes and reboot, the suspend log started to look
like this:

```
[869.263458] acpi_lid0: Lid closed
[873.064223] uhub3: at usbus0, port 1, addr 1 (disconnected)
[873.064250] ugen0.2: <vendor 0x8087 product 0x0024> at usbus0 (disconnected)
[873.064261] uhub6: at uhub3, port 1, addr 2 (disconnected)
[873.064275] uhub0: at usbus1, port 1, addr 1 (disconnected)
[873.064284] uhub0: detached
[873.064370] uhub1: at usbus3, port 1, addr 1 (disconnected)
[873.064395] ugen3.2: <vendor 0x8087 product 0x0024> at usbus3 (disconnected)
[873.064406] uhub5: at uhub1, port 1, addr 2 (disconnected)
[873.064428] uhub2: at usbus2, port 1, addr 1 (disconnected)
[873.064445] ugen2.2: <vendor 0x0424 product 0x2514> at usbus2 (disconnected)
[873.064451] uhub4: at uhub2, port 4, addr 1 (disconnected)
[873.083752] ugen2.3: <Logitech USB Trackball> at usbus2 (disconnected)
[873.083759] ums0: at uhub4, port 4, addr 2 (disconnected)
[873.130146] vgapci0: child drmn0 requested pci_set_powerstate
[874.462359] acpi0: cleared fixed power button status
[874.465674] vgapci0: child drmn0 requested pci_set_powerstate
[874.465679] vgapci0: child drmn0 requested pci_enable_io
[874.465683] vgapci0: child drmn0 requested pci_enable_io
[874.495321] ums0: detached
[874.507165] ugen3.3: <vendor 0x8087 product 0x0a2b> at usbus3 (disconnected)
[874.507240] ugen0.3: <AGAN X230> at usbus0 (disconnected)
[874.507246] uhid0: at uhub6, port 1, addr 3 (disconnected)
[874.527727] uhub5: detached
[874.527833] uhid0: detached
[874.547719] uhub1: detached
[874.547796] ugen0.4: <UPEK Biometric Coprocessor> at usbus0 (disconnected)
[874.567762] ugen0.5: <Chicony Electronics Co., Ltd. Integrated Camera> at usbus0 (disconnected)
[874.587126] uhub6: detached
[874.607103] uhub3: detached
[874.607110] acpi_tz0: switched from NONE to _AC0: 128.1C
[874.647204] uhub0 on usbus1
[874.647210] uhub0: <(0x1912) XHCI root HUB, class 9/0, rev 3.00/1.00, addr 1> on usbus1
[874.687042] xhci1: Resetting controller
[874.912798] uhub4: detached
[875.067049] uhub0: 4 ports with 4 removable, self powered
[875.332794] uhub2: detached
[875.407768] uhub1 on usbus2
[875.407773] uhub1: <(0x1033) XHCI root HUB, class 9/0, rev 3.00/1.00, addr 1> on usbus2
[875.829528] uhub1: 4 ports with 4 removable, self powered
[875.829546] uhub1: at usbus2, port 1, addr 1 (disconnected)
[875.829557] uhub1: detached
[875.907776] uhub1 on usbus2
[875.907781] uhub1: <(0x1033) XHCI root HUB, class 9/0, rev 3.00/1.00, addr 1> on usbus2
[876.317035] uhub1: 4 ports with 4 removable, self powered
[876.772401] ugen2.2: <vendor 0x0424 product 0x2514> at usbus2
[876.772533] uhub2 on uhub1
[876.772539] uhub2: <vendor 0x0424 product 0x2514, class 9/0, rev 2.00/0.00, addr 1> on usbus2
[876.773101] uhub2: MTT enabled
[877.514530] uhub2: 4 ports with 4 removable, self powered
[877.955021] ugen2.3: <Logitech USB Trackball> at usbus2
[877.962031] ums0 on uhub2
[877.962047] ums0: <Logitech USB Trackball, class 0/0, rev 1.10/14.00, addr 2> on usbus2
[877.966407] ums0: 5 buttons and [XY] coordinates ID=0
[878.947083] em0: link state changed to DOWN
[878.947125] lagg0: link state changed to DOWN
```

And here is the log for the resume:

```
[892.927133] em0: link state changed to UP
[892.927163] lagg0: link state changed to UP
[894.817215] acpi_tz0: temperature 52.1C: resuming previous clock speed (2000 MHz)
[896.161189] battery0: battery initialization start
[896.187244] uhub3 on usbus0
[896.187252] uhub3: <Intel EHCI root HUB, class 9/0, rev 2.00/1.00, addr 1> on usbus0
[896.187324] uhub4 on usbus3
[896.187331] uhub4: <Intel EHCI root HUB, class 9/0, rev 2.00/1.00, addr 1> on usbus3
[897.733354] uhub4: 3 ports with 3 removable, self powered
[897.763355] uhub3: 3 ports with 3 removable, self powered
[897.857187] battery0: rev = ffff
[897.857192] battery0: units = 1
[897.857195] battery0: dcap = 7326
[897.857199] battery0: lfcap = 4538
[897.857202] battery0: btech = 1
[897.857205] battery0: dvol = 11100
[897.857208] battery0: wcap = 226
[897.857212] battery0: lcap = 200
[897.857215] battery0: gra1 = 1
[897.857218] battery0: gra2 = 1
[897.857222] battery0: battery initialization done, tried 1 times
[898.429058] ugen3.2: <vendor 0x8087 product 0x0024> at usbus3
[898.429234] uhub5 on uhub4
[898.429241] uhub5: <vendor 0x8087 product 0x0024, class 9/0, rev 2.00/0.00, addr 2> on usbus3
[898.459030] ugen0.2: <vendor 0x8087 product 0x0024> at usbus0
[898.459154] uhub6 on uhub3
[898.459159] uhub6: <vendor 0x8087 product 0x0024, class 9/0, rev 2.00/0.00, addr 2> on usbus0
[899.464604] uhub6: 6 ports with 6 removable, self powered
[899.694605] uhub5: 8 ports with 8 removable, self powered
[899.931834] ugen0.3: <AGAN X230> at usbus0
[899.935129] uhid0 on uhub6
[899.935137] uhid0: <AGAN X230, class 0/0, rev 1.10/0.00, addr 3> on usbus0
[900.160017] ugen3.3: <vendor 0x8087 product 0x0a2b> at usbus3
[900.390259] ugen0.4: <UPEK Biometric Coprocessor> at usbus0
[900.894470] ugen0.5: <Chicony Electronics Co., Ltd. Integrated Camera> at usbus0
```

New suspend time is 9.69 seconds (878.95 - 869.26). Previously, it was
19.25 seconds.

New resume time is 7.96 seconds (900.89 - 892.93). Previously, it was
72.97 seconds.

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
