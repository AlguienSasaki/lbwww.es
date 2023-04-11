---
title: Огляд коду
x-toc-enable: true
...

репозиторії libreboot
===================

Інформацію про те, хто працює над libreboot і хто керує проектом, можна
знайти на [who.uk.md](who.uk.md)

Проект `libreboot` має 3 основні сховища Git:

* Система побудови: <https://codeberg.org/libreboot/lbmk>
* Веб-сайт (+документація): <https://codeberg.org/libreboot/lbwww>
* Зображення (для веб-сайта): <https://codeberg.org/libreboot/lbwww-img>

You can also *browse* lbmk on Libreboot's own cgit instance, though it is not
intended for development (use codeberg for that):\
<https://browse.libreboot.org/lbmk.git/>

Libreboot was previously using NotABug, but it had continued reliability
issues due to HTTP 500 errors being returned, largely in the evenings, most
likely because too many people were on it; it was decided that Libreboot
needed something more stable, so now Libreboot is hosted on codeberg. See:
[announcement of move to codeberg, 8 April 2023](news/codeberg.md)

Є також ці програми, розміщені в проекті Libreboot, і libreboot
або рекомендує їх, або використовує їх:

The `ich9utils` project is now available under `util/ich9utils` in lbmk, and
lbmk uses *that*, but the old standalone repository is still available on
notabug (bucts is also there):

* Bucts (утиліта): <https://notabug.org/libreboot/bucts>
* ich9utils (утиліта): <https://notabug.org/libreboot/ich9utils>

Ви можете завантажити будь-яке з цих сховищ, внести будь-які зміни, і
потім надіслати свої зміни, дотримуючись інструкцій нижче.

Рекомендовано створювати libreboot (усі його частини) у дистрибутиві
Linux. Наприклад, система збірки (lbmk) не перевірена на системах BSD.
Встановіть `git` у вашій системі Linux, і завантажте одне із сховищ.

