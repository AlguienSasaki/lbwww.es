---
title: Проект libreboot
x-toc-enable: true
...

Проект *Libreboot* надає
[вільну](freedom-status.md) *завантажувальну
прошивку*, яка ініціалізує апаратне забезпечення (наприклад, контролер пам'яті, ЦП,
периферію) на [конкретних цілях Intel/AMD x86 та ARM](docs/hardware/), що
потім розпочинає завантажувач для вашої операційної системи. [Linux](docs/linux/)
та [BSD](docs/bsd/) добре підтримуються. Це заміняє пропрієтарну BIOS/UEFI
прошивку. Допомога доступна
через [\#libreboot](https://web.libera.chat/#libreboot)
на [Libera](https://libera.chat/) IRC.

<img tabindex=1 class="r" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

**НОВИЙ ВИПУСК: Останній випуск Libreboot 20230423, випущено 23 квітня 2023.
Дивіться: [Оголошення про випуск Libreboot 20230423](news/libreboot20230423.md).**

Чому вам варто використовувати *libreboot*?
----------------------------

Libreboot надає вам свободи, які в іншому випадку ви не можете отримати з більшістью інших
завантажувальних прошивок. Він
надзвичайно [потужний](docs/linux/grub_hardening.md)
та [налаштовується](docs/maintain/) для багатьох випадків використання.

У вас є права. Право на конфіденційність, свобода мислення, свобода висловлювання
та право читати. В цьому контексті, Libreboot надає вам ці права.
Ваша свобода має значення.
[Право на ремонт](https://yewtu.be/watch?v=Npd_xDuNi9k) має значення.
Багато людей використовує пропрієтарну (невільну)
завантажувальну прошивку, навіть якщо вони використовують [вільну операційну систему](https://www.openbsd.org/).
Пропрієтарна прошивка часто [містить](faq.html#intel) [лазівки](faq.html#amd),
та може бути глючною. Проект libreboot було засновано в грудні 2013 року, з
явною метою зробити прошивку coreboot доступною для нетехнічних користувачів.

Проект Libreboot використовує [coreboot](https://www.coreboot.org/) для [ініціалізації апаратного забезпечення](https://doc.coreboot.org/getting_started/architecture.html).
Coreboot помітно складний для встановлення для більшості нетехнічних користувачів; він
виконує тільки базову ініціалізацію та перестрибує до окремої програми
[корисного навантаження](https://doc.coreboot.org/payloads.html) (наприклад,
[GRUB](https://www.gnu.org/software/grub/),
[Tianocore](https://www.tianocore.org/)), які також мають бути налаштованими.
*Програмне забезпечення libreboot вирішує цю проблему*; це *дистрибутив coreboot* з
[автоматизованою системою побудови](docs/build/), яка збирає завершені *образи ROM*, для
більш надійної установки. Документація надається.

Чим libreboot відрізняється від звичайного coreboot?
---------------------------------------------

<img tabindex=1 class="l" style="max-width:15%;" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

Таким же самим чином, як *Debian* це дистрибутив Linux, Libreboot це
*дистрибутив coreboot*. Якщо ви хочете зібрати образ ROM з нуля, вам
інакше довелось би виконати налаштування експертного рівня coreboot, GRUB та
будь-якого іншого потрібного програмного забезпечення, для підготування образа ROM. З *libreboot*,
ви можете буквально завантажити з Git або архіву джерельного коду, та запустити `make`, і це
побудує всі образи ROM. Автоматизована система побудови, названа `lbmk`
(Libreboot MaKe), збирає ці образи ROM автоматично, без будь-якого вводу користувача
або потрібного втручання. Налаштування вже виконано заздалегідь.

Якщо би ви збирали звичайний coreboot, не використовуючи автоматизовану систему побудови libreboot,
це вимагало би набагато більше втручання та гідних технічних
знань для створення робочої конфігурації.

Звичайні бінарні випуски Libreboot надають ці
образи ROM попередньо зібраними, і ви можете просто встановити їх, не маючи спеціальних
знань або навичок, окрім можливості
слідувати [спрощеним інструкціям, написаним для нетехнічних
користувачів](docs/install/).

Як допомогти
-----------

The *single* biggest way you can help it to *add* new mainboards to Libreboot,
by submitting a config. Anything coreboot supports can be integrated in
Libreboot, with ROM images provided in releases. See:

* [Apply to become a board maintainer/tester](docs/maintain/testing.md)
* [Porting guide for new mainboards](docs/maintain/porting.md)
* [Libreboot build system documentation](docs/maintain/)

After that, there is build system maintenance (see above), and *documentation*
which we take seriously. Documentation is critical, in any project.

*User support* is also critical. Stick around on IRC, and if you're competent
to help someone with their issue (or wily enough to learn with them), that is
a great service to the project. A lot of people also ask for user support
on the `r/libreboot` subreddit.

Ви можете перевірити помилки, перелічені
в [системі відстеження помилок](https://codeberg.org/libreboot/lbmk/issues).

Якщо ви помітили помилку і маєте виправлення, [ось інструкції щодо надсилання виправлень](git.md), а також ви можете повідомити про це. Також, весь цей веб-сайт
написаний Markdown та розміщений в [окремому
репозиторії](https://codeberg.org/libreboot/lbwww), де ви можете надсилати виправлення.

Усі обговорення розробки та підтримка користувачів відбуваються на IRC
каналі. Більше інформації на [сторінці зворотнього зв'язку](contact.md).
