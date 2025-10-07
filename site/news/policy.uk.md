% Політика зменшення бінарних блобів
% Лія Роу
% 4 січня 2022 року (оновлено 15 листопада 2022 року)

Вступ
-----

Політика Libreboot полягає в тому, щоб надати настільки, наскільки можливо, свободи
кожному користувачу, на кожному підтримуваному апаратному забезпеченні та *підтримувати
стільки апаратного забезпечення з coreboot, наскільки це можливо*; це означає, що ви повинні
мати можливість вивчати, змінювати та *ділитися* всім джерельним кодом, документацією
чи іншими подібними ресурсами, які роблять Libreboot таким, яким він є. Простіше кажучи, ви повинні
мати *контроль* над своїми власними обчисленнями.

*Мета* Libreboot полягає в тому, щоб
зробити саме це та допомогти якомога більшій кількості людей, автоматизувавши
конфігурацію, компіляцію та встановлення *coreboot* для *нетехнічних*
користувачів, ще більше спростивши це для звичайного користувача, надаючи користувачам
дружні інструкції для всього. По суті, Libreboot - це *дистрибутив
coreboot*, приблизно так само, як *Alpine Linux* є дистрибутивом Linux!

Метою цього документа є окреслення того, як це досягається, і як
проект працює на цій основі. *Цей* документ здебільшого стосується
ідеології, тому він (переважно) нетехнічний; для отримання технічної інформації
ви можете звернутися до [документації системи збірки Libreboot](../docs/maintain/).

Поточний обсяг проекту
----------------------