Розробка libreboot виконується за допомогою системи контролю версій Git.
Зверніться до [офіційної документації Git](https://git-scm.com/doc), якщо ви не
знаєте, як користуватися Git.

Репозиторій `bucts` розміщено в проекті libreboot, оскільки оригінальний
репозиторій на `stuge.se` більше не доступний, коли ми перевіряли останній раз.
Програма `bucts` була написана Пітером Стьюджем. Вам знадобляться `bucts`, якщо ви прошиваєте
внутрішньо libreboot ROM на ThinkPad X60 або T60, на якому зараз працює
невільний Lenovo BIOS. Інструкції щодо цього доступні тут:\
[посібники зі встановлення libreboot](docs/install/)

Репозиторій `ich9utils` активно використовується системою збирання `lbmk`. Однак
ви також можете завантажити `ich9utils` самостійно та використовувати його. Він генерує ICH9M
дескриптор+GbE образи для GM45 ThinkPad, які використовують південний міст ICH9M. Він
також може працювати з іншими системами, що використовують ту саму платформу/чіпсет.
Документація для `ich9utils` доступна тут:\
[документація ich9utils](docs/install/ich9utils.md)

lbmk (libreboot-make)
---------------------

Це основна система збирання в libreboot. Можна сказати, що `lbmk` *це*
libreboot! Завантажте репозиторій Git:

	git clone https://codeberg.org/libreboot/lbmk

Команда `git`, показана вище, завантажить систему збірки libreboot `lbmk`.
Потім ви можете перейти до цього так:

	cd lbmk

Внесіть будь-які зміни, які забажаєте, або просто побудуйте. Щоб отримати вказівки щодо
збирання `lbmk`, зверніться до [інструкцій зі збирання](docs/build/).

Інформація про саму систему збірки та про те, як вона працює, доступна в
[посібнику обслуговування lbmk](docs/maintain/).

lbwww та lbwww-img
-------------------

*Весь* веб-сайт і документація libreboot розміщені в репозиторії Git.
Завантажте так:

	git clone https://codeberg.org/libreboot/lbwww

Зображення розміщені на <https://av.libreboot.org/> і доступні в окремому
сховищі:

	git clone https://codeberg.org/libreboot/lbwww-img

Вносьте будь-які зміни, які забажаєте. Дивіться нотатки нижче про те, як надсилати виправлення.

Весь веб-сайт написаний у Markdown, зокрема його версія Pandoc.
Статичні сторінки HTML створюються за допомогою [Untitled](https://untitled.vimuser.org/).
Лія Роу, засновниця libreboot, також є засновницею проекту генератор статичних сайтів
Untitled.

Якщо хочете, ви можете налаштувати локальний HTTP-сервер і створити власну локальну
версію веб-сайту. Зауважте, що зображення все одно будуть посилатися на ті, що
розміщені на <https://av.libreboot.org/>, тому будь-які зображення, які ви додаєте до `lbwww-img`
не відображатимуться на вашому локальному сайті `lbwww`, якщо ви зробите, щоб посилання на зображення (для
зображень, які ви додаєте) посилались на `av.libreboot.org`. Однак необхідно, щоб такі
зображення розміщувалися на av.libreboot.org.

Тому, якщо ви бажаєте додати зображення на веб-сайт, надішліть їх також до
репозиторія `lbwww-img`, із посиланням на них
<https://av.libreboot.org/шлях/до/вашого/нового/зображення/в/lbwww-img> для кожного з них.
Коли його буде поєднано на веб-сайті libreboot, ваші зображення з'являться в реальному часі.

Для цілей розробки ви можете спочатку створити локальні посилання на зображення, а
потім налаштувати URL-адреси, коли надсилатимете документацію/патчі веб-сайту.

На веб-сайті Untitled є інструкції щодо налаштування локальної версії
веб-сайту. Завантажте untitled, і в своєму каталозі `untitled` створіть каталог
під назвою `www/`, потім увійдіть у каталог www і клонуйте сховище `lbwww`
там. Налаштуйте локальний HTTP-сервер відповідним чином.

Знову ж таки, інструкції для цього доступні на веб-сайті Untitled.

Ім'я не вимагається
-----------------

Внески, які ви робите, реєструються публічно в репозиторії Git, доступ
до якого мають всі. Це включає ім'я та електронну адресу
учасника.

У Git для імені автора та електронної адреси вам не потрібно використовувати
ідентифікаційні дані. Ви можете використовувати `libreboot Contributor`, а свою електронну адресу можна
вказати як contributor@libreboot.org. Вам дозволено це робити, якщо
ви бажаєте зберегти конфіденційність. Ми віримо в конфіденційність. Якщо ви вирішите залишитися
анонімними, ми врахуємо це.

Звичайно, ви можете використовувати будь-яке ім'я та/або адресу електронної пошти.

З юридичної точки зору всі авторські права є автоматичними відповідно до Бернської конвенції
міжнародного авторського права. Немає значення, яке ім'я, чи дійсно ви навіть
заявляєте про авторське право (але ми вимагаємо наявності певного
ліцензування авторського права - докладніше про це на цій же сторінці).

Якщо ви використовуєте інше ім'я та адресу електронної пошти у своїх комітах/патчах, то
маєте бути досить анонімним. використовуйте
[git log](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
та [git show](https://git-scm.com/docs/git-show), щоб підтвердити це перед тим, як ви
надсилаєте зміни до загальнодоступного сховища Git.

Ліцензії (для учасників)
--------

Обов'язково вільно ліцензуйте свою роботу, за вільною ліцензією. Libreboot більше не
встановлює довільні обмеження на те, які ліцензії приймаються, і багато
інших ліцензій вже існує. Ми перевіримо ваш внесок і розкажемо вам, якщо з ним
виникли проблеми (наприклад, немає ліцензії).

*Завжди* декларуйте ліцензію на свою роботу! Недекларування ліцензії означає, що
за замовчуванням застосовуються обмежувальні закони про авторське право, які зроблять вашу роботу
захищеною власністю, підпадаючи під усі ті самі обмеження.

Ліцензія MIT є хорошою для початку, і вона є бажаною ліцензією
для всіх нових робіт у Libreboot, але ми не вибагливі. Libreboot історично
використовував ліцензування GNU, таке як GPL; багато з цього залишилося, і, ймовірно, залишиться.
Це ваша робота; очевидно, якщо ви використовуєте існуючу роботу,
може мати сенс використовувати ту саму ліцензію для вашого внеску, для сумісності
ліцензії.

Ви можете знайти типові приклади ліцензій
[тут](https://opensource.org/licenses).

Якщо ви *виходите* на основі існуючого твору, важливо, щоб ваша ліцензія (на ваш внесок)
була сумісна з ліцензуванням твору, з якого
ваш був отриманий. Ліцензія MIT хороша, оскільки вона широко сумісна
з багатьма іншими ліцензіями та надає багато свобод (наприклад, свободу
субліцензування), яких немає в інших ліцензіях:

<https://opensource.org/licenses/MIT>

Надсилайте виправлення
------------

Створіть обліковий запис на <https://codeberg.org/> і перейдіть (увійшовши в систему) до
репозиторію, над яким ви хочете працювати. Натисніть *Fork*, і у вашому обліковому записі,
ви матимете власне сховище libreboot. Клонуйте свій репозиторій, внесіть у нього
будь-які зміни, а потім надішліть їх у свій репозиторій у своєму обліковому
записі на NotABug. Ви також можете зробити це на новій гілці, якщо хочете.

У своєму обліковому записі Codeberg, ви можете перейти до офіційного репозиторія libreboot
і надіслати запит на отримання. Принцип роботи подібний до інших популярних веб-платформ
Git, якими люди користуються сьогодні.

Ви можете відправити свої патчі туди. Крім того, ви можете увійти на
IRC-канал libreboot і повідомити канал, які виправлення ви хочете бути переглянутими, якщо у вас
є власне сховище Git з виправленнями.

Після того, як ви подасте Pull Request, розробники libreboot отримають сповіщення
електронною поштою. Якщо ви не отримаєте достатньо швидкої відповіді від проекту, ви
також можете повідомити проект через канал `#libreboot` на Libera Chat.

Інший спосіб подати виправлення - це напряму надіслати Лії Роу електронною поштою:
[leah@libreboot.org](mailto:leah@libreboot.org) - це адреса електронної пошти проекту Лії.

Однак, для прозорості процесу перевірки коду, ми рекомендуємо на даний момент
використовувати Codeberg.

Mirrors of lbmk.git
===================

The `lbmk` repository contains Libreboot's automated build system, which
produces Libreboot releases (including compiled ROM images).

You can run `git clone` on any of these links (the links are also clickable,
to view changes in your Web browser):

* <https://gitea.treehouse.systems/libreboot/lbmk>
* <https://0xacab.org/libreboot/lbmk/>
* <https://framagit.org/libreboot/libreboot>
* <https://gitlab.com/libreboot/lbmk>
* <https://pagure.io/libreboot>
* <https://rocketgit.com/libreboot/libreboot>

lbwww.git mirror
----------------

The `lbwww` repository contains Markdown files (pandoc variant), for use
with the [Untitled Static Site Generator](https://untitled.vimuser.org/); this
is what Libreboot uses to provide HTML web pages, *including* the page that
you are reading right now!

You can run `git clone` on these links, and/or click to view changes in your
Web browser. See:

* <https://gitea.treehouse.systems/libreboot/lbwww>
* <https://0xacab.org/libreboot/lbwww>
* <https://framagit.org/libreboot/lbwww/>
* <https://gitlab.com/libreboot/lbwww>
* <https://rocketgit.com/libreboot/lbwww>

NOTE: The `lbwww-img` repository is not generally provided, on mirrors, as
those are just image files which you can find on <https://av.libreboot.org>
and it is not the intention of the Libreboot project to bog down *mirrors*
with additional traffic by hosting images.

Notabug repositories
====================

Commits that go to codeberg are also still pushed to notabug, in addition to
the other mirrors. Notabug is considered a *mirror* since 8 April 2023, when
Libreboot's main development site moved to *Codeberg*.

OLD notabug repos are still pushed to as backup, but the codeberg mirror is
considered to be main/official now, as of the [announcement on 8
April 2023](news/codeberg.md). See:

* Build system: <https://notabug.org/libreboot/lbmk>
* Website (+docs): <https://notabug.org/libreboot/lbwww>
* Images (for website): <https://notabug.org/libreboot/lbwww-img>

For sending patches, it is now preferred that you use *codeberg*. Technically,
pull requests are still possible via Notabug. While Notabug still exists,
Libreboot patches will continue be pushed there, mirroring what gets pushed
on Notabug.

Because pull requests and issues were available on notabug in the past, it
makes sense to keep them open, though we ask that you send to codeberg. If
they were to be closed on notabug, existing PRs and issues won't be visible
anymore either, so they have to stay open.
