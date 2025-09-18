---
title: Libreboot Açık Kaynak BIOS/UEFI için Yazılım ve Donanım Özgürlüğü Durumu
x-toc-enable: true
...

Özgürlük olarak özgür
----------------------

Bu sayfa, Kasım 2022'de kabul edilen Libreboot'un [İkili Blob Azaltma Politikası](news/policy.md)'nın nasıl uygulandığını, özellikle de "eğer özgür yazılım kullanılabiliyorsa, kullanılmalıdır" diyen satırı nasıl uygulandığını belgeliyor.

Libreboot, donanım başlatma için [coreboot](https://coreboot.org/) kullanır. Coreboot nominal olarak [*özgür* veya *açık kaynak* yazılım](https://writefreesoftware.org/) olmasına rağmen, *bazı* (hepsi değil) platformlarda coreboot, raminit gibi şeyler için belirli [satıcı dosyaları](https://en.wikipedia.org/wiki/Binary_blob) gerektirir. Libreboot tarafından şu anda desteklenen tüm kartlar, coreboot'un kendisinden tamamen *özgür*, *libre* veya *açık kaynak* kodla başlatılabilir, çünkü Libreboot şu anda yalnızca bu tür anakartlara odaklanmaktadır. Libreboot'un amacı, coreboot'tan tüm anakartları desteklemektir.

Libreboot, Kasım 2022'de daha fazla donanımı (coreboot'tan) desteklemek için daha *pragmatik* bir [ikili blob azaltma politikası](news/policy.md) benimsemiştir, böylece çoğu insanın aksi takdirde kullanacağı tamamen tescilli önyükleme yazılımına kıyasla *yazılım özgürlüğünü* artıran coreboot'u daha fazla insanın kullanabilmesini sağlamak için; Libreboot'un modern politikası bu nedenle pragmatiktir ve *yazılım özgürlüğü* davasını daha da ileriye taşır.

Libreboot'un *önceki* politikası *tüm ikili blobları yasaklamaktı*. Bu, coreboot'u kullanabilecek insan sayısını azaltarak Özgür Yazılım hareketine aktif olarak *zarar verdi*, çünkü bugüne kadar Libreboot gibi bir şey henüz yok. Libreboot'un ana amacı, coreboot'u coreboot fikrini seven ancak sıfırdan yapılandırmak için yeterli olmayan normal, teknik olmayan kullanıcılar için *kullanımı mümkün olduğunca kolay hale getirmektir*. Bu zarar, Kasım 2022'de *düzeltildi*.

### Coreboot mimarisi

Belirli konular hakkında bağlam için lütfen okuyun:

<https://doc.coreboot.org/getting_started/architecture.html>

Coreboot'ta %100 libre başlatma
-------------------------------

