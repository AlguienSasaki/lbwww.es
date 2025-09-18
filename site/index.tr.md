---
title: Özgür ve Açık Kaynaklı BIOS/UEFI Önyükleme Yazılımı
x-toc-enable: true
...

*Libreboot*, Debian'ın bir *Linux dağıtımı* olması gibi, bir *[coreboot dağıtımıdır](docs/maintain)*. Libreboot, [belirli Intel/AMD x86 ve ARM tabanlı anakartlarda](docs/install/#which-systems-are-supported-by-libreboot) özel mülk BIOS/UEFI yazılımının yerini alan, coreboot tabanlı [özgür, açık kaynaklı](https://writefreesoftware.org/) (*libre*) önyükleme yazılımı sağlar. Bu anakartlar dizüstü ve masaüstü bilgisayarları içerir. Donanımı başlatır (örn. bellek denetleyicisi, CPU, çevre birimleri) ve işletim sisteminiz için bir önyükleyici başlatır. [Linux](docs/linux/) ve [BSD](docs/bsd/) iyi desteklenmektedir. [Libera](https://libera.chat/) IRC'deki [\#libreboot](https://web.libera.chat/#libreboot) kanalından yardım alabilirsiniz.

<img tabindex=1 class="r" alt="Libreboot çalıştıran ThinkPad T480" title="ThinkPad T480" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img alt="Libreboot çalıştıran ThinkPad T480" src="https://av.libreboot.org/t480/t480.jpg" /></span>

On [8 September 2025](news/spi.html), Libreboot became an official [Associated
Project](https://www.spi-inc.org/projects/libreboot/) at SPI ([Software in
the Public Interest](https://www.spi-inc.org/)).

Libreboot, x86/x86_64 Intel/AMD anakartlarda GNU önyükleyici "[GRUB](docs/linux/)" ve SeaBIOS yüklerini, ARM64(Aarch64) anakartlarda ise *coreboot için* [U-Boot UEFI yükünü](docs/uboot/) sağlar. Bazı kartlarda [x86/x86_64 U-Boot UEFI yükü](docs/uboot/uboot-x86.md) de mevcuttur. x86, x86_64 ve arm64 U-Boot yükleri hafif bir UEFI önyükleme uygulaması sağlar. Libreboot'un [tasarımı](docs/maintain/) tüm bu yükleri tek bir imajda birleştirir ve önyükleme sırasında birini seçersiniz.

**YENİ SÜRÜM: En son sürüm 30 June 2025'te yayınlanan Libreboot 25.06'dır.
Bakınız: [Libreboot 25.06 sürüm duyurusu](news/libreboot2506.md)

Ayrıca Minifree Ltd'den seçili donanımlarda [Libreboot önyüklenmiş cihazlar satın alabilir](https://minifree.org/) veya uyumlu donanımınızı [Libreboot önyükleme hizmeti](https://minifree.org/product/installation-service/) için gönderebilirsiniz. Libreboot'un kurucusu ve baş geliştiricisi Leah Rowe aynı zamanda Minifree'yi de işletmektedir; satışlar Libreboot için finansman sağlamaktadır.

*Biz*, yazılımı [inceleme, paylaşma, değiştirme ve kullanma özgürlüğünün](https://writefreesoftware.org/), herhangi bir kısıtlama olmaksızın, herkesin sahip olması gereken temel insan haklarından biri olduğuna inanıyoruz. Bu bağlamda, *yazılım özgürlüğü* önemlidir. Özgürlüğünüz önemlidir. Eğitim önemlidir. [Onarım hakkı](https://en.wikipedia.org/wiki/Right_to_repair) önemlidir; Libreboot, donanımınızı sürekli yazılım güncellemeleriyle kullanmaya devam etmenizi sağlar. Tüm bunlar Libreboot'un *var olma nedenidir*.

A parallel [Libreboot fork, Canoeboot](https://canoeboot.org), also exists.
Both Canoeboot and Libreboot are maintained by the same developer (Leah Rowe).

Libreboot Tasarımına Genel Bakış
--------------------------------

<img tabindex=1 class="l" alt="Libreboot çalıştıran HP EliteBook 2560p ve Folio 9470m" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

Libreboot, [makine başlatma](https://doc.coreboot.org/getting_started/architecture.html) için [coreboot](https://coreboot.org/) sağlar, bu da önyükleme belleğindeki bir [yüke](https://doc.coreboot.org/payloads.html) atlar; coreboot, Linux/BSD gibi işletim sisteminizi önyükleyen birçok yükle çalışır.

Libreboot, [tam otomatik bir derleme sistemi](docs/maintain/), [otomatik derleme süreci](docs/build/) ve [kullanıcı dostu kurulum talimatları](docs/install/) sağlayarak, ayrıca desteklenen donanımlara kurulum için önceden derlenmiş ROM imajları sağlayan düzenli [ikili sürümler](download.md) sunarak, coreboot'u teknik olmayan kullanıcılar için kullanımı kolay hale getirir. Libreboot'un sağladığı gibi bir otomasyon olmadan, coreboot çoğu kullanıcı için erişilemez olurdu; yine de Libreboot'u istediğiniz gibi [yeniden yapılandırabilirsiniz](docs/maintain/).

Neden Libreboot Kullanmalı?
---------------------------

<img tabindex=1 class="r" alt="Libreboot çalıştıran çeşitli Lenovo ThinkPad'ler" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

Halihazırda özgür yazılıma eğilimliyseniz, belki de zaten bir coreboot kullanıcısıysanız, Libreboot, derleme otomasyonu aracılığıyla ya başlamanızı ya da coreboot'u makinenizde sürdürmenizi kolaylaştırır. Düzenli test edilmiş sürümler sağlar, önceden derlenmiş, genellikle kararlılığı sağlamak için coreboot (ve diğer kodlar) üzerinde belirli yamalarla birlikte gelir. Karşılaştırma olarak, coreboot her birkaç ayda bir kod tabanının bir anlık görüntüsüyle sürekli sürüm modelini kullanır; çok geliştirici odaklıdır, oysa Libreboot özellikle son kullanıcılar için tasarlanmıştır. Başka bir deyişle, Libreboot'un amacı *Sadece Çalışmaktır*. Coreboot'un doğrudan yapılandırılması ve kurulumu da mümkündür, ancak Libreboot bunu *çok* daha kolay hale getirir.

Libreboot size çoğu diğer önyükleme yazılımıyla alamayacağınız [Özgür Yazılım](https://writefreesoftware.org/) sağlar, ayrıca daha hızlı önyükleme hızları ve [daha iyi güvenlik](docs/linux/grub_hardening.md) sunar. Birçok kullanım senaryosu için son derece güçlü ve [yapılandırılabilirdir](docs/maintain/). Özel mülk BIOS satıcıları tarafından size dayatılan kısıtlamalardan (güvenlik sorunlarından bahsetmiyorum bile) memnun değilseniz, Libreboot sizin için olası bir seçenektir. Coreboot'tan miras aldığı için, kodda bilinen arka kapılar içermez ve 1980'lerden kalma eski öğeler içermez. Libreboot, düzenli olarak denetlenen ve geliştirilen coreboot'a dayalı Linux/BSD sistemleri için şık, hızlı bir önyükleme deneyimi sağlar.

Libreboot özel mülk yazılımlardan daha güvenilirdir. Birçok kişi [özgür bir işletim sistemi](https://www.openbsd.org/) kullansa bile özel mülk (özgür olmayan) önyükleme yazılımı kullanır. Özel mülk yazılımlar genellikle [arka kapılar](faq.html#intel) [içerir](faq.html#amd) ve hatalı olabilir. Libreboot projesi, Aralık 2013'te, coreboot yazılımını teknik olmayan kullanıcılar için erişilebilir kılma açık amacıyla kuruldu.

Libreboot, kullanıcıların özel mülk önyükleme yazılımından kaçmasına yardımcı olmaya odaklanan topluluk odaklı bir projedir; biz kendimiz tüm yazılımların [özgür](https://writefreesoftware.org/) olduğu bir dünyada yaşamak istiyoruz ve bu nedenle Libreboot, o dünyaya yaklaşmaya yardımcı olma çabasıdır. Büyük satıcıların aksine, sizi herhangi bir şekilde kısıtlamaya çalışmıyoruz veya sizi bir güvenlik tehdidi olarak görmüyoruz; yazılımı özgürce kullanma, inceleme, değiştirme ve yeniden dağıtma yeteneğinin herkesin sahip olması gereken bir insan hakkı olduğunu düşünüyoruz. Bilgisayarlara uyarlandığında, bunlar satın aldığınız ürünlerdir ve bu nedenle onları istediğiniz gibi değiştirme özgürlüğüne sahip olmalısınız. Intel'in *Boot Guard*'ından (yalnızca kendileri tarafından imzalanmış yazılımın çalıştırılmasına izin vererek coreboot'u engelleyen) veya diğer satıcıların benzer kısıtlamalar getirmesinden bahsederken "güvenlik"ten bahsettiklerini duyduğunuzda, yalnızca *kendi* güvenliklerinden bahsediyorlar, sizinkinden değil. Libreboot projesinde, bu tersine çevrilmiştir; biz Intel Boot Guard ve benzeri teknolojileri kendi mülkünüz (bilgisayarınız) üzerindeki özgürlüğünüze bir saldırı olarak görüyoruz ve bu nedenle, bu tür kontrolü [geri almanıza](docs/install/deguard.html) [yardımcı olmayı](https://trmm.net/TOCTOU/) misyonumuz haline getiriyoruz.

Libreboot Coreboot'un Bir Çatalı Değildir
----------------------------------------

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.vimuser.org/uboot.png" /><span class="f"><img src="https://av.vimuser.org/uboot.png" /></span>

Aslında, Libreboot her kart için mümkün olduğunca *stok* coreboot'a yakın kalmaya çalışır, ancak Libreboot derleme sistemi tarafından otomatik olarak sağlanan birçok farklı yapılandırma türüyle birlikte.

*Alpine Linux*'un bir *Linux dağıtımı* olması gibi, Libreboot bir *coreboot dağıtımıdır*. ROM imajını sıfırdan oluşturmak istiyorsanız, aksi takdirde ROM imajını hazırlamak için coreboot, GRUB ve ihtiyacınız olan diğer yazılımların uzman düzeyinde yapılandırmasını gerçekleştirmeniz gerekir. *Libreboot* ile, Git'ten veya bir kaynak arşivinden indirebilir ve basit bir betik çalıştırabilirsiniz, ve tüm ROM imajlarını oluşturacaktır. `lbmk` (Libreboot MaKe) adlı otomatik bir derleme sistemi, bu ROM imajlarını herhangi bir kullanıcı girişi veya müdahalesi gerekmeden otomatik olarak oluşturur. Yapılandırma önceden gerçekleştirilmiştir.

Libreboot'un otomatik derleme sistemini kullanmadan normal coreboot'u derleseydiniz, çalışan bir yapılandırma üretmek için çok daha fazla müdahale ve iyi teknik bilgi gerektirirdi.

Libreboot'un düzenli ikili sürümleri bu ROM imajlarını önceden derlenmiş olarak sağlar ve [teknik olmayan kullanıcılar için yazılmış basitleştirilmiş talimatları](docs/install/) takip etme yeteneği dışında özel bir bilgi veya beceri gerektirmeden bunları kurabilirsiniz.

### Nasıl Yardımcı Olabilirsiniz

[Görevler sayfası](tasks/) üzerinde çalışılabilecek (çalışılacak) görevleri listeler. Daha fazla görev eklendikçe/tamamlandıkça güncellenecektir. Yardımcı olmak istiyorsanız, bu görevlerden birini seçip üzerinde çalışabilirsiniz.

<img tabindex=1 class="l" style="max-width:15%;" alt="Libreboot'ta GRUB önyükleyici" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

Yardımcı olabileceğiniz *en büyük* yol, bir yapılandırma göndererek Libreboot'a *yeni* anakartlar eklemektir. Coreboot'un desteklediği her şey, sürümlerde sağlanan ROM imajlarıyla Libreboot'a entegre edilebilir. Bakınız:

* [Kart bakımcısı/test edicisi olmak için başvurun](docs/maintain/testing.md)
* [Yeni anakartlar için port kılavuzu](docs/maintain/porting.md)
* [Libreboot derleme sistemi belgeleri](docs/maintain/)

Bunun ardından, derleme sistemi bakımı (yukarıya bakın) ve ciddiye aldığımız *belgelendirme* var. Belgelendirme, herhangi bir projede kritik öneme sahiptir.

*Kullanıcı desteği* de kritik öneme sahiptir. IRC'de kalın ve eğer birine sorunuyla yardımcı olabilecek yetkinliğe sahipseniz (veya onlarla birlikte öğrenecek kadar kurnazysanız), bu projeye büyük bir hizmettir. Birçok kişi ayrıca `r/libreboot` subreddit'inde kullanıcı desteği istemektedir.

[Hata takipçisinde](https://codeberg.org/libreboot/lbmk/issues) listelenen hataları kontrol edebilirsiniz.

Bir hata fark eder ve düzeltmeniz varsa, [yamaları nasıl göndereceğinize dair talimatlar burada](git.md), ve ayrıca rapor edebilirsiniz. Ayrıca, bu web sitesinin tamamı Markdown ile yazılmıştır ve yamalar gönderebileceğiniz [ayrı bir depoda](https://codeberg.org/libreboot/lbwww) barındırılmaktadır.

Tüm geliştirme tartışmaları ve kullanıcı desteği IRC kanalında yapılmaktadır. Daha fazla bilgi [iletişim sayfasında](contact.md) bulunmaktadır.

### libreboot.org için Çeviriler Gerekli

Libreboot şu anda Ukraynaca ve Fransızca çevrilmiş web sayfalarına sahiptir (ancak her iki dilde de henüz tüm sayfalar için değil).

Çevirilere yardımcı olmak istiyorsanız, sayfaları çevirebilir, mevcut çevirileri güncelleyebilir ve çevrilmiş versiyonlarınızı gönderebilirsiniz. Talimatlar için lütfen okuyun:

[libreboot.org için çeviri nasıl gönderilir](news/translations.md)

Belirli bir dilde biri zaten çalışıyor olsa bile, her zaman birden fazla kişi kullanabiliriz. Ne kadar çok o kadar iyi! 


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
