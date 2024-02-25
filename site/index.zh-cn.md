---
title: Libreboot 项目
x-toc-enable: true
...

*Libreboot* 项目基于 coreboot 提供了[自由且开源](https://writefreesoftware.org/zh-cn/)的引导固件，替代了特定基于 Intel/AMD x86 及 ARM 的主板（包括笔记本和桌面计算机）上的专有 BIOS/UEFI 固件。它首先对硬件（如内存控制器、CPU、外设）进行初始化，然后为操作系统启动 bootloader。本项目对 [Linux](docs/linux/) 和 [BSD](docs/bsd/) 支持良好。寻求帮助，可以前往 [Libera](https://libera.chat/) IRC 上的 [\#libreboot](https://web.libera.chat/#libreboot) 频道。

<img tabindex=1 class="r" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

You can also [buy Libreboot preinstalled](https://minifree.org/) from Minifree Ltd,
on select hardware, aswell as send your compatible hardware
for [Libreboot preinstallation](https://minifree.org/product/installation-service/).
The founder and lead developer of Libreboot, Leah Rowe, also owns and operates
Minifree; sales provide funding for Libreboot.

**新版发布: 最新版本 Libreboot 20240225 已在 2024 年 02 月 25 日发布。详见: [Libreboot 20240225 发布公告](news/libreboot20240225.md).**

为什么要使用 *Libreboot*?
----------------------------

Libreboot 赋予了你[自由](https://writefreesoftware.org/)，而这等自由，是你用其他引导固件得不到的。同时，它的启动速度更加快，[安全性也更加高](docs/linux/grub_hardening.md)。在各种情况下使用，它都十分强大，具有高度[可配置性](docs/maintain/)。

*我们*相信，不受限制地[研究、分享、修改及使用软件](https://writefreesoftware.org/)的自由，是每个人都必须享有的基本人权的一部分。这时，*软件自由*至关重要。你的自由至关重要。教育至关重要。[修理权](https://yewtu.be/watch?v=Npd_xDuNi9k)至关重要。尽管许多人在用[自由的操作系统](https://www.openbsd.org/)，但他们用的引导固件却是专有（非自由）的。专有固件常常[包含](faq.html#intel)了[后门](faq.html#amd)，并且也可能出问题。2013 年 12 月，我们建立了 Libreboot 项目，目的是让不懂技术的用户能使用 coreboot 固件。

Libreboot 项目使用 [coreboot](https://www.coreboot.org/) 来[初始化硬件](https://doc.coreboot.org/getting_started/architecture.html)。对大部分不懂技术的用户来说，coreboot 是出了名地难安装；它只处理了基础的初始化，然后跳转进入单独的 [payload](https://doc.coreboot.org/payloads.html) 程序（例如 [GRUB](https://www.gnu.org/software/grub/)、[Tianocore](https://www.tianocore.org/)），而后者也需要进行配置。*Libreboot 解决了这样的问题*；他是一个 *coreboot 发行版*，配有[自动构建系统](docs/build/)，能够构建完整的 *ROM 镜像*，从而让安装更加稳定。另有文档可参考。

Libreboot 不是 coreboot 的分支
-----------------------------------

<img tabindex=1 class="l" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

事实上，Libreboot 对每一块主板，都尽可能保持与*标准*的 coreboot 接近，但 Libreboot 构建系统也自动提供了许多不同类型的配置。

Libreboot 是一个 *coreboot 发行版*，就好比 *Alpine Linux* 是一个 *Linux 发行版*。如果你想要从零开始构建 ROM 镜像，那你需要对 coreboot、GRUB 以及其他所需软件进行专业级别的配置，才能准备好 ROM 镜像。有了 *Libreboot*，你只需要下载 Git 仓库或者源代码归档，然后运行 a script，接着就能构建整个 ROM 镜像。一套自动构建系统，名为 `lbmk`（Libreboot Make），将自动构建 ROM 镜像，而无需任何用户输入或干预。配置已经提前完成。

如果你要构建常规的 coreboot，而不使用 Libreboot 的自动构建系统，那么需要有很多的干预以及相当的技术知识，才能写出一份能工作的配置。

Libreboot 的常规二进制版本，提供了这些预编译的 ROM 镜像。你可以轻松安装它们，而无需特别的知识和技能，只要能遵循[写给非技术用户的简单指南](docs/install/)。

如何帮忙
-----------

<img tabindex=1 class="l" style="max-width:15%;" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

要帮忙的话，*最*最好的方式，就是通过提交配置文件，来为 Libreboot *添加*新的主板。coreboot 支持的任何东西，都能并入 Libreboot，并且带有 ROM 镜像。见：

* [申请成为主板维护者/测试者](docs/maintain/testing.md)
* [新主板移植指南](docs/maintain/porting.md)
* [Libreboot 构建系统文档](docs/maintain/)

然后，就是构建系统的维护（见下）以及重要的*文档*。文档十分重要，在任何项目都是如此。

*用户支持*也十分重要。多瞧一瞧 IRC，如果你有能力帮别人解决问题（或者愿意跟他们一起学习），那对本项目的贡献会很大。许多人也在 reddit 版块 `r/libreboot` 寻求用户支持。

你可以检查[缺陷追踪系统](https://codeberg.org/libreboot/lbmk/issues)列出的缺陷。

如果你发现了一个缺陷，并且有解决方案，[这里说明了发布补丁的方法](git.md)，你也可以提交报告。同时，本站完全使用 Markdown 编写，并托管在了一个[单独的仓库](https://codeberg.org/libreboot/lbwww)，你可以在那里发送补丁。

所有开发方面的讨论以及用户支持，都是在 IRC 频道上完成的。了解更多，可以查看[联系](contact.md)页面。

libreboot.org 需要翻译
--------------------------------------

Libreboot 目前有乌克兰语和法语的网页翻译（但两个语言都还没翻译完所有页面）。

如果你想帮忙翻译，你可以翻译网页、更新已有翻译并提交你的译本。请阅读下面的指南：

[如何提交 libreboot.org 翻译](news/translations.md)

即使已经有人在进行某个语言的翻译了，我们也总是欢迎更多人。多多益善！
