% Зашифрований /boot/ на LUKSv2 тепер є можливим в Libreboot GRUB (PHC argon2 імпортовано)
% Лія Роу
% 20 серпня 2023 року

<img tabindex=1 class="r" src="https://av.libreboot.org/grub/argon2.jpg" /><span class="f"><img src="https://av.libreboot.org/grub/argon2.jpg" /></span>

Вступ
------

Корисне навантаження GRUB підтримувало LUKSv2 протягом довгого часу, але тільки з
старосвітовським методом формування ключа PBKDF2; більшість Linux dm-crypt встановлень на LUKSv2
використовує засноване на argon2 формування ключа, який GRUB не підтримував. Це значить або:
використовуйте LUKSv2 з PBKDF2 (менш безпечно), або LUKSv1 (так само), в GRUB.

На сьогодняшній день, Libreboot тепер підтримує формування ключа argon2 в своїй версії GRUB,
яка запропонована в якості корисного навантаження coreboot в системі побудови Libreboot, і
в випусках Libreboot.

Виправлення Libreboot argon2 засновані на [цьому репозиторії
AUR](https://aur.archlinux.org/cgit/aur.git/tree/?h=grub-improved-luks2-git&id=1c7932d90f1f62d0fd5485c5eb8ad79fa4c2f50d),
який виправляв GRUB 2.06, і виправлення були перебазованими для використання з GRUB 2.12,
який Libreboot використовує; перебазування було виконано [Ніколасом
Джонсоном](https://nicholasjohnson.ch/). Ніколас написав мені електронною поштою, щоб сказати, що це було
зроблено, і я потім злила роботу Ніколаса в Libreboot. Дякую,
Ніколас! Подяка також іде до [Аксель](https://axelen.xyz/), хто є автором
оригінальної роботи, яку Ніколас імпортував з Archlinux AUR.

The work on AUR, and Nicholas's update based on it, was ultimately based on
the work done by Patrick Steinhardt for the GRUB project, importing PHC Argon2.

### Чому це має значення?

Libreboot раніше документував, як завантажувати дистрибутиви з зашифрованого `/boot`,
що є добрим для безпеки, тому що складніше скомпрометувати машину, яка має
зашифрований `/boot` (що містить ядро linux), ніж якщо він не є зашифрованим,
подібно більшості встановлень LUKS. В доповнення до цього, ви можете розглянути [посилення
GRUB](../docs/linux/grub_hardening.md), кроки подібні перевірці підпису GPG
вашого встановленого ядра Linux.

Ці виправлення імпортують реалізацію PHC argon2 в версію Libreboot
GRUB:

* <https://browse.libreboot.org/lbmk.git/commit/?id=2c0c521e2f15776fd604f8da3bc924dec95e1fd1>
* <https://browse.libreboot.org/lbmk.git/commit/?id=fd6025321c4ae35e69a75b45d21bfbfb4eb2b3a0>
* <https://browse.libreboot.org/lbmk.git/commit/?id=438bf2c9b113eab11439c9c65449e269e5b4b095>

Argon2 є новішим формуванням ключа, якому надається перевага на сучасних встановленнях LUKSv2. Дуже рекомендовано, щоб
ви *оновились* на argon2id, в точності, для вашого
встановлення.

### Як отримати це

Це є недоступним для поточного випуску Libreboot 20230625, але буде
доступним в наступному випуску Libreboot. Наразі, ви можете зібрати образ ROM
самостійно з системи побудови Libreboot, lbmk, подібним чином:

[Як побудувати образи ROM Libreboot з джерела](../docs/build/index.uk.md)

Наступне читання
----------------

### Реалізація PHC argon2

Це реалізація argon2 для посилання, що тепер використовується Libreboot,
і апстрім проект для цього розміщено тут:

<https://github.com/P-H-C/phc-winner-argon2>

### Стаття Метью Гарретт

[PSA: оновіть ваш LUKS метод формування
ключа](https://mjg59.dreamwidth.org/66429.html) від Метью Гарретт, розповідає
більше про важливість безпечного формування ключа (в точності argon2id)
на зашифрованих встановленнях Linux.

Завжди використовуйте шифрування!
