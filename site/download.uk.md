---
title: Завантаження
x-toc-enable: true
...

Нові випуски оголошуються в [основній секції новин](news/).

Якщо ви більше зацікавлені в розробці libreboot, пройдіть на
[сторінку розробки libreboot](../git.md), яка також включає посилання на
репозиторії Git. Сторінка на [/docs/maintain/](docs/maintain/) описує те, як
Libreboot складається разом, і як підтримувати його. Якщо ви бажаєте зібрати
Libreboot із джерельного кода, [прочитайте цю сторінку](docs/build/).

Buy Libreboot pre-installed
--------------

If you want professional installation, Minifree Ltd sells [Libreboot
pre-installed](https://minifree.org/) on select hardware, and it also provides
a [Libreboot preinstall service](https://minifree.org/product/installation-service/)
if you want to send your machine in to have Libreboot installed for you.

Leah Rowe, the founder and lead developer of Libreboot, also owns and
operates Minifree Ltd; sales provide funding for the Libreboot project.

Safety warning
--------------

**IMPORTANT ADVICE: [PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING/UPDATING
LIBREBOOT](news/safety.md).**

Код підпису GPG
---------------

**Останнім випуском є Libreboot 20240225, в директорії `testing`.**

### НОВИЙ КЛЮЧ

Повний відбиток ключа: `8BB1 F7D2 8CF7 696D BF4F  7192 5C65 4067 D383 B1FF`

Вищезазначений ключ для Libreboot 20240126, та наступних випусків. This key
is applicable to any release made on or after the date: 28 December 2023. It
will expire on 26 December 2028.

Завантажте ключ тут: [lbkey.asc](lbkey.asc)

Випуски Libreboot підписані з використанням GPG.

### СТАРИЙ КЛЮЧ

Повний відбиток ключа: `98CC DDF8 E560 47F4 75C0  44BD D0C6 2464 FA8B 4856`

This key is for Libreboot releases *after* the 20160907 release, and up
to the Libreboot 20240225 release. This key *expired* during December 2023,
so you should use the *newer* key (see above) for the releases after
Libreboot 20240126.

Завантажте ключ тут: [lbkey.asc](lbkeyold.asc)

Випуски Libreboot підписані з використанням GPG.

### СТАРИЙ СТАРИЙ КЛЮЧ:

Цей ключ для Libreboot 20160907 та всіх старіших випусків:

Повний відбиток ключа: CDC9 CAE3 2CB4 B7FC 84FD  C804 969A 9795 05E8 C5B2

Ключ GPG також може бути завантажений разом із цим експортованим дампом
публічного ключа: [lbkeyold.asc](lbkeyoldold.asc).

	sha512sum -c sha512sum.txt
	gpg --verify sha512sum.txt.sig

Репозиторій Git
--------------

Посилання на архіви регулярних випусків зазначені на цій сторінці.

Однак, для абсолютно найновішої версії Libreboot,
існує репозиторії Git, з якого можна завантажити. Ідіть сюди:

[Як завантажити Libreboot через Git](git.md)

Дзеркала HTTPS {#https}
-------------

**Останнім випуском є Libreboot 20240225, в директорії `testing`.**

Дані дзеркала є рекомендованими, оскільки використовують TLS (https://) шифрування.

Ви можете завантажити Libreboot через дані дзеркала:

* <https://www.mirrorservice.org/sites/libreboot.org/release/> (Кентський
університет, Великобританія)
* <https://mirrors.mit.edu/libreboot/> (Університет МТІ, США)
* <https://mirror.math.princeton.edu/pub/libreboot/> (Прінстонський
університет, США)
* <https://mirror.shapovalov.website/libreboot/> (shapovalov.website, Україна)
* <https://mirror.koddos.net/libreboot/> (koddos.net, Нідерланди)
* <https://mirror-hk.koddos.net/libreboot/> (koddos.net, Гонконг)
* <https://mirror.cyberbits.eu/libreboot/> (cyberbits.eu, Франція)

Дзеркала RSYNC {#rsync}
-------------

Наступні дзеркала rsync доступні публічно:

* <rsync://rsync.mirrorservice.org/libreboot.org/release/> (Кентський університет,
Великобританія)
* <rsync://mirror.math.princeton.edu/pub/libreboot/> (Прінстонський університет, США)
* <rsync://rsync.shapovalov.website/libreboot/> (shapovalov.website, Україна)
* <rsync://ftp.linux.ro/libreboot/> (linux.ro, Румунія)
* <rsync://mirror.koddos.net/libreboot/> (koddos.net, Нідерланди)
* <rsync://mirror-hk.koddos.net/libreboot/> (koddos.net, Гонконг)

Ви підтримуєте роботу дзеркала? Зв'яжіться з проектом libreboot, і посилання буде
додано до цієї сторінки!

Ви можете зробити своє дзеркало rsync доступним через свій веб-сервер, а також налаштувати
ваше *власне* дзеркало бути доступним через rsync. Є багато онлайн-ресурсів,
які показують вам те, як налаштувати сервер rsync.

Як створити ваше власне дзеркало rsync:

Корисно для відзеркалювання повного набору архівів випусків Libreboot. Ви можете розмістити
команду rsync в crontab та витягувать файли в директорію на
вашому веб-сервері.

Якщо ви збираєтесь відзеркалювати повний набір, рекомендовано, щоб вами було виділено
хоча би 25 ГБ. Rsync Libreboot наразі приблизно 12 ГБ, таким чином виділення 25 ГБ
забезпечить вам багато місця на майбутнє. Мінімально, ви маєте переконатись, що
хоча би 15-20 ГБ простору доступно, для вашого дзеркала Libreboot.

*Настійно рекомендується, щоб ви використовували дзеркало libreboot.org*, якщо бажаєте
розміщувати офіційне дзеркало. В іншому випадку, якщо ви просто бажаєте створити своє власне
локальне дзеркало, вам варто використовувати одне з інших дзеркал, яке синхронізується з
libreboot.org.

Перед створенням дзеркала, зробіть директорію на вашому веб-сервері. Для 
прикладу:

	mkdir /var/www/html/libreboot/

Тепер ви можете виконувати rsync, для прикладу:

	rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

Ви могли би розмістить це в щогодинний crontab. Для прикладу:

	crontab -e

Потім в crontab, додайте цей рядок і збережіться/вийдіть (щогодинний crontab):

	0 * * * * rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

**Це надзвичайно важливо, щоб мати в кінці косу лінію (/) в кінці кожного шляху,
в вищезазначеній команді rsync. В інакшому випадку, rsync буде поводитись дуже дивно.**

**ПОМІТКА: `rsync.libreboot.org` не є напряму доступним для громадськості, окрім
тих, чиї IP у білому списку. Через пропускну здатність, Брандмауер, який працює
на libreboot.org, блокує вхідні запити rsync, окрім окремих IP.**

**Якщо ви бажаєте запустити дзеркало rsync, синхронізуйте з одного з дзеркал третіх сторін
вище і встановіть своє дзеркало. Ви можете потім зв'язатись з Лією Роу, щоб мати ваші адреси
IP внесеним в білий список для використання rsync - якщо адреси IP відповідають DNS A/AAAA
записам для вашого хоста rsync, це може бути використано. Сценарій виконується в щогодинному
crontab на libreboot.org, який отримує A/AAAA записи внесених в білий список дзеркал
rsync, автоматично додаючи правила, які дозволяють їм проходити через
брандмауер.**

Якщо ви бажаєте регулярно тримати свої дзеркала rsync оновленими, ви можете додати це до
crontab. Ця сторінка розповідає вам, як використовувати crontab:
<https://man7.org/linux/man-pages/man5/crontab.5.html>

Дзеркала HTTP {#http}
------------

**Останнім випуском є Libreboot 20240225, під директорією `testing`.**

УВАГА: ці дзеркала є не-HTTPS, що означає, що вони
незашифровані. Ваш трафік може бути об'єктом втручання
противників. Особливо ретельно переконайтесь, щоб перевірити підписи GPG, передбачаючи, що
ви маєте правильний ключ. Звісно, вам варто зробити це в будь-якому випадку, навіть
при використанні HTTPS.

* <http://mirror.linux.ro/libreboot/> (linux.ro, Румунія)
* <http://mirror.helium.in-berlin.de/libreboot/> (in-berlin.de, Німеччина)

Дзеркала FTP {#ftp}
-----------

**Останнім випуском є Libreboot 20240225, під директорією `testing`.**

УВАГА: FTP є також незашифрованим, подібно HTTP. Ті ж самі ризики присутні.

* <ftp://ftp.mirrorservice.org/sites/libreboot.org/release/> (Кентський
університет, Великобританія)
* <ftp://ftp.linux.ro/libreboot/> (linux.ro, Румунія)

Статично зв'язані
------------------

Libreboot включає статично зв'язані виконувані файли в деяких випусках, побудовані з
доступного джерельного кода. Ці виконувані файли мають деякі бібліотеки, вбудовані в
них, так щоб виконувані файли працювали на багатьох дистрибутивах Linux.

Для дотримання GPL v2, джерельні ISO постачаються
проектом Libreboot. Ви можете знайти ці джерельні ISO  в директорії `ccsource`
на дзеркалах `rsync`.

Попередні випуски Libreboot 20160907 не розповсюджують статично зв'язані двійкові
файли. Натомість ці випуски є лише вихідними кодами, окрім попередньо скомпільованих образів
ПЗП, для яких достатньо звичайних архівів джерельного коду Libreboot. Ці новіші
випуски натомість автоматизують встановлення залежностей побудови, з інструкцієї
в документації для побудови різних утиліт з джерельного коду.

Ці виконувані файли є утилітами, подібними `flashprog`.
