---
title: Libreboot Özgür/Açık Kaynaklı BIOS/UEFI Yazılımını İndirin
x-toc-enable: true
...

Yeni sürümler [ana haberler bölümünde](news/) duyurulur.

Libreboot geliştirmesiyle daha çok ilgileniyorsanız, Git depolarına bağlantılar da içeren [libreboot geliştirme sayfasına](../git.md) gidin. [/docs/maintain/](docs/maintain/) sayfasında Libreboot'un nasıl bir araya getirildiği ve nasıl bakım yapılacağı açıklanmaktadır. Libreboot'u kaynak koddan derlemek istiyorsanız, [bu sayfayı okuyun](docs/build/).

Libreboot Önyüklenmiş Olarak Satın Alın
--------------------------------------

Profesyonel kurulum istiyorsanız, Minifree Ltd seçili donanımlarda [Libreboot önyüklenmiş](https://minifree.org/) cihazlar satmaktadır ve ayrıca makinenizi göndererek Libreboot kurulumu yaptırmak istiyorsanız [Libreboot önyükleme hizmeti](https://minifree.org/product/installation-service/) sunmaktadır.

Libreboot'un kurucusu ve baş geliştiricisi Leah Rowe aynı zamanda Minifree Ltd'yi de işletmektedir; satışlar Libreboot projesi için finansman sağlamaktadır.

Güvenlik Uyarısı
---------------

**KURULUMDAN ÖNCE LÜTFEN [BU TALİMATLARI OKUYUN](docs/install/ivy_has_common.md),
AKSİ TAKDİRDE CİHAZINIZI BOZMUŞ OLABİLİRSİNİZ!! - Lütfen bağlantıya tıklayın ve oradaki talimatları
takip edin. Hatırlatma olarak, [işte bağlantı tekrar burada](docs/install/ivy_has_common.md).**

GPG İmza Anahtarı
----------------

**En son sürüm, `testing` dizini altındaki Libreboot 25.04'dır.**

### YENİ ANAHTAR

Tam anahtar parmak izi: `8BB1 F7D2 8CF7 696D BF4F  7192 5C65 4067 D383 B1FF`

Bu anahtar, 20240126 sürümünden *sonraki* Libreboot sürümleri içindir. 2024 yılındaki tüm Libreboot sürümleri için geçerlidir ve (erken iptal edilmezse) 26 Aralık 2028'de sona erecektir.

Anahtarı buradan indirin: [lbkey.asc](lbkey.asc)

Libreboot sürümleri GPG kullanılarak imzalanır.

### ESKİ ANAHTAR

Tam anahtar parmak izi: `98CC DDF8 E560 47F4 75C0  44BD D0C6 2464 FA8B 4856`

Bu anahtar, 20160907 sürümünden *sonraki* ve Libreboot 20240126 sürümüne kadar olan Libreboot sürümleri içindir. Bu anahtar Aralık 2023'te *sona erdi*, bu nedenle Libreboot 20240126'dan sonraki sürümler için *yeni* anahtarı (yukarıya bakın) kullanmalısınız.

Anahtarı buradan indirin: [lbkey.asc](lbkeyold.asc)

Libreboot sürümleri GPG kullanılarak imzalanır.

### ESKİ ESKİ ANAHTAR:

Bu anahtar Libreboot 20160907 ve 20160907'den eski sürümler içindir:

Tam anahtar parmak izi: CDC9 CAE3 2CB4 B7FC 84FD  C804 969A 9795 05E8 C5B2

GPG anahtarı ayrıca bu dışa aktarılmış pubkey dökümüyle de indirilebilir: [lbkeyold.asc](lbkeyoldold.asc).

    sha512sum -c sha512sum.txt
    gpg --verify sha512sum.txt.sig

Git Deposu
---------

Bu sayfada düzenli sürüm arşivlerine bağlantılar listelenmiştir.

Ancak, Libreboot'un en güncel sürümü için indirebileceğiniz bir Git deposu vardır. Buraya gidin:

[Libreboot'u Git'ten nasıl indirirsiniz](git.md)

HTTPS Yansıları {#https}
--------------

**En son sürüm, `testing` dizini altındaki Libreboot 25.04'dır.**

Bu yansılar TLS (https://) şifrelemesi kullandığı için önerilir.

Libreboot'u bu yansılardan indirebilirsiniz:

* <https://mirror.math.princeton.edu/pub/libreboot/> (Princeton
üniversitesi, ABD)
* <https://mirror.shapovalov.website/libreboot/> (shapovalov.website, Ukrayna)
* <https://www.mirrorservice.org/sites/libreboot.org/release/> (Kent
Üniversitesi, BK)
* <https://mirrors.mit.edu/libreboot/> (MIT üniversitesi, ABD)
* <https://mirror.koddos.net/libreboot/> (koddos.net, Hollanda)
* <https://mirror-hk.koddos.net/libreboot/> (koddos.net, Hong Kong)
* <https://mirror.cyberbits.eu/libreboot/> (cyberbits.eu, Fransa)

RSYNC Yansıları {#rsync}
--------------

Aşağıdaki rsync yansıları herkese açık olarak kullanılabilir:

* <rsync://rsync.mirrorservice.org/libreboot.org/release/> (Kent Üniversitesi,
BK)
* <rsync://mirror.math.princeton.edu/pub/libreboot/> (Princeton üniversitesi, ABD)
* <rsync://rsync.shapovalov.website/libreboot/> (shapovalov.website, Ukrayna)
* <rsync://ftp.linux.ro/libreboot/> (linux.ro, Romanya)
* <rsync://mirror.koddos.net/libreboot/> (koddos.net, Hollanda)
* <rsync://mirror-hk.koddos.net/libreboot/> (koddos.net, Hong Kong)

Bir yansı mı çalıştırıyorsunuz? Libreboot projesiyle iletişime geçin, bağlantı bu sayfaya eklenecektir!

Rsync yansınızı web sunucunuz üzerinden kullanıma sunabilir ve ayrıca *kendi* yansınızı rsync üzerinden erişilebilir olacak şekilde yapılandırabilirsiniz. İnternette bir rsync sunucusunun nasıl kurulacağını gösteren birçok kaynak vardır.

Kendi rsync yansınızı nasıl oluşturursunuz:

Libreboot'un tüm sürüm arşivlerini yansılamak için kullanışlıdır. Bir rsync komutunu crontab'a koyabilir ve dosyaları web sunucunuzdaki bir dizine çekebilirsiniz.

Tüm seti yansılayacaksanız, en az 25GiB ayırmanız önerilir. Libreboot'un rsync'i şu anda yaklaşık 12GiB'dir, bu nedenle 25GiB ayırmak size gelecek için bol alan sağlayacaktır. Minimum olarak, Libreboot yansınız için en az 15-20GiB alan kullanılabilir olduğundan emin olmalısınız.

Resmi bir yansı barındırmak istiyorsanız, *libreboot.org yansısını kullanmanız şiddetle önerilir*. Aksi takdirde, sadece kendi yerel yansınızı oluşturmak istiyorsanız, libreboot.org'dan senkronize olan diğer yansılardan birini kullanmalısınız.

Yansıyı oluşturmadan önce, web sunucunuzda bir dizin oluşturun. Örneğin:

    mkdir /var/www/html/libreboot/

Şimdi rsync'i çalıştırabilirsiniz, örneğin:

    rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

Bunu saatlik bir crontab'a koyabilirsiniz. Örneğin:

    crontab -e

Ardından crontab'a bu satırı ekleyin ve kaydedin/çıkın (saatlik crontab):

    0 * * * * rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

**Yukarıdaki rsync komutunda her yolun sonunda ileri eğik çizginin (/) olması son derece önemlidir.
Aksi takdirde, rsync çok garip davranacaktır.**

**NOT: `rsync.libreboot.org` doğrudan halka açık değildir, yalnızca
IP'leri beyaz listeye alınanlar erişebilir. Bant genişliği nedeniyle, libreboot.org'da çalışan
güvenlik duvarı, belirli IP'ler dışında gelen rsync isteklerini engeller.**

**Bir rsync yansısı çalıştırmak istiyorsanız, yukarıdaki üçüncü taraf yansılarından birinden senkronize edin
ve yansınızı kurun. Ardından IP adreslerinizin rsync kullanımı için beyaz listeye alınması için
Leah Rowe ile iletişime geçebilirsiniz - IP adresleri rsync ana bilgisayarınızın DNS A/AAAA
kayıtlarıyla eşleşiyorsa, bu kullanılabilir. libreboot.org'da saatlik bir crontab'da çalışan bir betik,
beyaz listeye alınmış rsync yansılarının A/AAAA kayıtlarını otomatik olarak alır ve güvenlik duvarından
geçmelerine izin veren kurallar ekler.**

Rsync yansınızı düzenli olarak güncel tutmak istiyorsanız, bir crontab'a ekleyebilirsiniz. Bu sayfa size crontab'ın nasıl kullanılacağını anlatır:
<https://man7.org/linux/man-pages/man5/crontab.5.html>

HTTP Yansıları {#http}
-------------

**En son sürüm, `testing` dizini altındaki Libreboot 25.04'dır.**

UYARI: bu yansılar HTTPS olmadığı için şifrelenmemiştir. Trafiğiniz
kötü niyetli kişiler tarafından müdahaleye uğrayabilir. Doğru anahtara sahip olduğunuzu
varsayarak, özellikle GPG imzalarını kontrol ettiğinizden emin olun. Tabii ki,
HTTPS kullansanız bile bunu yapmalısınız.

* <http://mirror.linux.ro/libreboot/> (linux.ro, Romanya)
* <http://mirror.helium.in-berlin.de/libreboot/> (in-berlin.de, Almanya)

FTP Yansıları {#ftp}
------------

**En son sürüm, `testing` dizini altındaki Libreboot 25.04'dır.**

UYARI: FTP de HTTP gibi şifrelenmemiştir. Aynı riskler mevcuttur.

* <ftp://ftp.mirrorservice.org/sites/libreboot.org/release/> (Kent
Üniversitesi, BK)
* <ftp://ftp.linux.ro/libreboot/> (linux.ro, Romanya)

Statik Olarak Bağlanmış
----------------------

Libreboot, bazı sürümlerde mevcut kaynak koddan derlenmiş statik olarak bağlanmış yürütülebilir dosyalar içerir. Bu yürütülebilir dosyalar, birçok Linux dağıtımında çalışabilmeleri için belirli kitaplıkları içlerinde barındırır.

GPL v2'ye uymak için, Libreboot projesi tarafından kaynak ISO'lar sağlanır. Bu kaynak ISO'ları `rsync` yansılarındaki `ccsource` dizininde bulabilirsiniz.

20160907 sürümünden sonraki Libreboot sürümleri statik olarak bağlanmış ikili dosyalar dağıtmaz. Bunun yerine, bu sürümler normal Libreboot kaynak kodu arşivlerinin yeterli olduğu önceden derlenmiş ROM imajları dışında yalnızca kaynak içerir. Bu yeni sürümler bunun yerine derleme bağımlılıklarının kurulumunu otomatikleştirir ve çeşitli yardımcı programları kaynak koddan derlemek için belgelerde talimatlar sunar.

Bu yürütülebilir dosyalar `flashprog` gibi yardımcı programlardır. 
