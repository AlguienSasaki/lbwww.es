---
title: 兼容硬件列表
x-toc-enable: true
...

**[安装之前请先阅读这些指示](../../news/safety.md)，否则你的机器可能会变砖：[安全措施](../../news/safety.md)**

这一部分说明了 libreboot 已知兼容的硬件。

安装指南，请参看 [../install/](../install/)。

注意：对 T60/R60 thinkpad 而言，请确认它拥有的是 Intel GPU 而非 ATI GUI，因为 coreboot 对这些机器缺少 ATI GPU 的原生图像初始化。

（对 T500、T400 等后续机器而言，有 ATI GPU 也没问题，因为它也有 Intel GPU，而 libreboot 会用 Intel 的）

更新 LIBREBOOT 前请先阅读这里，否则你的机器可能会成变砖
====================================================================

**有些新的 Intel 平台需要 Intel ME 和/或 MRC 固件（例如 ThinkPad X230 或 T440p），还有些 HP 笔记本需要 KBC1126 EC 固件。对上述机器而言，Libreboot 官方发布的 ROM 缺少了特定的文件，你必须自己加入进去。如果无视这则警告，而坚持在不修改的情况下刷入官方发布的 ROM，那你可能会让你的机器变砖（导致它无法启动）。详请阅读：**

**[在 Sandybridge/Ivybridge/Haswell 插入二进制 blob](../install/ivy_has_common.md)**

注意：如果你是自己使用 lbmk 编译的 ROM，则不用在意这条警告。它只针对官方发布的 ROM，因为这些 ROM 里删除了 ME/MRC/EC 固件。上面的链接讲解了怎么把它们加回去。如果是自己从源代码构建 ROM 镜像，Libreboot 的构建系统会自动处理的。见：[Libreboot 构建指南](../build/)

已支持的硬件
==================

该版本的 libreboot 目前支持以下机器：

### 服务器（AMD，x86）

-   [ASUS KFSN4-DRE 主板](kfsn4-dre.md)
-   [ASUS KGPE-D16 主板](kgpe-d16.md)

### Desktops (AMD, Intel, x86)

-   [Acer G43T-AM3](acer_g43t-am3.md)
-   [Apple iMac 5,2](imac52.md)
-   [ASUS KCMA-D8 主板](kcma-d8.md)
-   Dell OptiPlex 7020/9020 MT and SFF (no guides yet)
-   [Dell Precision T1650](t1650.md)
-   [Gigabyte GA-G41M-ES2L 主板](ga-g41m-es2l.md)
-   [HP Elite 8200 SFF/MT](hp8200sff.md)（HP 6200 Pro Business 多半也能用）
-   [HP Elite 8300 USDT](hp8300usdt.md)
-   [Intel D510MO 及 D410PT 主板](d510mo.md)
-   [Intel D945GCLF](d945gclf.md)（移出 lbmk，计划：重新加入支持）

### 笔记本（Intel，x86）

-   [Apple MacBook1,1 及 MacBook2,1](macbook21.md)
-   [Dell Latitude E6400, E6400 XFR 及 E6400 ATG，皆支持 Nvidia 或 Intel GPU](e6400.md) **（刷入简单，无需拆解，硬件类似 X200/T400）**
-   Dell Latitude E6420 (Intel GPU) - no guide yet. internal flashing possible.
-   [Dell Latitude E6430, Intel GPU](e6430.md) **（刷入简单，无需拆解）**
-   Dell Latitude E5530 (Intel GPU) - no guide yet. internal flashing possible.
-   Dell Latitude E6520 (Intel GPU) - no guide yet. internal flashing possible.
-   [HP EliteBook 2170p](hp2170p.md)
-   [HP EliteBook 2560p](hp2560p.md)
-   [HP EliteBook 2570p](hp2570p.md)
-   [HP EliteBook 820 G2](hp820g2.md)
-   [HP EliteBook 8460p](hp8460p.md)
-   [HP EliteBook 8470p](hp8470p.md)
-   [HP EliteBook 8560w](hp8560w.md)
-   [HP EliteBook Folio 9470m](hp9470m.md)
-   [Lenovo ThinkPad R400](r400.md)
-   [Lenovo ThinkPad R500](r500.md)
-   [Lenovo ThinkPad T400 / T400S](t400.md)
-   [Lenovo Thinkpad T420](../install/ivy_has_common.md)（暂无安装文档）
-   [Lenovo ThinkPad T420S](../install/ivy_has_common.md)（暂无安装文档）
-   [Lenovo ThinkPad T430](../install/ivy_has_common.md)（暂无安装文档）
-   [Lenovo ThinkPad T440p](../install/t440p_external.md)
-   [Lenovo ThinkPad T500](t500.md)
-   [Lenovo ThinkPad T530 / W530](../install/ivy_has_common.md)（暂无安装文档）
-   Lenovo ThinkPad T60（Intel GPU 款）
-   [Lenovo ThinkPad W500](t500.md)
-   [Lenovo ThinkPad W541](../install/ivy_has_common.md)（暂无安装文档）
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](x200.md)
-   [Lenovo Thinkpad X220](../install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../install/ivy_has_common.md)
-   Lenovo ThinkPad X230
-   [Lenovo Thinkpad X230](../install/x230_external.md)
-   [Lenovo Thinkpad X230t](../install/x230_external.md)
-   Lenovo ThinkPad X301
-   Lenovo ThinkPad X60 / X60S / X60 Tablet

