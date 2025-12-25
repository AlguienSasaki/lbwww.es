---
title: Скачать Libreboot — свободную/открытую прошивку BIOS/UEFI
x-toc-enable: true
...

Новые релизы публикуются в [основном разделе новостей](news/).

Если вас больше интересует разработка Libreboot, перейдите на
[страницу разработки Libreboot](../git.md), где также находятся ссылки на Git-репозитории.  
Страница [/docs/maintain/](docs/maintain/) описывает, как устроен Libreboot и как его поддерживать.  
Если вы хотите собрать Libreboot из исходников, [прочтите эту страницу](docs/build/).

## Купить Libreboot с предустановкой

Если вы хотите профессиональную установку, компания Minifree Ltd продаёт [Libreboot с предустановкой](https://minifree.org/) на выбранное оборудование,  
а также предлагает [услугу по установке Libreboot](https://minifree.org/product/installation-service/), если вы хотите отправить своё устройство на установку.

Лиа Роу, основатель и ведущий разработчик Libreboot, также владеет и управляет Minifree Ltd.  
Продажи помогают финансировать проект Libreboot.

## Предупреждение о безопасности

[ОБЯЗАТЕЛЬНО ПРОЧИТАЙТЕ ЭТИ ИНСТРУКЦИИ ПЕРЕД УСТАНОВКОЙ](docs/install/ivy_has_common.md),  
ИНАЧЕ ВЫ МОЖЕТЕ ВЫВЕСТИ УСТРОЙСТВО ИЗ СТРОЯ! — Пожалуйста, перейдите по ссылке и следуйте инструкциям.  
На всякий случай, [вот та же ссылка](docs/install/ivy_has_common.md).

## GPG-ключ для подписи

Последний выпуск — Libreboot 25.06, находится в каталоге stable.

### НОВЫЙ КЛЮЧ

Полный отпечаток ключа: 8BB1 F7D2 8CF7 696D BF4F  7192 5C65 4067 D383 B1FF

Этот ключ используется для выпусков Libreboot после релиза 20240126.  
Он применяется ко всем релизам с 2024 года и действует до 26 декабря 2028 года (если не будет отозван ранее).

Скачать ключ: [lbkey.asc](lbkey.asc)

Релизы Libreboot подписываются с помощью GPG.

### СТАРЫЙ КЛЮЧ

Полный отпечаток ключа: 98CC DDF8 E560 47F4 75C0  44BD D0C6 2464 FA8B 4856

Этот ключ использовался для релизов после 20160907 и до 20240126.  
Ключ истёк в декабре 2023, поэтому следует использовать новый ключ (см. выше) для всех релизов после Libreboot 20240126.

Скачать ключ: [lbkeyold.asc](lbkeyold.asc)

Libreboot подписывает релизы через GPG.

### СТАРЫЙ-ПРЕДЫДУЩИЙ КЛЮЧ

Этот ключ использовался для Libreboot 20160907 и более старых релизов.

Полный отпечаток: CDC9 CAE3 2CB4 B7FC 84FD  C804 969A 9795 05E8 C5B2

Ключ также доступен как экспортированный дамп открытого ключа: [lbkeyoldold.asc](lbkeyoldold.asc)

Проверка подписи:

`sh
sha512sum -c sha512sum.txt
gpg --verify sha512sum.txt.sig

## Репозиторий Git

Ссылки на архивы стабильных релизов перечислены на этой странице.

Однако, если вы хотите самую свежую, постоянно обновляемую версию Libreboot,  
вы можете скачать её из Git-репозитория. Подробнее:

[Как скачать Libreboot из Git](git.md)

## Зеркала HTTPS {#https}

Последний выпуск — Libreboot 25.06, находится в каталоге `stable`.

Рекомендуется использовать следующие зеркала, так как они используют шифрование TLS (https://):

- <https://rsync.libreboot.org/> (Libreboot.org official mirror, UK)
- <https://mirror.math.princeton.edu/pub/libreboot/> (Университет Принстона, США)  
- <https://mirror.shapovalov.website/libreboot/> (shapovalov.website, Украина)  
- <https://www.mirrorservice.org/sites/libreboot.org/release/> (Университет Кента, Великобритания)  
- <https://mirrors.mit.edu/libreboot/> (MIT, США)  
- <https://mirror.koddos.net/libreboot/> (koddos.net, Нидерланды)  
- <https://mirror-hk.koddos.net/libreboot/> (koddos.net, Гонконг)  
- <https://mirror.cyberbits.eu/libreboot/> (cyberbits.eu, Франция)  
- <https://mirror.diyarciftci.xyz/libreboot/> (diyarciftci.xyz, Deutschland)
- <http://5gxsihf4vjdhimiotrhchihrfujoanondrplizofwparrtmgbwsafhid.onion/libreboot/> (diyarciftci.xyz Hidden Mirror)
- <http://iue4ubnjvskey5gop7n3odkylyjvelb5sl7z4cx6xjwdnthv2hba.b32.i2p/libreboot/> (diyarciftci.xyz Hidden Mirror)
- <https://ca.mirrors.cicku.me/libreboot/> (cicku, Canada)
- <https://au.mirrors.cicku.me/libreboot/> (cicku, Australia)
- <https://in.mirrors.cicku.me/libreboot/> (cicku, India)
- <https://za.mirrors.cicku.me/libreboot/> (cicku, South Africa)

NOTE: the "hidden" mirrors are encrypted by virtue of the network they're on,
so they're still listed under the HTTPS (encrypted) section, despite actually
using plain HTTP, because i2p and/or Tor provide encryption instead; on these
networks, it's normal to use plain HTTP.

## Зеркала RSYNC {#rsync}

Следующие зеркала доступны через rsync:

- <rsync://rsync.mirrorservice.org/libreboot.org/release/> (Университет Кента, Великобритания)  
- <rsync://mirror.math.princeton.edu/pub/libreboot/> (Университет Принстона, США)  
- <rsync://rsync.shapovalov.website/libreboot/> (shapovalov.website, Украина)  
- <rsync://ftp.linux.ro/libreboot/> (linux.ro, Румыния)  
- <rsync://mirror.koddos.net/libreboot/> (koddos.net, Нидерланды)  
- <rsync://mirror-hk.koddos.net/libreboot/> (koddos.net, Гонконг)  
- <rsync://mirror.diyarciftci.xyz/libreboot/> (diyarciftci.xyz, Deutschland)
- <rsync://rsync.libreboot.org/mirrormirror/> (Libreboot.org)

### Хотите создать своё зеркало?

Свяжитесь с проектом Libreboot, и ваша ссылка будет добавлена сюда!

Вы можете сделать зеркало rsync доступным через свой веб-сервер,  
а также настроить собственное зеркало для доступа через rsync.

#### Как создать зеркало Libreboot через rsync:

Это удобно, если вы хотите зеркалировать весь архив релизов Libreboot.  
Вы можете добавить команду rsync в crontab, чтобы периодически обновлять зеркало.

Рекомендуется выделить как минимум 25GiB дискового пространства.  
Сейчас объём rsync-зеркала Libreboot около 12GiB, но с запасом на будущее лучше 25GiB.  
Минимум — 15–20GiB.

> **Рекомендуется использовать зеркало libreboot.org**,  
> если вы хотите хостить официальное зеркало.  
> В противном случае — выбирайте другое зеркало, синхронизирующееся с libreboot.org.

Перед тем, как создать зеркало, создайте каталог на вашем веб-сервере. Например:

  mkdir /var/www/html/libreboot/

Теперь вы можете запустить rsync, например:

  rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

Вы можете добавить это в почасовой crontab. Например:

  crontab -e

Затем в crontab добавьте эту строку и сохраните/выйдите (почасовой crontab):

  0 * * * * rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

Очень важно иметь конечный слеш (/) в конце каждого пути
в команде rsync выше. Иначе rsync будет вести себя очень странно.

Если вы хотите регулярно обновлять ваше rsync-зеркало, вы можете добавить это в crontab.
Эта страница рассказывает, как использовать crontab:
<https://man7.org/linux/man-pages/man5/crontab.5.html>

**NOTE: Some people use systemd timers now instead of crontab. That's OK
too! Whatever floats your boat.**

HTTP зеркала {#http}
------------

Последний релиз — Libreboot 25.06, в директории stable.

ВНИМАНИЕ: эти зеркала не используют HTTPS, значит, трафик не зашифрован.
Ваш трафик может подвергаться вмешательству злоумышленников. Особенно
обязательно проверяйте GPG-подписи, при условии что у вас есть нужный ключ.
Конечно, это следует делать всегда, даже при использовании HTTPS.

* <http://mirror.linux.ro/libreboot/> (linux.ro, Румыния)
* <http://mirror.helium.in-berlin.de/libreboot/> (in-berlin.de, Германия)

FTP зеркала {#ftp}
-----------

Последний релиз — Libreboot 25.06, в директории stable.

ВНИМАНИЕ: FTP тоже нешифрованный, как и HTTP. Присутствуют те же риски.

* <ftp://ftp.mirrorservice.org/sites/libreboot.org/release/> (Университет Кента, Великобритания)
* <ftp://ftp.linux.ro/libreboot/> (linux.ro, Румыния)

Статически слинкованные
------------------

Libreboot включает статически слинкованные исполняемые файлы в некоторых релизах, собранные из
доступного исходного кода. Эти исполняемые файлы содержат встроенные библиотеки,
что позволяет им работать на многих дистрибутивах Linux.

Для соблюдения GPL v2 исходные ISO предоставляются проектом Libreboot.
Вы можете найти эти исходные ISO в директории ccsource
на rsync-зеркалах.

Релизы Libreboot после версии 20160907 не распространяют статически слинкованные
бинарники. Вместо этого эти релизы содержат только исходники, кроме заранее скомпилированных ROM
образов, для которых обычные архивы исходников Libreboot подходят. Эти более новые
релизы автоматизируют установку зависимостей сборки, с инструкциями
в документации по сборке различных утилит из исходников.

Эти исполняемые файлы — это утилиты, такие как `flashprog`.

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