Bu ayrımın (özellikle coreboot'un başlatma tarafına atıfta bulunarak) neden önemli olduğu, aşağıdaki bölümlerde daha net hale gelecektir:

Libreboot'ta evrensel bir muafiyet yapılır, CPU mikro kod güncellemelerinin mevcutsa dahil edilmesini (okuyun: proje politikası gereği) zorunlu kılar. Bunun nedenleri hakkında daha fazla bilgi edinmek için şu makaleyi okuyabilirsiniz:

[CPU mikro kod güncellemeleri **TAMAM**](news/policy.md#more-detailed-insight-about-microcode)

[Libreboot 20230423'ten sonraki sürümler, mikro kod hariç tutulan ayrı ROM'lar sağlayacak, varsayılan olanlarla birlikte mikro kod dahil edilecektir.](news/microcode.md)

Intel platformları
------------------

### Tanımlayıcı vs tanımlayıcı olmayan yapılandırma

Libreboot, Intel platformlarını kullanan birkaç anakartı destekler. Bunlardan, bu makale için esasen iki tür makine vardır:

* Tanımlayıcı olmayan yapılandırma
* Tanımlayıcı tabanlı yapılandırma

*Tanımlayıcı* ne anlama gelir? Geleneksel olarak, Intel makinelerindeki ana flash IC (önyükleme yazılımını içeren, genellikle *BIOS* olarak adlandırılır), yalnızca *x86* yürütülebilir kod içerir ve CPU, bellek denetleyicisi ve çevre birimleri gibi tüm donanımları başlatmaktan sorumludur. Bu, *tanımlayıcı olmayan* yapılandırma olarak adlandırılacaktır; bu tür yapılandırmalarda, Intel ME yazılımı varsayılan olarak yoktur.

Bir *tanımlayıcı* yapılandırmasında, flash aşağıdaki gibi bölgelere ayrılmıştır:

* Intel flash tanımlayıcı: her zaman flash'ın ilk 4KiB'si. Makine için ikili kodlanmış yapılandırma verileri ve bölgeler (bu veya aşağıdaki diğerleri gibi) burada tanımlanır. Bazı açılardan, bunu bir sabit diskteki *Ana Önyükleme Kaydı*na benzetebilirsiniz.
* Intel GbE NVM (gigabit ethernet kalıcı bellek): yerleşik Intel gigabit ethernet cihazı için ikili kodlanmış yapılandırma verileri, eğer varsa. MAC adresi, NIC'nin hangi hızda çalışması gerektiği, PCI kimlikleri, *LED yanıp sönme hızı* ve daha fazlası gibi birçok ayarı içerir. Bir Intel olmayan NIC kullanılıyorsa, bu flash bölgesi mevcut olmayacaktır.
* ME (Intel Yönetim Motoru): varsayılan durumunda *kötü* bir güvenlik açığı olan ME, tam ağ bağlantısı ile uzaktan yönetim gibi birçok özellik sağlar. ME, ana CPU'dan ayrı özel bir yardımcı işlemcidir ve başlatma yazılımı ve işletim sistemi bu özel bölgeden ana önyükleme flash'ına yüklenir. Daha fazla bilgi [SSS'de](faq.md#intelme) mevcuttur - ME yazılımının başka bir yerde mevcut olduğu durumlarda, Libreboot ya onu kaldırır ya da (`me_cleaner` programı ile) [yeniden yapılandırır](https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F) ve makine başlatma sırasında devre dışı bırakılır.
* Platform bölgesi: genellikle donanım satıcısı tarafından yerleştirilen bir dizi dize olan program dışı veriler.
* BIOS bölgesi: bu, CPU, bellek denetleyicisi ve çevre birimlerini başlatmaktan sorumlu olan ana önyükleme yazılımını içerir ve makineyi programları çalıştırabilecek bir duruma getirir (muhtemelen bir önyükleyici, ardından bir işletim sistemi). Coreboot kodu (Libreboot tarafından sağlanır) buraya gider.

*Temel olarak*, bu tür "bölgeleri" bir sabit diskteki *bölümlere* benzetebilirsiniz. Önemli olan, flash IC'nin bu tür bölgelere *bölünmüş* olmasıdır, her bölge belirli bir amaç için tasarlanmıştır.

*Tanımlayıcı* ve *GbE* bölgelerinin içeriği Intel veri sayfaları tarafından tanımlanmıştır, ancak bu veri sayfaları genellikle *ayrılmış* bölümler içerir ve bu bölümler belgelenmemiş olarak bırakılır. Yıllar boyunca yapılan tersine mühendislik çalışmaları bu boş noktaların bazılarını belgelemiştir.

### Libreboot Intel ME görüntülerini dağıtmaz

Libreboot, Git deposunda veya sürümlerde Intel ME yazılımını hiçbir şekilde dağıtmaz. Gerektiğinde, Libreboot, `me_cleaner` programını çalıştırarak otomatik olarak indirip kuran komut dosyaları sağlar. Bu tamamen otomatikleştirilmiştir. Lütfen okuyun:

<https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F>

Libreboot yapılandırmalarında, Intel ME'nin *BringUp* kodu kalır. ME BringUp (BUP), ME'nin başlatılmasını sağlayan coreboot'a benzer; aynı benzetmeyle, Libreboot'un bunu yapılandırma şekli, *coreboot*'u bir yükleyici olmadan çalıştırmaya benzer. ME başlatılır, kod çalıştırabilecek bir duruma getirilir, ancak sonra *kod çalıştırmaz*. Bu nedenle *devre dışı bırakılmıştır*.

Başka bir deyişle, *nötrleştirilmiş* bir Intel ME kurulumu, hem yazılım özgürlüğü hem de güvenlik açısından tamamen zararsızdır. Kullanılmayan, kullanılmayan bir işlemci haline gelir ve gerçek dünyadaki çoğu insanın kullanmak istemeyeceği bir şeydir. Bu perspektifle, Intel ME'nin ortalama kullanıcı için artık tamamen önemsiz olduğunu görüyoruz.

*Yayınlanmış* Libreboot ROM görüntüleri, önceden derlenmiş olarak sağlanır, ME yazılımını hiç içermez; bir sürüm hazırlarken otomatik yayın komut dosyaları tarafından temizlenirler. Kaynaktan derliyorsanız, Libreboot yapı sistemi bunu otomatik olarak indirir (satıcıdan), nötrleştirir ve ardından ekler; sürüm ROM'larında, yapı sistemleri tarafından kullanılan aynı komut dosyaları manuel olarak çalıştırılabilir ve aynı sonucu elde eder. Lütfen okuyun: [docs/install/ivy_has_common.md](docs/install/ivy_has_common.md)

ME yazılımı, neredeyse tüm Intel platformlarında *gereklidir*, aksi takdirde makine 30 dakika sonra *kapanır* (veya ME, CPU'nun sıfırlamadan çıkıp çıkmayacağını kontrol ediyorsa, hiç başlatmaz).

### Intel ME kaldırma/devre dışı bırakma hakkında daha fazla bilgi

*Libreboot*, GM45 platformlarında ICH9M güney köprüsü ile ME yazılımını tamamen kaldırmanın bir yolunu sağlar. Bunlar dizüstü bilgisayarlardır: ThinkPad X200/T400/T500/W500 ve bu nesil.

Yukarıda bahsedildiği gibi, daha yeni platformlarda `me_cleaner` kullanılır.

Belirli türde satıcı dosyaları hakkında notlar
---------------------------------------------

### VGA seçenek ROM'ları

*Yerel* video başlatma, coreboot tarafından sağlanan kaynak kodu altında tüm desteklenen Intel platformları için desteklenir ve *etkindir*.

Bazı durumlarda, bir dizüstü bilgisayar coreboot tarafından desteklenmeyen bir grafik yongasına sahip olabilir. Bu durumda, *VGA Seçenek ROM* adı verilen bir satıcı dosyası kullanılmalıdır. Libreboot, Dell Latitude E6400'ün Nvidia GPU modelleri için deneysel bir dalda VGA ROM'unu otomatik olarak indiren deneysel desteğe sahiptir. Bu şu anda sürümlerde veya `lbmk`'nin kararlı dalında mevcut değildir.

Diğer durumlarda, bir makine *iki* grafik cihazına sahip olabilir, bunlardan biri coreboot tarafından sağlanan yerel (özgür/libre) başlatma ile. Bu durumlarda, diğer cihaz için bir VGA ROM eklemek mümkündür; Libreboot şu anda bunun için belgelerden yoksundur, ancak test edilmiştir. Örnek: bazı ivybridge veya haswell thinkpad'lerde çift Intel/Nvidia grafik.

*Eklenti* GPU'lar için, SeaBIOS (yükleyici) genellikle kartta bulunan bir VGA ROM'u tarayabilir ve çalıştırabilir. Bu, Libreboot'un desteklediği belirli masaüstü anakartlarda test edilmiştir ve gayet iyi çalışır; Libreboot'un bu dosyaları hiç ele alması gerekmez.

Libreboot'un varsayılanı, pratikte mümkün olduğunda her zaman özgürlüktür. Bu ek GPU'ları kullanmak isteyen kullanıcılar, Libreboot politikasındaki aşağıdaki paragrafı dikkate almalıdır:

*"Yukarıdaki ilkeler varsayılan yapılandırmalar için geçerli olmalıdır. Ancak, libreboot kullanıcıların istediklerini yapmalarına izin verecek şekilde yapılandırılabilir olmalıdır."* - yapılandırılabilir, kesinlikle öyledir! Bkz: [docs/maintain/](docs/maintain/)

### Bellek denetleyici başlatma

Libreboot, 20230319 veya daha yüksek sürümlerde Haswell'e kadar olan tüm Intel bellek denetleyicileri için *tamamen libre* başlatma sağlar; Broadwell'de Intel MRC kullanılır ve Skylake/yeni platformlarda FSP-M kullanılır.

ARM platformları (chromebook'lar)
---------------------------------

Çoğunlukla özgür yazılım, `daisy` ve `peach` anakartlarında satıcıdan BL1 önyükleyici dosyalarının dahil edilmesi gerekliliği dışında. Bunlar:

* HP Chromebook 11 G1 (daisy-spring) **(sorunlar nedeniyle Libreboot'tan kaldırıldı (daha sonra yeniden eklenecek))**
* Samsung Chromebook XE303 (daisy-snow) **(aynı şekilde)**
* Samsung Chromebook 2 13" (peach-pi) **(aynı şekilde)**
* Samsung Chromebook 2 11" (peach-pit) **(aynı şekilde)**
* nyan-* chromebook'lar da geçici olarak kaldırıldı, ancak Libreboot'ta %100 özgür yazılım

Her kart için özel olarak satıcı dosyalarının listesi
-----------------------------------------------------

Bu makale, Libreboot'ta hangi satıcı dosyalarının yer aldığını ayrıntılı bir genel bakışla açıklamıştır. Yine, coreboot'tan tamamen libre başlatma *tüm şu anda desteklenen kartlarda* mevcuttur.

*Coreboot'tan* bu kritik bir yönüdür, ancak Libreboot'un tam kapsamı, bazı durumlarda coreboot dışında yazılım içeren ana flash IC'dir.

İşte her kart için bu dosyaların listesi:

### Intel/x86

#### Intel ME:

Bu hedeflerde nötrleştirilmiş ME gereklidir:

`dell9020mt_nri_12mb`, `dell9020sff_nri_12mb`, `e5420_6mb`, `e5520_6mb`, `e5530_12mb`, `e6220_10mb`, `e6230_12mb`, `e6320_10mb`, `e6330_12mb`, `e6420_10mb`, `e6430_12mb`, `e6520_10mb`, `e6530_12mb`, `hp2170p_16mb`, `hp2560p_8mb`, `hp2570p_16mb`, `hp8200sff_4mb`, `hp8200sff_8mb`, `hp820g2_12mb`, `hp8300cmt_16mb`, `hp8300usdt_16mb`, `hp8460pintel_8mb`, `hp8470pintel_16mb`, `hp8560w_8mb`, `hp9470m_16mb`, `t1650_12mb`, `t420_8mb`, `t420s_8mb`, `t430_12mb`, `t440plibremrc_12mb`, `t520_8mb`, `t530_12mb`, `w530_12mb`, `w541_12mb`, `x220_8mb`, `x230_12mb`, `x230_16mb`, `x230t_12mb`, `x230t_16mb`, `dell3050micro_vfsp_16mb`, `t480_vfsp_16mb`, `t480s_vfsp_16mb`

Belirtildiği gibi, Libreboot bunu ME'nin artık bir güvenlik tehdidi olmadığı bir durumda sağlar. Kendini başlatır, ancak sonra hiçbir şey yapmaz, bu nedenle devre dışı bırakılmıştır. Bu, `me_cleaner` kullanılarak yapılır. Bkz: <https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F>

MEv11 tabanlı platformlarda, ThinkPad T480 ve Dell OptiPlex 3050 Micro gibi, Intel Boot Guard'ı devre dışı bırakmak için [deguard](docs/install/deguard.md) da kullanıyoruz.

#### KBC1126 EC yazılımı (HP dizüstü bilgisayarlar):

Bu, şu hedefler için geçerlidir: `hp2170p_16mb`, `hp2560p_8mb`, `hp2570p_16mb`, `hp8470pintel_16mb`, `hp9470m_16mb`.

[EC yazılımı](faq.md#ec-embedded-controller-firmware) ana önyükleme flash'ına eklenir, ayrı bir IC'de değil. Bu *daha iyidir* çünkü gelecekte libre yedeklerin kurulumu daha kolay olur ve tersine mühendislik çok daha kolay hale gelir. Libreboot'un yapı sistemi, bu tür yazılımları bir komut dosyasında ele alır ve yapı sürecinde otomatik olarak indirir. Libreboot 20230423'ten itibaren EC yazılımını temizler ve coreboot yapılandırması tarafından her kart için tanımlanan doğru ofsete `cbfstool` ile eklemek için özel bir komut dosyasında işlevsellik sağlar.

#### Intel FSP

Coreboot tarafından bazı kartlarda kullanılan romstage ve raminit sağlar. Libreboot'ta, aşağıdaki yapı hedefleri için kullanıyoruz:

`t480_vfsp_16mb`, `t480s_vfsp_16mb`, `dell3050micro_vfsp_16mb`

#### SMSC SCH5545 Çevresel Kontrol

Bu, Dell Precision T1650'de fan kontrolü için gereken küçük bir yazılımdır.

#### CPU mikro kodu:

[*Mikro kod* güncellemeleri](faq.md#microcode) varsayılan olarak *tüm* x86 platformlarında sağlanır. Teknik olarak gerekli değildir, ancak şiddetle tavsiye edilir. Kaldırmak için şunu yapın:

	cbfstool filename.rom remove -n cpu_microcode_blob.bin

ASUS KFSN4-DRE, KCMA-D8 ve KGPE-D16 kartlarında bunun yerine şunu yapın:

	cbfstool filename.rom remove -n microcode_amd.bin
	cbfstool filename.rom remove -n microcode_amd_fam15h.bin

[Libreboot 20230423'ten sonraki sürümler, mikro kod hariç tutulan ayrı ROM'lar sağlayacak, varsayılan olanlarla birlikte mikrokod dahil edilecektir.](news/microcode.md)

Mikro kod güncellemelerinin kaldırılması sistem kararlılığını olumsuz yönde etkileyecek, standart olmayan bozuk davranışlar getirecek ve makinenin düzgün önyükleme yapamamasına neden olabilecektir. Diğer durumlarda, S3 askıya alma/devam ettirme gibi özellikleri bozabilir.

CPU mikro kod dosyaları varsayılan olarak tüm x86 kartlarında dahil edilir. Çoğu durumda gerekli olmasa da, kullanılmaları şiddetle tavsiye edilir. Nedenleri için bkz:
[news/policy.md#more-detailed-insight-about-microcode](news/policy.md#more-detailed-insight-about-microcode)

#### Intel Flash Tanımlayıcı (IFD):

Intel Flash Tanımlayıcıları bazı kartlarda blob olarak sağlanır, ancak bunlar *yazılım* blobu değildir. Bunlar ikili formatta sağlanan yapılandırmalardır ve özgür yazılım tarafından tamamen okunabilir. Örneğin:

* Coreboot'un `ifdtool` programı, Intel flash tanımlayıcılarını değiştirmek için kapsamlı özelliklere sahiptir.
* Coreboot'un `bincfg` programı, insan tarafından okunabilir bir formatta herhangi bir ikili formatı tanımlayabilen bir `.spec` dosyasından herhangi bir ikili oluşturur. Çeşitli platformlar için birkaç flash tanımlayıcı içerir, ancak Libreboot bunları kullanmaz.

Intel GbE NVM yapılandırması (gigabit NIC için ikili kodlanmış yapılandırma verileri):

* Libreboot'un `nvmutil` programı GbE NVM görüntülerini değiştirebilir

### ARM/chromebook'lar

#### BL1 önyükleyici (peach/daisy):

BL1 önyükleyici şunlar için gereklidir: `daisy_snow`, `daisy_spring` ve `peach_pit`.

Bu kartlar *şu anda* mevcut değil. Yapı sistemi henüz BL1 dosyalarını otomatik olarak eklemediği için Libreboot'tan kaldırıldılar. Kartların, Libreboot'taki Alper'in U-Boot portunu kullanarak çalıştığına inanılıyor.

Sonuç
----------

Yukarıdakilerden görebileceğiniz gibi, Libreboot gerçekten de *ikili blob azaltma politikası*nı uyguluyor ve *azaltma* vurgusu en kritik nokta. Libreboot'un tüm kartlarda makul düzeyde *yazılım özgürlüğü* sağladığı söylenebilir.

Libreboot, çeşitli ek özellikler sağlamak için birçok platform için daha fazla bu tür dosya *ekleyebilirdi*, ancak Libreboot projesinin *amacı* *özgür* yazılımı teşvik etmek ve tescilli yazılım geliştiricilerinin kullanıcılar üzerindeki gücünü *en aza indirmek* olduğu için bunları eklemeyi tercih etmez.

Umarım bu makale düşünmek için malzeme sağlamıştır.

### Bir not: donanım özgürlüğü

Şu anda desteklenen Libreboot makinelerinin hiçbiri özgür *donanıma* sahip değildir, yani IC'ler kamuya açık *verilog* dosyaları ve benzerleriyle birlikte gelmez. Bu makinelerin kendi yedeklerini üretemezsiniz.

Her makinenin devre kartlarını açıklayan bazı şematikler ve boardview dosyaları çeşitli kanallar aracılığıyla çevrimiçi olarak mevcuttur. Bunları kendiniz aramanız gerekir; bir gün, Tamir Hakkı hareketi umarız ki bu tür belgelere kamunun evrensel erişimini sağlayacaktır.

İleri okuma
---------------

Bu makale *ana önyükleme flash*'ına giren kodu açıkladı, ancak satın aldığınız herhangi bir bilgisayarda sistemin başka yerlerinde *tonlarca* yazılım bulunur. Bazı içgörüler Libreboot SSS'de mevcuttur. Bkz:

* [faq.md#what-level-of-software-freedom-does-libreboot-give-me](faq.md#what-level-of-software-freedom-does-libreboot-give-me)
* [faq.md#what-other-firmware-exists-outside-of-libreboot](faq.md#what-other-firmware-exists-outside-of-libreboot)

Bunlardan en kritik olanları HDD/SSD yazılımı ve EC yazılımıdır. Bu iki bağlantıda açıklanan sorunlar, Libreboot'lular da dahil olmak üzere birçok farklı bilgisayar ve dünyadaki neredeyse diğer tüm bilgisayarlar için geçerlidir.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