### 笔记本（ARM，配 U-Boot payload）

-   [ASUS Chromebook Flip C101 (gru-bob)](../install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../install/chromebooks.md)

## 已移除的主板

这些主板 Libreboot 以前支持，但现在移除了，计划在以后重新加回来。它们之所以被移除，是因为出现了问题。主板列表：

-   [HP Chromebook 14 G3 (nyan-blaze)](../install/chromebooks.md)
-   [Acer Chromebook 13 (CB5-311, C810) (nyan-big)](../install/chromebooks.md)
-   [Hisense Chromebook C11 and more (veyron-jerry)](../install/chromebooks.md)
-   [Samsung Chromebook 2 13" (peach-pi)](../install/chromebooks.md)
-   [Samsung Chromebook 2 11" (peach-pit)](../install/chromebooks.md)
-   [HP Chromebook 11 G1 (daisy-spring)](../install/chromebooks.md)
-   [Samsung Chromebook XE303 (daisy-snow)](../install/chromebooks.md)
-   [ASUS Chromebit CS10 (veyron-mickey)](../install/chromebooks.md)
-   [ASUS Chromebook Flip C100PA (veyron-minnie)](../install/chromebooks.md)
-   [ASUS Chromebook C201PA (veyron-speedy)](../install/c201.md)

### 关于已移除的主板

**警告：2023 年 2 月 19 日，veyron speedy 主板（例如 C201）的图像初始化无法工作，截至该时还没有解决方案。见 <https://notabug.org/libreboot/lbmk/issues/136> —— 2021.01 的最后一个测试过的版本，这个带 u-boot 的主板是可以工作的。见：\
<https://wiki.postmarketos.org/wiki/ASUS_Chromebook_C201_(google-veyron-speedy)>
（IRC 上的 alpernebbi 正在研究这个问题，二分法排查 uboot 并更新最新的版本）——目前，Libreboot 20221214 和 20230309 的 ROM 镜像已经删除。**

**警告：daisy- 和 peach- 主板需要 B1 bootloader blob，但 coreboot 第三方有一个假的/占位的 blob。我们需要在 Libreboot 构建系统中实现一套逻辑，正确获取/提取这些 blob，并为其编写文档。目前的话，就假定它们是损坏的——ROM 镜像目前已经排除，并在 Libreboot 20221214 和 20230309 中删除。——见：<https://review.coreboot.org/plugins/gitiles/blobs/+/4c0dcf96ae73ba31bf9aa689768a5ecd47bac19e> 及 <https://review.coreboot.org/plugins/gitiles/blobs/+/b36cc7e08f7337f76997b25ee7344ab8824e268d>**

d945gclf：据上次报告，根本无法启动。D510MO 仍在 lbmk 中，但仍然报告有问题；其他主板应该没问题（见上方列表）。

警告：这些主板的支持，还处于概念验证阶段。参考 [docs/uboot/](../uboot/) 了解 U-boot payload 的更多信息。

### 模拟

-   [Qemu x86](../misc/emulation.md)
-   [Qemu arm64](../misc/emulation.md)


计划：支持更多硬件。见 lbmk 中的 `config/coreboot/`。更新上面的列表！

所谓“支持”，即指构建脚本知道如何构建这些机器的 ROM 镜像，并且机器经过测试（确认能够工作）。也可能会有例外；换言之，这是“官方”支持的机器列表。

在 i945（X60、T60）及 GM45（X200、X301、T400、T500、R400、W500、R500）上更新 EC
==============================================================

建议更新到最新 EC 固件版本。[EC 固件](../../faq.md#ec-embedded-controller-firmware) 与 libreboot 是独立的，所以我们实际上并不会提供这些固件，但如果你仍还有 Lenovo BIOS，那你可以直接运行 Lenovo BIOS 更新工具，它会同时更新 BIOS 和 EC 版本。见：

-   [../install/#flashprog](../install/#flashprog)
-   <http://www.thinkwiki.org/wiki/BIOS_update_without_optical_disk>

NOTE: Libreboot standardises on [flashprog](https://flashprog.org/wiki/Flashprog)
now, as of 27 January 2024, which is a fork of flashrom.

注意：只有在运行 Lenovo BIOS 的时候，你才能这样做。如何在运行 libreboot 的时候更新 EC 固件尚不清楚。libreboot 只会替换 BIOS 固件，而不会替换 EC。

更新的 EC 固件有一些好处，例如电池管理更加好。

如何得知你的 EC 版本（i945/GM45）
------------------------------------------------

在 Linux，你可以试试这条命令：

	grep 'at EC' /proc/asound/cards

输出样例：

	ThinkPad Console Audio Control at EC reg 0x30, fw 7WHT19WW-3.6

7WHT19WW 另一种形式的版本号，使用搜索引擎来找出正常的版本号——这个例子中的 x200 tablet，版本号是 1.06。

或者，如果能用 `dmidecode`，则（以 `root`）运行以下命令，来得知目前刷入的 BIOS 版本：

	dmidecode -s bios-version

运行最新 BIOS 的 T400 上，它的输出结果为 `7UET94WW (3.24 )`。