Проект libreboot стосується того, що входить до основної мікросхеми завантажувальної флеш-пам'яті,
але є й інші компоненти мікропрограми, які слід взяти до уваги, про що йдеться
в [поширених запитаннях щодо libreboot](../faq.uk.md#яке-ще-мікропрограмне-забезпечення-існує-за-межами-libreboot).

Найбільш критичні з них це:

* Прошивка вбудованого контролера (EC)
* Прошивка жорстких дисків/твердотілих накопичувачів
* Прошивка Intel Management Engine / AMD PSP

### Що таке двійковий блоб?

Двійковий блоб у цьому контексті - це будь-який виконуваний файл, для якого не існує вихідного коду,
який ви не можете досліджувати та змінювати розумним чином. За визначенням,
усі такі блоби є *пропрієтарними* за своєю природою, і їх слід уникати, якщо це можливо.

Конкретні двійкові блоби також є проблематичними в більшості систем coreboot, але вони
відрізняються для кожної машини. Дізнайтесь більше в розділі поширених запитань і на цій сторінці про те,
як ми працюємо з двійковими блобами в проекті Libreboot.

Для інформації про Intel Management Engine та AMD PSP зверніться до поширених запитань.

Політика *зменшення* блобів
---------------------------

### Конфігурації за замовчуванням

Coreboot, на якому Libreboot базується, є здебільшого вільним програмним забезпеченням, але
на деяких платформах вимагає двійкових блобів. Найпоширенішим прикладом може бути raminit
(ініціалізація контролера пам'яті) або ініціалізація кадрового буфера відео. Прошивка
coreboot використовує двійкові блоби для деяких з цих завдань, на деяких материнських платах,
але деякі материнські плати з coreboot можна ініціалізувати з 100% вільним джерельним
кодом, який ви можете перевірити та скомпілювати для свого використання.

Libreboot вирішує цю ситуацію *суворо* та *принципово*. Природа
цього - це те, що ви збираєтесь прочитати.

Проект libreboot має наступну політику:

* Якщо блоб *можна* уникнути, його слід уникати. Наприклад, якщо ініціалізація VGA ROM
  в іншому випадку працює краще, але coreboot має *вільний* код ініціалізації
  для певного графічного пристрою, цей код слід використовувати в libreboot під
  час створення образу ROM. Подібним чином, якщо *ініціалізація контролера пам'яті* можлива
  за допомогою бінарного блобу *або* вільного коду в coreboot, *вільний* код
  слід використовувати в ROM, створених системою збірки Libreboot, а *блоб*
  для raminit не слід використовувати; однак, якщо вільний код ініціалізації недоступний
  для зазначеного raminit, це дозволено, і система збірки Libreboot використовуватиме
  *блоб*.
* Необхідно звернути увагу на деякі нюанси: у деяких конфігураціях ноутбуків або настільних комп'ютерів
  зазвичай буде *два* графічних пристрої (наприклад, чіп nvidia та
  чіп intel, використовуючи технологію nvidia optimus technology, на ноутбуці). Можливо,
  один із них має вільний код ініціалізації в coreboot, а інший - ні. Абсолютно
  прийнятно і бажано, щоб libreboot підтримував обидва пристрої та
  розміщував необхідний двійковий блоб на тому, якому бракує власної
  ініціалізації.
* Виняток зроблено для оновлень мікрокоду ЦП: вони дозволені та фактично
  *обов'язкові* відповідно до політики libreboot. Ці оновлення виправляють помилки ЦП, у тому
  числі помилки безпеки, і оскільки ЦП уже має невільний мікрокод, записаний в
  ROM в будь-якому випадку, єдиний вибір - *x86* або *зламаний x86*. Таким чином, libreboot
  дозволить лише конфігурації материнської плати coreboot, де
  *ввімкнено* оновлення мікрокоду, якщо доступно для ЦП на цій системній платі.
  [Releases after 20230423 will provide separate ROM images with microcode
  excluded, alongside the default ones that include microcode.](microcode.md)
* Intel Management Engine: у документації libreboot *повинні* бути написані
  слова, щоб розповісти людям, як *нейтралізувати* ME, якщо це можливо на даній дошці.
  Програма `me_cleaner` є дуже корисною та забезпечує набагато безпечнішу конфігурацію
  ME.
* Бінарні блоби *ніколи* не слід видаляти, навіть якщо вони не використовуються. 
  У проекті coreboot доступний набір субмодулів `3rdparty` з бінарними блобами
  для завдань ініціалізації на багатьох платах. *Усі* вони повинні бути включені до випусків
  libreboot, навіть якщо вони не використовуються. Таким чином, навіть якщо система збірки Libreboot
  ще не підтримує певну плату, хтось, хто завантажує libreboot, все одно
  може внести зміни у свою локальну версію системи збірки, якщо
  забажає, щоб надати конфігурацію свого апаратного забезпечення.

Загалом, застосовано здоровий глузд. Наприклад, винятком
із мінімізації може бути те, що *блоб* raminit та *вільний* raminit доступні, але
*вільний* так зламано, що його не можна використовувати. У такій ситуації натомість
слід використовувати той, що з блобом, тому що в іншому випадку користувач може повернутися до
повністю пропрієтарної системи замість використання coreboot (через libreboot).
*Деякі* свободи *краще, ніж жодних*.

Нова прагматична політика Libreboot також може призвести до того, що в майбутньому більше людей стануть
розробниками coreboot, виступаючи в якості важливого *містка* між
*ним* і нетехнічними людьми, яким просто потрібна допомога, щоб розпочати роботу.

### Налаштування

Наведені вище принципи мають застосовуватися до конфігурацій *за замовчуванням*. Однак libreboot
має бути *конфігурованим*, дозволяючи користувачеві робити все, що заманеться.

Цілком природно, що користувач може захотіти створити *менш* вільний параметр, ніж
стандартний у libreboot. Це цілком прийнятно; свобода вище,
і її слід заохочувати, але *свободу вибору* користувача
також слід поважати та пристосовуватись до неї.

Іншими словами, не читайте лекції користувачеві. Просто спробуйте допомогти їм у
їхній проблемі! Метою проекту libreboot є просто зробити coreboot більш
доступним для нетехнічних користувачів.

КАТАЛОГ СВОБОДИ
---------------

Бажано бачити світ, де все апаратне та програмне забезпечення є вільним, під
тією ж ідеологією, що й проект Libreboot. Обладнання!?

Так, обладнання. RISC-V є чудовим прикладом сучасної спроби вільного апаратного забезпечення, яке
часто називають *Апаратним забезпеченням з відкритим кодом*.
Це ISA для виробництва мікропроцесора. Уже існує багато реальних
реалізацій, які можна використовувати, і їх буде лише
більше.

Таке *апаратне забезпечення* ще знаходиться в зародковому стані. Ми повинні почати проект, який
буде каталогізувати статус різних зусиль, у тому числі на апаратному рівні (навіть на
рівні кремнію). Такі рухи, як OSHW і Право на ремонт (Right To Repair), є надзвичайно
важливими, включно з нашим власним рухом, який інакше зазвичай менше думатиме про свободу апаратного
забезпечення (хоча йому справді, справді
варто!)

Одного дня ми житимемо у світі, де будь-хто зможе виготовити власні чіпи,
включаючи процесори, а також будь-які інші типи мікросхем. Зусилля зробити домашнє виробництво
чіпів реальністю зараз знаходяться в зародковому стані, але такі зусилля існують,
наприклад, робота, виконана Семом Зелофом і проектом Libre Silicon:

* <https://www.youtube.com/channel/UC7E8-0Ou69hwScPW1_fQApA>
* <http://sam.zeloof.xyz/>
* <https://libresilicon.com/>

(Сем буквально виробляє процесори в своєму гаражі)

More detailed insight about microcode
-------------------------------------

To be clear: it is preferable that microcode be free.
Not including CPU microcode updates is an absolute disaster for system
stability and security, so Libreboot *includes microcode updates by default, in
all modern release images, where possible to do so*.

The CPU already has microcode burned into mask ROM. The microcode configures
logic gates in the CPU, to implement an instruction set, via special *decoders*
which are fixed-function; it is not possible, for example, to implement a RISCV
ISA on an otherwise x86 processor. It is only possible for the microcode to
implement x86, or *broken* x86, and the default microcode is almost always
*broken x86* on Intel/AMD CPUs; it is inevitable, due to the complexity of
these processors.

These processors provide a way to supply microcode *updates*. These updates
are volatile, and consequently must be applied during every boot cycle. The
updates fix stability/reliability/security bugs, and their *absence*
is *technically incorrect*, so you are strongly advised to install them.
Examples of where these updates fix bugs: on ASUS KCMA-D8/KGPE-D16
and ThinkPad X200/T400/T500/W500/X200T/X200/R500/X301, the updates make
hardware-based virtualization (via `kvm`) completely stable, where it would
otherwise lead to a kernel panic. They allow those same thinkpads to be run with
high CPU usage and I/O (RAM usage), without crashing (otherwise, it's very
likely to encounter a kernel panic caused by a *Machine Check Exception*).

Not including these updates will result in an unstable/undefined state. Intel
themselves define which bugs affect which CPUs, and they define workarounds, or
provide fixes in microcode. Based on this, software such as the Linux kernel
can work around those bugs/quirks. Also, upstream versions of the Linux kernel
can update the microcode at boot time (however, it is recommend still to do it
from coreboot, for more stable memory controller initialization or “raminit”).
Similar can be said about AMD CPUs.

Once upon a time, Libreboot *excluded* microcode updates by default, but this
lead to broken behaviour. Here are some examples:

<https://browse.libreboot.org/lbmk.git/plain/resources/coreboot/default/patches/0012-fix-speedstep-on-x200-t400-Revert-cpu-intel-model_10.patch?id=9938fa14b1bf54db37c0c18bdfec051cae41448e>

<https://browse.libreboot.org/lbmk.git/plain/resources/coreboot/default/patches/0018-Revert-cpu-intel-Configure-IA32_FEATURE_CONTROL-for-.patch?id=4b7be665968b67463ec36b9afc7e8736be0c9b51>

These patches revert *bug fixes* in coreboot, fixes that happen to break other
functionality but only when microcode updates are excluded. The most
technically correct solution is to *not* apply the above patches, and instead
supply microcode updates!

You *need* microcode updates, or you will have a broken CPU; broken, because
it literally behaves differently than it's supposed to, so software will have
unpredictable bugs that could even cause data corruption - or worse.

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
