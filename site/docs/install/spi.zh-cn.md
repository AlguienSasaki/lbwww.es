---
title: 通过 SPI 协议对 25XX NOR flash 进行读/写
x-toc-enable: true
...

**[安装之前请先阅读这些指示](../../news/safety.md)，否则你的机器可能会成砖：[安全措施](../../news/safety.md)**

本指南将教你怎样使用各种工具，通过 SPI 协议对 25xx NOR flash 进行外部再编程。这是 coreboot 所支持的计算机中，最常见的 flash IC 类型。目前 libreboot 支持的每个系统，基本都使用这种类型的引导 flash；唯一的例外就是 ASUS KFSN4-DRE，它在 PLCC32 芯片座中使用了 LPC flash，你可以在供应商固件启动后，对其进行热切换，然后再内部刷入。十分简单！

我们会用到 [flashrom](https://flashrom.org/Flashrom) 软件，这个软件可以读出、擦除及重写这些 flash 芯片。

libreboot 目前记录了这些 SPI 编程器的使用方法：

* Raspberry Pi Pico
* 树莓派（Raspberry Pi，RPi）
* BeagleBone Black（BBB）
* Libre Computer 'Le Potato'

其他的 SPI 编程器还有许多。本页面以后会记载更多的 SPI 编程器。或者你也可以自己弄明白；尽管你用的编程器还没有被 Libreboot 记载，但这个页面某些部分的信息仍然会很有用。

大部分支持 libreboot 机器，都需要在第一次刷写的时候，借助这里的教程或是类似教程，对其进行外部再刷写。不过，目前支持的所有机器，你都可以在 libreboot 运行时，对其进行内部再刷写。

*内部*刷写是指，主机上的 CPU 可以使用板载 SPI 编程器（每个主板都有）对 SPI flash 进行再编程。这可以在 Linux 上使用 flashrom 做到。

你在读的*这个*教程，使用的是*外部*编程器。之所以叫*外部*，是因为用的不是主板上的*内部*编程器。

Raspberry Pi Pico
=================

![Left to right: Raspberry Pi Pico and Pico H](https://av.libreboot.org/rpi_pico/two_picos.webp)

If you don't already have a programmer, get this one! It's well engineered,
safe, and costs just $5 with headers pre-soldered (Raspberry Pi Pico H).
Additionally, all the software running on it is free, down to the full
[Boot ROM](https://github.com/raspberrypi/pico-bootrom). The wireless
versions (Pico W & Pico WH) need non-free firmware to use the Wi-Fi chip,
which is not needed for following this guide.

A Pico has proper 3.3V logic levels, unlike a ch341a. Which means it won't
destroy your board by sending 5V to it. If you have a 1.8V flash chip,
you need to add a logic level converter.

First, connect just the Pico to your computer with a micro-USB cable.
Mount it like any other USB flash drive. If it isn't detected, you might need
to press the BOOTSEL button while you plug it in (this forces it into the
bootloader mode).

Now download the serprog
[firmware](https://viitanen.xyz/pico-serprog-20230827.tar.lz).
Copy the file `pico_serprog.uf2` into your Pico.
Disconnect the Pico and proceed to wire it to your
[flash chip](/docs/install/spi.html#identify-which-flash-type-you-have).

![Raspberry Pi Pico pinout, when using the firmware linked above](https://av.libreboot.org/rpi_pico/pinout_serprog.png)

![A Raspberry Pi Pico connected to a SOIC16 flash chip](https://av.libreboot.org/rpi_pico/soic16_x200.webp)

Headers were manually soldered on the top side, and the plastic packaging
was repurposed as an insulating base. These might be nice to have, but by no
means necessary. If your headers are on the other side, just keep in mind
that the pinouts are as seen from above.

Now run (as root) `dmesg -wH`. When you plug in the Pico, a line like this
will appear:

	[453876.669019] cdc_acm 2-1.2:1.0: ttyACMx: USB ACM device

Take note of the ttyACMx. Flashrom is now usable
(substitute ttyACMx with what you observed earlier).

	flashrom -p serprog:dev=/dev/ttyACMx,spispeed=16M

spispeed=32M usually works, but since it's not much faster it's probably
not worth it. The 12Mbps USB port is limiting the actual speed here.

不要使用 CH341A！
==================

libreboot 支持的机器，NOR flash 使用的是 3.3V DC 或 1.8V DC，这也包括了数据线路。CH341A 在数据线路上有 5V 逻辑电平，这会损伤 SPI flash 和它连接的南桥，以及它连接的其它任何东西。

可惜的是，ch341a 编程器非常流行。除非你修复了这个问题，不然千万不要用它。你确实是可以修复它，让数据线路在 3.3v 工作的，只要跟着这里的步骤走：

<https://www.eevblog.com/forum/repair/ch341a-serial-memory-programmer-power-supply-fix/>

现实是，大多数人都不会去修复自己的 ch341a，而是选择去冒这个险，所以本网站不会提供 ch341a 的指南。最好不要使用那个设备。

**那篇 eevblog 没谈论到的一点是：WP/HOLD 引脚（第 3 和第 7 引脚）必须通过上拉电阻保持到高电平，但在 CH341A 上，它们是直接连接到 3.3V DC 的（连接在第 8 引脚上）。建议切断与 WP 和 HOLD 引脚的连接，并使用上拉电阻连接两引脚。1k 到 10k（欧姆）的值应该都可以。**

**如果电流出现浪涌，比如你把夹子连错了导致两个引脚短路的时候，那 WP/HOLD 缺少上拉电阻就会导致 VCC 与接地端之间短路，这会产生大量积热，可能还会起火（电路损坏更是当然的了）。在 SOIC8 上，第 3 引脚是 WP，第 4 引脚是 GND，所以说直接把 3.3v 连过去十分冒险；这些都是你用上拉电阻的理由。**

（比如你用 pomona 的夹子的话）你想刷的所有主板，多半已经存在用于 WP/HOLD 的上拉电阻了，所以直接断开 CH341A 上的 WP/HOLD 也行。只有在你也想刷写 ZIP 芯片座中的芯片时，你在这个 CH341A 上放置（在这个修改版中）的上拉电阻才有用。如果比方说笔记本主板和 CH341A 都存在上拉电阻，那就意味着（各支路上的）两个并联电阻会成为等效的串联电阻。假如我们有一台笔记本，它可能有约 3.3k 大小的上拉电阻，那在 CH341A 这边加上约 5.6k 欧姆的阻值就是合理的。

或者，你也可以这样解决问题：用一个有逻辑电平转换器的适配器，并确保有匹配的 vcc 进入 flash。在这种方案下，使用逻辑电平转换器相当灵活，你可以用它设置许多电压，比如说 1.8v 或者 3.3v。

再重复一遍：

**不要买 ch341a!** 它设计出来，不适合把 ROM 刷到那些使用了 3.3v SPI 的机器（即大多数 coreboot 机器）。千万不要使用它！现在厂商还没有修复这个问题，而且估计他们也不会再修复了！

如果你看到有人在谈论 CH341A，请给他们看这个页面，告诉他们为什么 CH341A 不好。

以下是完成了这两个修改（3.3v 逻辑电平和 WP/HOLD 上拉电阻）的黑色 CH341A：\
<img tabindex=1 src="https://av.libreboot.org/ch341a/0000_th.jpg" /><span class="f"><img src="https://av.libreboot.org/ch341a/0000.jpg" /></span>
<img tabindex=1 src="https://av.libreboot.org/ch341a/0001_th.jpg" /><span class="f"><img src="https://av.libreboot.org/ch341a/0001.jpg" /></span>

绿色版本（上面未展示）可能已经连接了 3.3v 逻辑电平，但仍然需要为 WP/HOLD 增加上拉电阻。

识别你的 flash 类型
==================================

每一个 flash，都会有一个点或者标记，表明这是第 1 引脚（如 WSON8 的第 1 焊盘）。

参考下面的图片，再看看你的主板。搞清楚你的芯片类型之后，根据你了解的情况及本教程剩下的部分，来实现你的目标，即对你的引导 flash 进行读/写。

SOIC8
-----

![](https://av.libreboot.org/chip/soic8.jpg)

SOIC16
------

![](https://av.libreboot.org/chip/soic16.jpg)

SOIC8 和 SOIC16 是最常见的类型，但也有其他的类型：

WSON8
-----

X200S 或 X200 Tablet 上是像这样的：\
![](https://av.libreboot.org/x200t_flash/X200T-flashchip-location.jpg)

T400S 上，是在 RAM 附近的这个位置：\
![](https://av.libreboot.org/t400s/soic8.jpg)\
注意: 本照片中的芯片换成了 SOIC8

DIP8
----

![](https://av.libreboot.org/dip8/dip8.jpg)

电源电压
--------------

之前，Libreboot 支持的所有主板，刚好都有在 3.3V DC 下工作的 SPI NOR 芯片。因为最近添加了额定 1.8V DC 芯片的 Chromebook，所以就不再是这么回事了。

检查主板上芯片的元件型号，再查找一下它的数据表。找出它需要的电源电压并记下来。如果它和你外部刷写硬件的输出电压不匹配，那你只能通过适配器或者逻辑电平转换器，把它连至芯片，而绝不能直接连接。

软件配置
======================

通用/Le potato
-----------------

[通用指南](spi_generic.md)可以帮助你使用本指南未列出的 SBC（单板电脑）。不过，那份指南会使用 libre computer 'Le Potato' 作为参考板。如果你有那块板子，你应该参考 [通用指南](spi_generic.md)。

BeagleBone Black（BBB）
----------------------

SSH 连接到你的 BeagleBone Black。假定你在 BBB 上用的是 Debian 9。你将在 BBB 上运行 `flashrom`。

注意：该部分已过时，因为它是写给 BBB 上运行的 Debian 9 的。

以 root 运行以下命令，开启 spidev：

```
config-pin P9.17 spi_cs
config-pin P9.18 spi
config-pin P9.21 spi
config-pin P9.22 spi_sclk
```

检查 spidev 设备现在是否存在：

	ls /dev/spidev*

输出：

	/dev/spidev1.0  /dev/spidev1.1  /dev/spidev2.0  /dev/spidev2.1

现在 BBB 可以开始刷了。以下的 systemd 服务文件可以选择性开启，从而让它重启后依然存在。

```
[Unit]
Description=Enable SPI function on pins

[Service]
Type=oneshot
ExecStart=config-pin P9.17 spi_cs
ExecStart=config-pin P9.18 spi
ExecStart=config-pin P9.21 spi
ExecStart=config-pin P9.22 spi_sclk
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

现在测试 flashrom：

	./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512

重要的一点是，要使用 `spispeed=512` 或者更低的速度，例如 256 或 128，否则 BBB 会十分不稳定。

输出示例：

```
Calibrating delay loop... OK.
No EEPROM/flash device found.
Note: flashrom can never write if the flash chip isn't found automatically.
```

这表示正常工作了（夹子没连接任何 flash 芯片，所以出错是正常的）。

BBB 注意事项
-----------------

不建议使用 BeagleBone Black，因为拿它来进行 SPI 刷写，速度很慢而且不稳定，并且现在有更好的选择了。我们以前建议使用 BBB，因为它可以完全运行自由软件，但现在有了更佳的选择。

计划：讲解其他 SPI 刷写工具

Rasberry Pi（RPi）
-----------------

SSH 连接到树莓派。你将在树莓派上运行 `flashrom`。

你必须在树莓派上配置 `spidev`。这是 Linux 内核的一个特别驱动；它严谨的名字叫做 `spi-bcm2835`。

这个页面有信息：\
<https://www.raspberrypi.org/documentation/hardware/raspberrypi/spi/README.md>

假定你的树莓派运行的是最新的 Raspbian。运行：

	sudo raspi-config

在 Interface 部分，你可以开启 SPI。

用于 SPI 通讯的设备位于 `/dev/spidev0.0`。

RPi 驱动强度（Drive Strength）
------------------

RPi 的 flashrom 可能无法检测到一些系统的 SPI flash，即使你已经完美地连好了线并夹住了芯片。这可能是因为树莓派 GPIO 的驱动强度，它默认是 8mA。驱动强度本质上就是，在保持高电平最低电压的同时，引脚最高能输出的电流。对树莓派而言，这个电压是 3.0 V。

类似地，也要满足一个最低电压，SPI flash 芯片才会把它当成高逻辑电平。这个值一般是 SPI flash 的 0.7*VCC，对 3.3V 的芯片而言也就是 2.31V。如果驱动强度太低了，那 flash 芯片的引脚处的电压可能会低于最低电压，导致它被视为发送了低逻辑电平，而不是高逻辑电平。

在许多系统，SPI flash 的 VCC 引脚和机器中其他芯片是共享的，导致你的编程夹提供的电压会给这些芯片供电。这样的话，芯片组的一部分就会启动，并试图把 SPI 线路弄得忽高忽低，妨碍了树莓派要发送的数据。如果树莓派和芯片组要把一个引脚设置为不同的值，驱动强度更高的一边能够将电压“拉”到它想设置的电平。

幸运的是，树莓派的驱动强度能增加到 16mA。有许多工具可以设置这一项，例如 pigpio 项目的 pigs 工具。在 Raspberry Pi OS 里，可以使用以下命令安装 pigpio 并将驱动强度设置为 16mA：

安装 pigpio：

	sudo apt install pigpio

启动 pigpiod 守护进程，pigs 工具会与其通讯，从而与 gpio 交互：

	sudo pigpiod

将包含了 spi0 引脚的 GPIO 0 组，驱动强度设置为 16mA：

	pigs pads 0 16 

注意，硬件工作的驱动强度是 2mA 的倍数，pigs 会将奇数值四舍五入到下一个 2 的倍数。你可以使用以下命令查看驱动强度

	pigs padg 0

警告：如果芯片组非常想把引脚的值设置为和树莓派相反，那树莓派的 GPIO 引脚就会通过比 16mA 还高的电流，这样会损坏引脚，因为它设计只能耐受 16mA。驱动强度不是电流限制。尽管如此，不管驱动强度如何，这对树莓派都有风险。芯片组和 flash 之间的电阻应该会对此进行保护，但并非所有主板都有这些电阻。

见 <https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#gpio-pads-control> 了解树莓派上的驱动强度控制。

RPi 注意事项
-----------------

基本上，Raspbian 项目，即现在的 Raspberry Pi OS，对其仓库进行了更新，增加了一个新的“受信任”仓库，这刚好是一个微软软件仓库。他们这么做，似乎是为了 VS Code，但问题在于，这可以让微软自由地控制他们喜欢的依赖（根据 apt-get 规则）。每当你更新，你都会对微软的服务器发送请求。不觉得这很奇怪吗？

微软不应该对你的 Linux 系统有*任何*访问权！这是 Raspbian 添加在他们仓库的 commit，正是它故意添加了我们应该称之为安全漏洞的仓库：

* <https://github.com/RPi-Distro/raspberrypi-sys-mods/commit/655cad5aee6457b94fc2336b1ff3c1104ccb4351>

在受到公众的强烈反对后，他们在以下 commit 中移除了它：

* <https://github.com/RPi-Distro/raspberrypi-sys-mods/commit/ed96790e6de281bc393b575c38aa8071ce39b555>
* <https://github.com/RPi-Distro/raspberrypi-sys-mods/commit/4d1afece91008f3787495b520ac03b53fef754c6>

RPi 的自由固件
---------------------

旧款树莓派的引导固件可以替换成完全自由的固件。对有些用户而言，这额外的一步可能很有用。参见：

<https://github.com/librerpi/>

网站：

<https://librerpi.github.io/>

安装 flashrom
----------------

如果你在使用 BBB 或者 RPi，你需要在 SSH 进去之后再这么做。

Flashrom 是用来读出、擦除、重写 NOR flash 内容的软件。

使用 Git 仓库中的 libreboot 构建系统，你可以下载并安装 flashrom。首先下载 [lbmk Git 仓库](https://codeberg.org/libreboot/lbmk)，然后执行：

	cd lbmk
	sudo ./build dependencies ubuntu2004

注意：你可以输入 debian、arch 或 void 来替换 ubuntu。debian 脚本也可以用于新版 ubuntu。

	./fetch flashrom
	./build module flashrom

如果 `ubuntu2004` 报告了依赖缺失，编辑一下这个脚本，把缺失的依赖移除就行了。脚本位于 `resources/scripts/build/dependencies/ubuntu2004`，它是写给 Ubuntu 20.04 的，但在其他使用 `apt-get` 包管理器的 Linux 发行版应该也能用。

接下来，会出现一个 `flashrom/` 目录，其中有一个 `flashrom` 可执行文件。如果你在运行上面的依赖命令的时候，出现了缺失包的错误，则修改 `resources/scripts/build/dependencies/ubuntu2004`。那个脚本会在 apt-get 中下载并安装构建依赖，它是为运行 Ubuntu 的 x86-64 系统写的，但在树莓派上的 Raspbian 应该能用。

或者，你可以直接从上游下载 flashrom，位于：<https://flashrom.org/Flashrom>

如果你是在 ThinkPad X200 上刷写 Macronix flash 芯片，则要使用一个 flashrom 的特别修改版，下载地址在这里：<https://vimuser.org/hackrom.tar.xz> —— 其中有修改版的源代码，也有可以直接运行的二进制文件。将 `--workaround-mx` 参数传给 flashrom。这会缓解稳定性问题。

如果你直接下载了 flashrom 源代码，你可以进入目录并直接运行 `make`。在 libreboot 构建系统中，`resources/scripts/build/dependencies/` 处的脚本写明了构建依赖，你可以直接使用 `apt-get` 软件安装。

如何使用 flashrom
===================

请先阅读本页更下方的部分，了解特定的芯片类型及其接线方法。

读出
-------

刷入新的 ROM 镜像之前，强烈建议你将当前芯片的内容读出到一个文件。

树莓派正确接线后，运行这个命令来查看是否检测到 25xx flash：

	sudo ./flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=32768

对 BBB 而言，必须使用更慢的速度及不同的设备路径：

	sudo ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512

在 BBB 上，绝对不要使用高于 `spispeed=512` 的速度。有时候，你可能还要低到 `spispeed=128` 的速度。BBB 对 SPI 刷写而言非常不稳定、不可靠。在读取的时候，要多次读出，并检查它们的 checksum 是否一致，然后再刷。你可能需要多次刷写芯片！

注意：在有些机器上，更高的速度会不稳定。这时，尝试一下更低的速度，例如 `spispeed=4096` 或者 `spispeed=2048`，这大多数情况下应该会工作，但明显会慢些。如果你使用的线短于 10cm，设置成 `spispeed=32768` 一般刚好就能工作了。

如果检测到了 flash 芯片，你可以现在尝试读出这个 flash 的内容，或者给它刷写新的 ROM。

在 RPi 上，这样读出：

	sudo ./flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=32768 -r dump.bin

BBB 的话，这样：

	sudo ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512 -r dump.bin

建议读出*两次*，比如弄一个 `dump2.bin`，然后检查 sha1sum：

	sha1sum dump*.bin

如果 checksum 匹配了，那读得就没问题。如果不匹配，那就要检查接线了。为了最佳稳定性，线长应该要短于 10cm，并且长度都要相同（VCC 和 GND 线路可以长些）。

这条建议是*特别*给 BBB 的，这块板子十分不可靠。

有些主板有不止一个 flash 芯片，你需要从两块芯片中读取，并把它们合并成一个文件。大多数时候。两个芯片的配置包含了一块 8mb 的“下层”芯片和一块 4mb 的“上层”芯片。刚刚描述的配置适用于 x230、t430 和 t440p 的情况。对其他主板而言，请确认那块芯片包含了 rom 的下层部分和上层部分。要把两个 flash 组合起来的话，你可以用比如说 `cat`：

	cat bottom_8mb.rom top_4mb.rom > full_12mb.rom

注意，如果你要手动提取 blob，那你就需要这个组合而成的 rom。

写入
-------

接下来，运行这个命令（RPi）：

	sudo ./flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=32768 -w /path/to/libreboot.rom

如果用的是 BBB：

	sudo ./flashrom -p linux_spi:dev=/dev/spidev1.0,spispeed=512 -w /path/to/libreboot.rom

用 BBB 的时候，可能得使用低于 512 的速度。你也许还得多次重复刷写，才能完全工作。

再说一遍，为了稳定性，你可能需要使用更低的 `spispeed` 值。

当命令输出了以下内容，那就说明刷写成功了。如果没有，那就再刷一遍。

```
Reading old flash chip contents... done.
Erasing and writing flash chip... Erase/write done.
Verifying flash... VERIFIED.
```

如果它显示“VERIFIED”了，或者芯片的内容和请求的镜像一致，那芯片就刷写成功了。

如果你要刷写的主板有两块芯片，那你需要把 rom 分成两部分。比如说，要给 x230、t430、t530 或 t440p 分割 rom，需要运行：

	dd if=libreboot_12mb.rom bs=1M of=bottom.rom count=8
	dd if=libreboot_12mb.rom bs=1M of=top.rom skip=8

根据上面的指南，将产出的 rom 刷到其对应的芯片。


硬件配置
======================

软件配置请参考上面的教程。下面的建议会教你如何为每种 flash 芯片接线。

警告
--------

在芯片还没有正确接好线时，先不要连接电源。例如，不要连接到接通电源的测试夹。

请先断开或关闭电源，再*断开*刷写工具与芯片的连接。

注意你提供给芯片的电源电压是否真的合适。目前 libreboot 支持的硬件，大多数的 SPI flash 都是在 3.3V DC 下工作的，逻辑电平也一样。有一部分（至少是 Chromebook）的芯片可能在 1.8V DC 下工作。你需要确认 SPI flash 芯片的元件型号及数据集，看看它需要的电源电压。如果你的外部刷写硬件无法匹配，那就使用适配器或者逻辑电平转换器来刷写。

对这些芯片做任何操作时，要检查一遍你使用的电源电压是否正确，这一点很重要。低于额定的电源电压不会造成任何损伤，但高于它的话就会把芯片烧了（大多数 3.3V 的芯片，能接受的电压介于 2.7V 和 3.6V 之间，但 3.3V 是最理想的）。

也不要给你的 flash 芯片连接超过 1 个 DC 电源！那样混合电压的话，很容易损伤你的设备及芯片/主板。

MISO/MOSI/CS/CLK 接线
----------------------

在刷写这些芯片的时候，你可能也想增加 47 欧姆的串联电阻（不是加在 VCC 或 GND 线上）。这可以提供一些过流保护。在 Intel 平台上，SPI flash 直接连接到南桥芯片组时，通常会通过这样的电阻。

ISP 编程及 VCC 二极管
-----------------------------

ISP 即系统内编程（in-system programming）。它指的是，一块芯片已经装在了你想安装 libreboot 的电脑的主板上，而你要对这块芯片进行刷写。

为了通过二极管来驱动（VCC 引脚上的）SPI flash，而对主板进行修改，可能会有一定的好处，但要注意的是：二极管会导致电压下降。一块需要 3.3V VCC 的芯片，能够接受大约 2.7V 到 3.6V DC，但压降后可能会低于这个区间。如果你要这么做的话，还请确认 WP 和 HOLD 引脚仍然保持在高电平状态；每一个引脚都通过自己的电阻（1K 到 10K 欧姆）连接到了*相同*的通过二极管的电源。

原因很简单：大多机器的 flash 都和主板上其他部件共用同一路电源，这会分走很多电流。然后，如果你不小心提供了过高的电压或者导致了过流，那可能就会烧了其他部分；但如果有二极管保护，你只能把引导 flash 烧了（如果你焊接技术不错的话，那很容易替换）。

把二极管接上去的时候，要确认芯片上的 VCC 隔离了主板上的其他所有部件，后者会共用同一路电源。然后，确保 WP/HOLD 的上拉电阻*仅*连接到了二极管导通 VCC 引脚的那一边（这一点很重要，因为如果不这样做的话，ISP 刷写的时候 WP/HOLD 就不会保持高电平，即使主板完全开机时它们还能保持高电平）。

还有就是：安装好二极管后，确保 SPI flash 完全启动时在合适的电源电压下工作（3.6V 芯片而言是 2.7V 到 3.3V）。

如果目标主板是桌面、工作站、服务器主板（而不是笔记本），那如果主板用的是 SOIC8/WSON8，你就可以对它拆焊，（对 SOIC8、WSON8 或 DIP8 之中的任意一个）使用芯片测试座来替换，因为那样你就可以简单地把 flash 插入面包板刷写了。

计划：在 libreboot.org 创建一个页面，讲怎么在 libreboot 支持的所有主板这么做。

BeagleBone Black（BBB）上的 GPIO 引脚
-----------------------------------

把 pomona 夹子连接到 BBB 时，参考这张图片：<https://beagleboard.org/Support/bone101#headers> （D0 = MISO 或连接到 MISO）。

如果你要用 *P9 排针*来连接芯片刷写的话，请看那个页面的那个部分。

40 引脚树莓派（RPi）的 GPIO 引脚
--------------------------------------

下图展示了大多数现代树莓派及其衍生版的引脚分配。图中右边是 RPi 的引脚，左边是两个 SOIC 夹。

![](https://av.libreboot.org/rpi/wiring.webp)

26 引脚树莓派（RPi）的 GPIO 引脚
-------------------------------

树莓派 B 款 26 GPIO 引脚图（对 40 引脚的 B+ 款而言，从右边开始数，剩下 14 个引脚）：

![](https://av.libreboot.org/rpi/0012.png) ![](https://av.libreboot.org/rpi/0013.png)

此处的信息，也请在阅读以下其他部分时参考：

SOIC8/DIP8/WSON8 接线图
-------------------------------

参考此表：

  Pin \# 25xx signal RPi(GPIO)  BBB(P9 header)
  ------ ----------- ---------- --------------
  1      CS          24         17
  2      MISO        21         21
  3      *未使用*     *未使用*    *未使用*
  4      GND         25         1
  5      MOSI        19         18
  6      CLK         23         22
  7      *未使用*     *未使用*    *未使用*
  8      VCC         1          3

在你的 SOIC8 上，其中一个角落会有一个点，这个点是第 1 引脚。

注意：第 3 和第 7 引脚是 WP/HOLD 引脚。在面包板上刷写芯片时，请对它们使用上拉电阻（见上面的注记），并在第 8 引脚（VCC）使用去耦电容。

注意：在 X60/T60 thinkpad 上，不要连接第 8 引脚。而是将你的 PSU 接在主板供电口上，但不要启动主板。这会提供稳定的 3.3V 电压及足够的电流。在这些笔记本上，这是必要的，因为 flash 和其他许多 IC 共用一路 3.3V DC 电源，这些 IC 都会分走很多电流。

SOIC16 接线图（树莓派）
------------------------------------

RPi GPIO 排针：\
![](https://av.libreboot.org/rpi/0009.png)
![](https://av.libreboot.org/rpi/0010.png)

BBB P9 排针：\
<https://beagleboard.org/static/images/cape-headers.png>

参考此表：

  Pin \#   25xx signal    RPi(GPIO)   BBB(P9 header)
  -------- -------------- ----------- --------------
  1        *未使用*        *未使用*     *未使用*
  2        VCC            1           3
  3        *未使用*        *未使用*     *未使用*
  4        *未使用*        *未使用*     *未使用*
  5        *未使用*        *未使用*     *未使用*
  6        *未使用*        *未使用*     *未使用*
  7        CS\#           24          17
  8        MISO           21          21
  9        *未使用*        *未使用*     *未使用*
  10       GND            25          1
  11       *未使用*        *未使用*     *未使用*
  12       *未使用*        *未使用*     *未使用*
  13       *未使用*        *未使用*     *未使用*
  14       *未使用*        *未使用*     *未使用*
  15       MOSI           19          18
  16       SCLK           23          22

参考本页面上方的 RPi GPIO 指南。

在你的 SOIC16 上，其中一个角落会有一个点，这个点是第 1 引脚。

注意：第 1 和第 9 引脚是 WP/HOLD 引脚。在面包板上刷写芯片时，请对它们使用上拉电阻（见上面的注记），并在第 2 引脚（VCC）使用去耦电容。

上拉电阻和去耦电容
-------------------------------------------

**如果芯片是装在面包板上的，那请遵循这里的步骤。如果你要刷写的芯片已经焊接在了主板上，那请忽略这一部分。**

如果你要刷写的新芯片还没有安装在主板上，那这一部分才有关系。你需要在 WP 和 HOLD 引脚增加上拉电阻，在 VCC 引脚增加去耦电容。如果芯片已经安装在了主板上，无论是通过焊接还是芯片座，那这些电容和电阻可能已经存在于主板上了，你可以直接刷写而无需将 WP/HOLD 拉高，也无需电容（只要连接外部电源即可）。

最佳方法如下：

* 如果是 DIP8 的话，将 DIP8 IC 插入面包板（2.54mm 的洞）
* 如果是 WSON8 的话，将 WSON8 插入 WSON8 socket，并装上面包板
* 如果是 SOIC8 的话，将 SOIC8 插入 SOIC8 socket，并装上面包板
* 使用 2.54mm 杜邦端子，将 SPI 刷写器连接到面包板，并正确接线（见下面的 SPI 刷写教程链接）

SOIC8/WSON8/DIP8：第 3 和第 7 引脚必须保持高逻辑电平状态，即每个引脚都通过上拉电阻连接到了 VCC（来自引脚 8 连接的电压层）；1K 欧到 10K 欧都可以。当你刷写的芯片已经处于笔记本/桌面/服务器主板上时，那第 3 和第 7 引脚很可能已经处于高电平，所以就不用再管这一点了。

SOIC8/WSON8/DIP8：如果芯片在主板上，那第 8 引脚，即 VCC，就已经有去耦电容在上面了。但如果单独把芯片拿出来刷，那这些电容就不存在。电容会通过 AC，但阻挡 DC。由于电磁感应及 IC 状态高速切换导致的射频噪声，DC 电压那根线（从示波器上看）其实并不是直线，而实际上还混进了一些低压 AC；在高荷载下的一根噪声特别大的线上，约 300mV 及以上的噪声是常见的。为了消除那些噪声，需要将 DC 线的电容接地，并将 VCC 那边的电容尽可能接近 IC 的 VCC 引脚。我们建议在这里使用陶瓷电容器。建议使用的电容器为：100nF 和 4.7uF 的陶瓷电容器。电解电容器还不如此，因为它的 ESR（等效串联电阻）更加高（陶瓷电容器的 ESR 非常低，十分利于去耦）。

使用去耦电容的结果就是，DC 线上的部分噪声过滤到了大地，使得 DC 信号（从示波器上看）更加干净、笔直了。

SOIC16：同上，但在面包板上使用 SOIC16 socket。在 SOIC16 上，WP/HOLD 不同于上面的第 3/7 引脚，而是第 1 和第 9 引脚，所以要把上拉电阻接到那里。SOIC16 上的 VCC 是第 2 引脚，所以要把去耦电容接到那里。

SOIC8/WSON8/DIP8/SOIC16 未安装在主板上
--------------------------------------------------

如果你的机器的 SPI flash 容量较低，那你可以升级。在*大多数*机器上，SPI flash 是经过映射的内存，而（实际上）你最大可以使用 16MiB 的芯片。例如，libreboot 的 KGPE-D16 和 KCMA-D8 主板默认有 2MiB flash，但你可以对它们轻松升级。另一个例子是 ThinkPad X200S、X200 Tablet 和 T400S，它们都有 WSON8，而最佳的方案就是将它替换为 SOIC8 flash 芯片。

在所有这些情况中，刷写新芯片都需要使用面包板，而不是测试夹。你需要使用 2.54mm 杜邦端子连接到树莓派。对数据线路而言，要确保每根线的长度都相等，线长约 10cm（不要再长了）。

一些建议：

* DIP8：建议选择 Winbond W25Q128FVIQ。这是个直接的替代品。
* SOIC8 也是可能的：建议选择 Winbond W25Q128FVSIG。
* DIP8 也可以使用转接器及 SOIC8。使用 208-mil 1.27mm SOP8/SOIC8 转 DIP8 的适配 PCB，并在每一边使用 2.54mm 四引脚排针（方形引脚），然后你就能把它当成正常的 P-DIP8 IC 插入了。这个页面是一个很好的例子： <https://coolcomponents.co.uk/products/soic-to-dip-adapter-8-pin>
* 如果你走上面的途径的话，那我们建议使用上述的 SOP8/DIP8 转接器。它是 Sparkfun 制造的，并且到处可见。没必要专门从一个网站上买。它的元件型号是：BOB-13655。
* 如果你使用 SOP/DIP 转接器和 SOIC8 IC，那你显然需要对它进行焊接。焊接这类 IC 时建议使用 K 焊头。使用良好的助焊剂和 60/40 含铅焊锡（或者 63/37），不要使用 Rohs 的无铅蹩脚货。

如果你使用 SOIC8，将它装上 SOP 转 DIP 的转接器（208mil 1.27mm），并为它焊上 2.54mm header。你可以将 2.54mm 引脚插在面包板上，将芯片焊在适配 PCB 上，并将它安装在面包板的引脚上，从而对齐再焊接。这时 PCB 在引脚上，引脚在面包板上，将引脚往里推一点。

这是写给新 SOIC8 芯片的，但你也可以使用类似视频里的 socket，不过是给 WSON8 的。有时候它们又叫做 DFN8 或 QFN8 socket。你需要 1.27mm 间距的。

如果你是在对单独的 WSON8 进行刷入/读出，那你需要一个 WSON8/QFN8/DFN8 芯片座（1.27mm 间距）并将其安装到面包板来刷写。如果你主板上 flash IC 的着陆焊盘能上 SOIC8，那我们建议你转而使用 SOIC8，因为后续你想再刷写它时，可以使用测试夹，但你的主板已有的 WSON8 换成 SOIC8 可能更加可取；那种情况下，你可能仍要把原 WSON8 的内容读出来。

这展示的是芯片座上的 SOIC8 安装在面包板上，以供刷写：\
![](https://av.libreboot.org/rpi/soic8_socket.jpg)

这张照片是 DIP8 IC：\
![](https://av.libreboot.org/dip8/dip8.jpg)

这张照片是 SOIC8 在 1.27mm 208mil SOP 转 DIP 的转接器上：\
![](https://av.libreboot.org/dip8/sop8todip8.jpg)

注意：DIP8、芯片座上的 WSON8 以及芯片座上的 SOIC16，基本上是一样的，只是用了相应的转接器。

如果你要替换 DIP8，但是在使用接在转接器上的 SOIC8，则先将它焊接在转接器上，然后将 2.54mm 排针（方形引脚）插入面包板来把它们对齐。将 SOIC8 置于 PCB 上，插入引脚，并把引脚往里推一些，然后将其焊接。或者对面包板而言，你可以直接在 2.54mm 引脚上插入 DIP8 芯片座，并在它上面装上 SOIC8 + 转接器，然后将其焊接。使用优质的松香助焊剂（非酸性）和良好的 60/40 或 63/37 含铅焊锡（不要使用无铅焊锡）：

![](https://av.libreboot.org/dip8/adapter_breadboard.jpg)
![](https://av.libreboot.org/dip8/adapter.jpg)
![](https://av.libreboot.org/dip8/sop8todip8.jpg)

SOIC8/SOIC16 焊接在主板上
------------------------------------

这是*系统内编程*或 *ISP* 的一个简短例子。

SOIC8：\
Pomona 5250 是一个 SOIC8 测试夹。也有其他的可以用，但这个是最好用的。使用 SOIC8 接线图（见上）来连接你的树莓派。你的主板很可能已经将 WP/HOLD（第 3 和第 7 引脚）拉到高电平了，所以不用连接这些。SOIC8 第 8 引脚的 VCC 很可能在主板上已经有去耦电容了，所以只需要勾起来而不用使用电容。

SOIC16：\
Pomona 5252 是一个 SOIC16 测试夹。也有其他的可以用，但这个是最好用的。就用这个。使用 SOIC16 接线图（见上）来连接你的树莓派。WP/HOLD 引脚是第 1 和第 9 引脚，并且很可能已经是高电平了，所以不需要上拉电阻。同样，第 2 引脚（VCC）也不再需要去耦电容，因为主板已经有一个了。

测试夹连接到 SOIC16 的例子如下：\
![](https://av.libreboot.org/rpi/0002.jpg)

SOIC8 例子的照片如下：\
![](https://av.libreboot.org/x60/th_bbb_flashing.jpg)

DIP8 焊接在主板上
------------------------------

把 DIP8 直接焊接在主板上怪异至极。它通常是安装在芯片座上的。

由于引脚足够大，你可以直接用测试夹的钩子来连接芯片进行刷写。你可能需要使用焊锡真空（提取）工具，对芯片进行拆焊，然后你就可以在那里安装一个芯片座了。接着你就可以把 DIP8 IC 插入芯片座。

我们在 libreboot 项目从来没听过有哪块板子的 DIP8 是直接焊上去的。它基本都是安装在芯片座上的。

DIP8 IC 的引脚分配和 SOIC8 IC 一样。

使用 SOIC8 替换 WSON8 IC
---------------------------

你*连是可以连* SOIC8 测试夹，但要连接效果好，需要费点功夫，而且这也十分不可靠。不要直接焊接 WSON8 的焊盘；有些人会这样做，但你不要这样做，因为你这样很容易就会损坏焊盘。

WSON8 的引脚分配和 SOIC8 一样，但它是球状 QFN（四边扁平无引脚封装）。它没有合适的夹子，有时候称为 QFN8。

目前 libreboot 支持的所有硬件，有 WSON8 的主板也可以上 SOIC8，因为它的焊盘长到足够容纳这两者任意一种芯片。

在 T12 焊台上上使用 T12-D08 或者 T12-K 焊头，是不错的烙铁选择。KSGER 生产的焊台还不错：\
<https://yewtu.be/watch?v=w0nZCK7B-0U>

KSGER 焊台的外壳默认是没有接地的，所以你应该改装一下，让外壳接地，以防发生电气事故。这是为了你的安全。这个视频展示了操作方法：\
<https://yewtu.be/watch?v=-6IZ_sBgw8I>

使用优质的 60/40 或者 63/37 铅+锡焊锡，不要使用无铅的！无铅的不适合这类业余用途。使用优质的*松香*助焊剂。绝不要使用酸性助焊剂。Amtech 和 MG Chemicals 生产的助焊膏就很不错。把它放在分配器管中使用。其中一些助焊剂含有己二酸，其 pH 值较低，只是用作一种温和的活化剂。只要事后清洗助焊剂，应该就没问题。

确保手边有铜丝刷和湿海绵。在铜丝刷上擦拭烙铁，并在湿海绵上轻拍（从而去除氧化物），保持清洁。焊头要经常清洁。此外，清洁过后，要使用新焊锡重新镀锡焊头，以防焊头氧化！

确保你买的是 99.9% 的异丙醇。不要购买弱一些的溶液，因为它们含水，也不要用其他的化学物质，因为其他大多数都有腐蚀性。焊接之前，先用异丙醇清洁要焊接的区域，然后用布吸收湿酒精。你还可以用它清除掉你使用过的任何助焊剂。

焊点要良好，助焊剂的使用十分重要，因为它移除了氧化物，并防止焊接过程中的进一步氧化，确保焊锡能够正常流动，否则焊锡会起球，无法获得良好的焊点。

如果你焊接技术不好，我们准备了一些不错的视频，链接在 [FAQ 页面](../../faq.md)，你可以去看看。

WSON8 IC：\
![](https://av.libreboot.org/rpi/wson8/0001.jpg)

使用几层 kapton 胶带包围芯片周围的一大块区域，然后再铺上铝箔。这样可以隔热，减少让其他焊点再流动的风险（这会把它们变成冷焊点，并且有风险把它们从主板上敲下来）：\
![](https://av.libreboot.org/rpi/wson8/0002.jpg)\
注意，胶带+铝箔不要盖住芯片本身或焊盘。让它们接触高温是必要的。

使用拆焊台，温度设为 330-340C 左右。温度这么高的原因是，空气的导热效率不如铁高，所以必须要高一些的温度。你需要在 IC 上面涂上很多松香助焊剂。喷嘴不要离主板太近。喷嘴的直径应该略高于芯片长度。在高气流下均匀加热。

对芯片喷射热空气时，用镊子夹住芯片，但不要用力。不要用力把芯片撬开了。只要用镊里夹住芯片，轻轻地推它，直到感觉到芯片可以自由移动了。这时，焊锡已经完全融化，芯片可以轻松取出来了。

如果不出什么问题，那一分钟内，芯片就能取下来，就像这样：\
![](https://av.libreboot.org/rpi/wson8/0003.jpg)

在焊盘上，包括导热垫上，加以新焊锡：\
![](https://av.libreboot.org/rpi/wson8/0004.jpg)

现在，用浸在松香助焊剂中的吸锡带将其吸干：\
![](https://av.libreboot.org/rpi/wson8/0005.jpg)

确保所有焊锡都已经清除：\
![](https://av.libreboot.org/rpi/wson8/0006.jpg)\
你会发现，其中一个焊盘上的焊锡没有完全去除。这是有意为之的，目的是给你参考对比一下。其他焊盘已经没有焊锡了。

你*可以*直接将没刷写好的芯片焊接上去，然后使用测试夹来刷写它。或者，你也可以将 SOIC8 插入面包板上的芯片座，并先刷好再焊接。如果你想把 WSON8 的内容读出来，你可以将移除的 WSON8 放在面包版上的芯片座中，然后使用 SPI 刷写器来对其进行读出。


对齐新的 SOIC8，将其焊到角落的引脚上。然后对其完全焊接。使用大量的助焊剂！\
![](https://av.libreboot.org/rpi/wson8/0007.jpg)\
这张图里用的是 T12-D08 焊头，但迷你凿形、迷你蹄形或刀形（如 T12-K）焊头是最理想的。

确保每一个焊点都完美了。好的焊点有光泽，并且焊锡流过的地方有凹陷的圆角。请看：\
![](https://av.libreboot.org/rpi/wson8/0008.jpg)

弄完之后，用软毛刷和 99.9% 的异丙醇把剩余的助焊剂清理干净，然后趁酒精还是湿的，用布吸收酒精。99.9% 的异丙醇是最好用的液体，因为它挥发很快，并且不会残留腐蚀性物质。

-------------------------------------------------------------------------------

许可证
=========

本页面发布所使用的版权条款，不同于本网站上大多数其他页面。

本页面及其照片，以 [CC BY SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/legalcode.txt) 发布。请检查 Git 仓库历史，了解本文档何部分归谁所有。

部分这些资源源自于*旧的* Libreboot git 仓库，Libreboot 随后分离成了单独的仓库，包括 `lbmk` 仓库。

展示了 BeagleBone Black 的照片，以 GNU Free Documentation License 许可，如同本网站的其他页面和图像。如果你想的话，你也可以在 CC-BY-SA 4.0 下使用它们（我，Leah Rowe，拥有本页展示的全部 BBB 照片的所有权，除了 beaglebone 网站上的那一张以外；而那一张只是在这里链接的，而不是托管在 av.libreboot.org 服务器的）。

本页面的该版本托管在 `lbwww` git 仓库，其图像托管在 `lbwww-img` 仓库（来自用户 libreboot）。
