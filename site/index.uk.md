---
title: завантажувальну прошивку BIOS/UEFI прошивку
x-toc-enable: true
...

Проект *Libreboot* надає
[вільну](https://writefreesoftware.org/) *завантажувальну
прошивку*, яка ініціалізує апаратне забезпечення (наприклад, контролер пам'яті, ЦП,
периферію) на [конкретних цілях Intel/AMD x86 та ARM](docs/install/#which-systems-are-supported-by-libreboot), що
потім розпочинає завантажувач для вашої операційної системи. [Linux](docs/linux/)
та [BSD](docs/bsd/) добре підтримуються. Це заміняє пропрієтарну BIOS/UEFI
прошивку. Допомога доступна
через [\#libreboot](https://web.libera.chat/#libreboot)
на [Libera](https://libera.chat/) IRC.

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>

Libreboot provides GNU boot loader "[GRUB](docs/linux/)" and SeaBIOS payloads
on x86/x86\_64
Intel/AMD motherboards, and a [U-Boot UEFI payload](docs/uboot/) *for coreboot*
on ARM64(Aarch64) motherboards.
An [x86/x86\_64 U-Boot UEFI payload](docs/uboot/uboot-x86.md) is also available
on some boards. The x86, x86\_64 and arm64 U-Boot payloads provide a lightweight
UEFI boot implementation, which can boot many Linux distros and BSD systems.
The SeaBIOS and GRUB payloads also boot Linux/BSD systems. Which one you use
depends on your preferences. Libreboot's design incorporates all of these boot
methods in a single image, so you can choose which one you use at boot time,
and more payloads (e.g. Linux kexec payload) are planned for future releases.

**НОВИЙ ВИПУСК: Останній випуск Libreboot 20241206, випущено 6 December 2024.
Дивіться: [Оголошення про випуск Libreboot 20241206](news/libreboot20241206.md).**

You can also [buy Libreboot preinstalled](https://minifree.org/) from Minifree Ltd,
on select hardware, as well as send your compatible hardware
for [Libreboot preinstallation](https://minifree.org/product/installation-service/).
The founder and lead developer of Libreboot, Leah Rowe, also owns and operates
Minifree; sales provide funding for Libreboot.

Чому вам варто використовувати *Libreboot*?
----------------------------

<img tabindex=1 class="l" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

Libreboot надає вам [свободи](https://writefreesoftware.org/), які в
іншому випадку ви не можете отримати з більшістю інших завантажувальних
прошивок. Він надзвичайно [потужний](docs/linux/grub_hardening.md)
та [налаштовується](docs/maintain/) для багатьох випадків використання.

У вас є права. Право на конфіденційність, свобода мислення, свобода висловлювання
та право читати. В цьому контексті, Libreboot надає вам ці права.
Ваша свобода має значення.
[Право на ремонт](https://en.wikipedia.org/wiki/Right_to_repair) має значення.
Багато людей використовують пропрієтарну (невільну)
завантажувальну прошивку, навіть якщо вони використовують [вільну операційну систему](https://www.openbsd.org/).
Пропрієтарна прошивка часто [містить](faq.uk.html#intel) [лазівки](faq.uk.html#amd),
та може бути глючною. Проект Libreboot було засновано в грудні 2013 року, з
явною метою зробити прошивку coreboot доступною для нетехнічних користувачів.

Проект Libreboot використовує [coreboot](https://www.coreboot.org/) для [ініціалізації апаратного забезпечення](https://doc.coreboot.org/getting_started/architecture.html).
Coreboot помітно складний для встановлення для більшості нетехнічних користувачів; він
виконує тільки базову ініціалізацію та перестрибує до окремої програми
[корисного навантаження](https://doc.coreboot.org/payloads.html) (наприклад,
[GRUB](https://www.gnu.org/software/grub/),
[Tianocore](https://www.tianocore.org/)), які також мають бути налаштованими.
*Програмне забезпечення Libreboot вирішує цю проблему*; це *дистрибутив coreboot* з
[автоматизованою системою побудови](docs/build/index.uk.md), яка збирає завершені *образи ROM*, для
більш надійної установки. Документація надається.

Чим Libreboot відрізняється від звичайного coreboot?
---------------------------------------------

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

Таким же самим чином, як *Debian* це дистрибутив Linux, Libreboot це
*дистрибутив coreboot*. Якщо ви хочете зібрати образ ROM з нуля, вам
інакше довелось би виконати налаштування експертного рівня coreboot, GRUB та
будь-якого іншого потрібного програмного забезпечення, для підготування образа ROM. З *Libreboot*,
ви можете буквально завантажити з Git або архіву джерельного коду, та запустити a script, і це
побудує всі образи ROM. Автоматизована система побудови, названа `lbmk`
(Libreboot MaKe), збирає ці образи ROM автоматично, без будь-якого вводу користувача
або потрібного втручання. Налаштування вже виконано заздалегідь.

Якщо би ви збирали звичайний coreboot, не використовуючи автоматизовану систему побудови Libreboot,
це вимагало би набагато більше втручання та гідних технічних
знань для створення робочої конфігурації.

Звичайні бінарні випуски Libreboot надають ці
образи ROM попередньо зібраними, і ви можете просто встановити їх, не маючи спеціальних
знань або навичок, окрім можливості
слідувати [спрощеним інструкціям, написаним для нетехнічних
користувачів](docs/install/).

Як допомогти
-----------

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.vimuser.org/uboot.png" /><span class="f"><img src="https://av.vimuser.org/uboot.png" /></span>

*Єдиний* найбільший шлях, яким ви можете допомогти є *додавання* нових материнських плат до Libreboot,
за допомогою відправки конфігурації. Що завгодно, що підтримує coreboot може бути інтегровано в
Libreboot, з образами ROM наданими в випусках. Дивіться:

* [Подача на становлення супровідником/випробувачем плати](docs/maintain/testing.md)
* [Керівництво перенесення для нових материнських плат](docs/maintain/porting.uk.md)
* [Документація системи побудови Libreboot](docs/maintain/)

Після цього, є обслуговування системи побудови (дивіться зверху), та *документація*,
до якої ми ставимося серйозно. Документація є критичною, в будь-якому проекті.

*Користувацька ідтримка* є також критичною. Тримайтесь поряд на IRC, і якщо ви компетентні
допомогти комусь з їх проблемним питанням (або досить вправні, щоб навчитись з ними), це є
чудовою послугою для проекту. Багато людей також запитує підтримки користувачів
на subreddit `r/libreboot`.

Ви можете перевірити помилки, перелічені
в [системі відстеження помилок](https://codeberg.org/libreboot/lbmk/issues).

<img tabindex=1 class="l" style="max-width:15%;" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

Якщо ви помітили помилку і маєте виправлення, [ось інструкції щодо надсилання виправлень](git.uk.md), а також ви можете повідомити про це. Також, весь цей веб-сайт
написаний Markdown та розміщений в [окремому
сховищі](https://codeberg.org/libreboot/lbwww), де ви можете надсилати виправлення.

Усі обговорення розробки та підтримка користувачів відбуваються на IRC
каналі. Більше інформації на [сторінці зворотнього зв'язку](contact.uk.md).

Переклади потрібні, для libreboot.org
--------------------------------------

Libreboot наразі має перекладеними веб-сторінки українською та французькою (але не
для всіх сторінок, ще, жодною з мов).

Якщо ви бажаєте допомогти з перекладами, ви можете перекласти сторінки, оновити
існуючи переклади та надати ваші перекладені версії. Для інструкцій, будь ласка
прочитайте:

[Як надати переклади для libreboot.org](news/translations.md)

Навіть якщо хтось вже працює над перекладами даною мовою, ми можемо
завжди використовувати багато людей. Чим більше, тим краще!
