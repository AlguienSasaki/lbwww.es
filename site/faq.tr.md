---
title: Libreboot Yazılımı Hakkında Sık Sorulan Sorular
x-toc-enable: true
...

**NOT: Libreboot, 27 Ocak 2024 itibariyle artık flashrom'un bir çatalı olan [flashprog](https://flashprog.org/wiki/Flashprog)'u standart olarak kullanmaktadır.
Bunun nedeni [Libreboot 20240225 sürümünde](news/libreboot20240225.md#flashprog-now-used-instead-of-flashrom) açıklanmıştır**

Diğer adıyla Sıkça Sorgulanan Yanıtlar

Yazılım Yüklemeden Önce Güvenliği Devre Dışı Bırakın
---------------------------------------------------

Dahili yazılım yüklemeden önce, `/dev/mem` korumalarını devre dışı bırakmanız gerekir. İşiniz bittiğinde tekrar etkinleştirdiğinizden emin olun.

Bakınız: [/dev/mem korumasını devre dışı bırakma](docs/install/devmem.md)

Libreboot Önyüklenmiş Olarak Satın Alın
--------------------------------------

Profesyonel kurulum istiyorsanız, Minifree Ltd seçili donanımlarda [Libreboot önyüklenmiş](https://minifree.org/) cihazlar satmaktadır ve ayrıca makinenizi göndererek Libreboot kurulumu yaptırmak istiyorsanız [Libreboot önyükleme hizmeti](https://minifree.org/product/installation-service/) sunmaktadır.

Libreboot'un kurucusu ve baş geliştiricisi Leah Rowe aynı zamanda Minifree Ltd'yi de işletmektedir; satışlar Libreboot projesi için finansman sağlamaktadır.

Önemli Konular
-------------

### Libreboot'u kaynak koddan nasıl derlerim?

[lbmk derleme talimatlarına](docs/build/) bakın.

### Derleme sistemi nasıl çalışır?

[lbmk bakım kılavuzuna](docs/maintain/) bakın.

### CH341A Kullanmayın!

Bu SPI programlayıcı çipinize ve bağlı olduğu anakarta zarar verecektir.

Daha fazla bilgi için [docs/install/spi.md](docs/install/spi.md) sayfasındaki CH341A hakkındaki notları okuyun.

### Nasıl Yardımcı Olabilirim

Projeye önemli ölçüde yardımcı olmak için yetenekli bir geliştirici olmanız gerekmez.
Coreboot tarafından desteklenen bir kartınız varsa, onu Libreboot'a [port etmeyi](/docs/maintain/porting.md) düşünün.

Libreboot'ta desteklenen bir kartınız varsa, lütfen test edici olmayı düşünün.
Test etmek minimal çaba gerektirir ve projeye gerçekten yardımcı olur.
ROM'lar yayınlanmadan önce test etmekle ilgileniyorsanız [kart bakımcıları belgelerine](/docs/maintain/testing.md) bakın.

### GM45 ThinkPad'lerde Düzensiz Arka Işık

Coreboot'ta kullanılacak doğru PWM değerini nasıl tespit edeceğimizi bilmiyoruz, bu yüzden coreboot'taki varsayılan değeri kullanıyoruz ve bu bazı CCFL panellerde bu soruna neden oluyor, ancak LED panellerde olmuyor.

[belgeler: arka ışık kontrolü](../docs/misc/#finetune-backlight-control-on-intel-gpus) sayfasındaki notları takip ederek dağıtımınızda bunu çözebilirsiniz.

### GM45 ThinkPad Ethernet Portu Otomatik Bağlanmıyor

Bu, network-manager kullanan bazı sistemlerde gözlemlendi. Bu durum hem orijinal BIOS'ta hem de libreboot'ta oluyor. Bu donanımdaki bir özelliktir. Debian sistemlerinde, ethernet kablosunu bağladığınızda ağ servisini yeniden başlatmak bir çözüm yoludur:

    sudo service network-manager restart

Systemd tabanlı dağıtımlarda şunu deneyebilirsiniz:

    sudo systemctl restart network-manager

(servis adı, yapılandırmanıza bağlı olarak sizin için farklı olabilir)

### PIKE2008 Modülü KGPE-D16 / KCMA-D8'i Donduruyor

ASUS KCMA-D8 veya KGPE-D16'da PIKE2008 modülünden seçenek ROM'unu yüklemek sistemin önyüklemede donmasına neden olur. Bunu yükte kullanmak mümkündür (linuxboot gibi bir linux çekirdeği yükü kullanıyorsanız) veya normal SATA'dan önyükleme yapmak (SeaGRUB ve/veya SeaBIOS ile) ve sonra Linux'ta kullanmak mümkündür. Linux çekirdeği, seçenek ROM'unu yüklemeden PIKE2008 modülünü kullanabilir.

### ThinkPad Dizüstü Bilgisayarlarda Çekirdek Panik Günlüklerini Nasıl Kaydederim?

Bunu yapmanın en kolay yolu, çekirdeğin netconsole'unu kullanmak ve paniği yeniden oluşturmaktır. Netconsole iki makine gerektirir: panik olan (kaynak) ve çökme günlüklerini alacak olan (hedef). Kaynağın ethernet kablosuyla bağlı olması ve hedefin panik anında erişilebilir olması gerekir. Bu sistemi kurmak için, kaynakta root olarak (`source#`) ve hedefte normal kullanıcı olarak (`target$`) aşağıdaki komutları çalıştırın:

1.  Hedef makinede bir dinleyici sunucu başlatın (netcat iyi çalışır):

    `target$ nc -u -l -p 6666`

2.  Configfs'yi bağlayın (önyükleme başına yalnızca bir kez, zaten bağlı olup olmadığını
    `mount | grep /sys/kernel/config` ile kontrol edebilirsiniz. Bağlı değilse
    çıktı vermeyecektir).

    `source# modprobe configfs`

    `source# mkdir -p /sys/kernel/config`

    `source# mount none -t configfs /sys/kernel/config`

3.  Kaynağın ethernet arayüz adını bulun, `enp*` veya `eth*` şeklinde olmalıdır,
    `ip address` veya `ifconfig` çıktısına bakın.

    `source# iface="enp0s29f8u1"` bunu değiştirin

    Hedef makinenin IPv4 adresini buraya girin:

    `source# tgtip="192.168.1.2"` bunu değiştirin

4.  Kaynak makinede netconsole günlükleme hedefi oluşturun:

    `source# modprobe netconsole`

    `source# cd /sys/kernel/config/netconsole`

    `source# mkdir target1; cd target1`

    `source# srcip=$(ip -4 addr show dev "$iface" | grep -Eo '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+')`

    `source# echo "$srcip" > local_ip`

    `source# echo "$tgtip" > remote_ip`

    `source# echo "$iface" > dev_name`

    `source# arping -I "$iface" "$tgtip" -f | grep -o '..:..:..:..:..:..' > remote_mac`

    `source# echo 1 > enabled`

5.  Konsol günlük seviyesini hata ayıklamaya değiştirin:

    `source# dmesg -n debug`

6.  Günlüklemenin çalışıp çalışmadığını test edin, örneğin kaynakta bir USB
    cihazı takıp çıkararak. Hedef makinede netcat'i (nc) başlattığınız
    terminalde birkaç satır görünmelidir.

7.  Çekirdek paniğini yeniden oluşturmayı deneyin.

Donanım Uyumluluğu
-----------------

### Hangi sistemler libreboot ile uyumludur?

Herhangi bir sistem kolayca eklenebilir, bu nedenle *uyumluluk* yalnızca libreboot'un kullandığı `lbmk` derleme sistemine entegre edilmiş kartlara atıfta bulunur.

[Kurulum sayfası](docs/install/) uyumlu makineleri listeler.

### Modern Intel Donanımlarında Özgürlük Tuzakları {#intel}

Coreboot nominal olarak Özgür Yazılımdır, ancak desteklediği bazı x86 hedeflerinde hem Intel hem de AMD'de belirli satıcı kodları gerektirir.

#### Intel Yönetim Motoru (ME) {#intelme}

NOT: Aşağıdaki bilgiler biraz eskidir. Günümüzde Intel ME bir ARC yardımcı işlemcisinde çalışmamaktadır, bunun yerine ME yazılımı Minix işletim sistemi tabanlı x86 için yazılmış, değiştirilmiş bir Intel 486 tabanlı mimaride çalışır. Ancak, genel tasarım felsefesi ve işleyiş çoğunlukla aynıdır.

Intel ME'ye sahip *çoğu* güncel Intel platformunda, BringUp'tan sonra Intel ME'yi devre dışı bırakmak artık mümkündür. Bakınız:

<https://github.com/corna/me_cleaner>

İçinde Intel ME bulunan tüm GM45+ICH9M dizüstü bilgisayarlarda (ayrıca bu X4X+ICH10 masaüstü bilgisayarlar anlamına gelir), ME yazılımının önyükleme belleğinde bulunması gerekmez. Ya ME'yi devre dışı bırakan ve önyükleme belleğindeki bölgeyi kaldıran değiştirilmiş bir tanımlayıcı kullanılır ya da tanımlayıcısız bir kurulum kullanılır. Ancak, tüm modern Intel platformları ana önyükleme belleğinde bir Intel ME imajının bulunmasını gerektirir.

Şimdi ana konuya geçelim:

Haziran 2006'da Intel'in 965 Express Yonga Seti Ailesi (Grafik ve) Bellek Denetleyici Hub'ları veya (G)MCH'ler ve ICH8 G/Ç Denetleyici Ailesinde tanıtılan Intel Yönetim Motoru (ME), (G)MCH yongasında fiziksel olarak bulunan ayrı bir hesaplama ortamıdır. 2009'un 3. çeyreğinde, ilk nesil Intel Core i3/i5/i7 (Nehalem) CPU'ları ve Platform Denetleyici Hub'larının (PCH) 5 Serisi Yonga Seti ailesi, ICH'nin yerini alan PCH yongasının içinde daha sıkı entegre edilmiş bir ME (şimdi 6.0 sürümünde) getirdi. Böylece ME, ***2006 ortasından bu yana tüm Intel masaüstü, mobil (dizüstü) ve sunucu sistemlerinde mevcuttur***.

ME, bir ARC işlemci çekirdeğinden (ME'nin sonraki nesillerinde diğer işlemci çekirdekleriyle değiştirilmiştir), kod ve veri önbelleklerinden, bir zamanlayıcıdan ve ek cihazların bağlandığı güvenli bir dahili veri yolundan oluşur. Bu cihazlar arasında bir kriptografi motoru, dahili ROM ve RAM, bellek denetleyicileri ve ana işletim sisteminin belleğine erişmek ve ME'nin sınırlı dahili RAM'ini desteklemek için korumalı bir harici bellek bölgesi ayırmak için bir ***doğrudan bellek erişimi (DMA) motoru*** bulunur. ME ayrıca bir Intel Gigabit Ethernet Denetleyicisi üzerinden kendi MAC adresiyle ***ağ erişimine*** sahiptir. Dahili ROM'da saklanan önyükleme programı, PC'nin SPI flash yongasından bir yazılım "manifestosu" yükler. Bu manifesto ***güçlü bir kriptografik anahtarla*** imzalanmıştır ve ME yazılımının sürümleri arasında farklılık gösterir. Manifesto belirli bir Intel anahtarıyla imzalanmamışsa, önyükleme ROM'u yazılımı yükleyip çalıştırmaz ve ME işlemci çekirdeği durdurulur.

ME yazılımı sıkıştırılmıştır ve içeriklerinin güvenli kriptografik hash'leriyle birlikte manifestoda listelenen modüllerden oluşur. Bir modül işletim sistemi çekirdeğidir ve "ThreadX" adlı ***özel bir gerçek zamanlı işletim sistemi (RTOS) çekirdeğine*** dayanır. Geliştirici Express Logic, ThreadX için lisanslar ve kaynak kodu satar. Intel gibi müşterilerin ThreadX kaynak kodunu açıklaması veya alt lisans vermesi yasaktır. Başka bir modül, bir ***Java sanal makinesi*** ve kriptografi, güvenli depolama vb. için önceden yüklenmiş Java sınıfları kümesinden oluşan Dinamik Uygulama Yükleyicisidir (DAL). DAL modülü, PC'nin HDD veya SSD'sinden ek ME modülleri yükleyip çalıştırabilir. ME yazılımı ayrıca flash bellek alanında Intel Aktif Yönetim Teknolojisi (AMT), bir Güvenilir Platform Modülü (TPM) uygulaması, Intel Boot Guard ve ses ve video DRM sistemleri dahil olmak üzere bir dizi yerel uygulama modülü içerir.

Intel "vPro" markasının bir parçası olan Aktif Yönetim Teknolojisi (AMT) uygulaması, uzak kullanıcıların PC'yi açmasını, kapatmasını, bilgi görüntülemesini ve başka şekillerde yönetmesini sağlayan bir Web sunucusu ve uygulama kodudur. PC ***kapalıyken bile uzaktan kullanılabilir*** (Wake-on-Lan aracılığıyla). Trafik SSL/TLS kitaplıkları kullanılarak şifrelenir, ancak tüm büyük SSL/TLS uygulamalarının çok ses getiren güvenlik açıkları olduğunu hatırlayın. AMT uygulamasının kendisinin, rootkit'ler ve keylogger'lar geliştirmek ve bir PC'nin yönetim özelliklerine gizlice şifreli erişim kazanmak için kullanılan ***[bilinen güvenlik açıkları](https://en.wikipedia.org/wiki/Intel_Active_Management_Technology#Known_vulnerabilities_and_exploits)*** vardır. ME'nin PC'nin RAM'ine tam erişimi olduğunu unutmayın. Bu, ME'nin bilgisayarınızdaki tüm verileri görebileceği anlamına gelir.

ME yazılımı, bilgisayarınızın donanımını başlatmak için gereken düşük seviyeli kodun yanı sıra, Intel'in sizin bilgisayarınızı kontrol etmesini sağlayan bir arka kapı görevi görür. Bu, Intel'in bilgisayarınızı uzaktan kontrol etmesine, verilerinize erişmesine ve bilgisayarınızı kapatmasına olanak tanır. ME, bilgisayarınızın RAM'ine tam erişime sahiptir ve ağ üzerinden iletişim kurabilir.

ME'nin varlığı, bilgisayarınızın güvenliğini ve gizliliğini tehlikeye atar. ME'nin varlığı, bilgisayarınızın güvenliğini ve gizliliğini tehlikeye atar. Bu nedenle, ME'yi devre dışı bırakmak veya kaldırmak önemlidir. Libreboot, ME'yi devre dışı bırakır veya kaldırır, böylece bilgisayarınızın kontrolünü size geri verir.

### AMD Platform Security Processor (PSP) {#amd}

AMD Platform Security Processor (PSP), AMD'nin Intel ME'ye benzer bir teknolojisidir. PSP, AMD işlemcilerinde bulunan bir ARM işlemci çekirdeğidir. PSP, ME gibi, bilgisayarınızın RAM'ine tam erişime sahiptir ve ağ üzerinden iletişim kurabilir.

PSP, AMD'nin bilgisayarınızı kontrol etmesini sağlayan bir arka kapı görevi görür. Bu, AMD'nin bilgisayarınızı uzaktan kontrol etmesine, verilerinize erişmesine ve bilgisayarınızı kapatmasına olanak tanır. PSP, bilgisayarınızın RAM'ine tam erişime sahiptir ve ağ üzerinden iletişim kurabilir.

PSP'nin varlığı, bilgisayarınızın güvenliğini ve gizliliğini tehlikeye atar. Bu nedenle, PSP'yi devre dışı bırakmak veya kaldırmak önemlidir. Libreboot, PSP'yi devre dışı bırakır veya kaldırır, böylece bilgisayarınızın kontrolünü size geri verir.

### Libreboot'u Nasıl Kurarım?

Libreboot'u kurmanın birkaç yolu vardır:

1. Minifree Ltd'den [Libreboot önyüklenmiş](https://minifree.org/) bir bilgisayar satın alabilirsiniz.
2. Uyumlu bir bilgisayarınız varsa, [Libreboot önyükleme hizmeti](https://minifree.org/product/installation-service/) için Minifree Ltd'ye gönderebilirsiniz.
3. [Kurulum talimatlarını](docs/install/) takip ederek kendiniz kurabilirsiniz.

### Libreboot'u Kaynak Koddan Nasıl Derlerim?

[lbmk derleme talimatlarına](docs/build/) bakın.

### Derleme Sistemi Nasıl Çalışır?

[lbmk bakım kılavuzuna](docs/maintain/) bakın.

### Libreboot'u Güncellemeli miyim?

Evet, Libreboot'u güncel tutmanız önerilir. Her yeni sürüm, güvenlik düzeltmeleri ve yeni özellikler içerir. Güncelleme talimatları için [kurulum sayfasına](docs/install/) bakın.

### Libreboot'u Kaldırabilir miyim?

Evet, Libreboot'u kaldırabilir ve orijinal BIOS/UEFI yazılımınıza geri dönebilirsiniz. Ancak, bu işlem risklidir ve dikkatli yapılmalıdır. Talimatlar için [kurulum sayfasına](docs/install/) bakın.

### Libreboot Hangi İşletim Sistemlerini Destekler?

Libreboot, [Linux](docs/linux/) ve [BSD](docs/bsd/) işletim sistemlerini iyi destekler. Windows gibi özgür olmayan işletim sistemleri de çalışabilir, ancak bu sistemler Libreboot'un özgürlük felsefesiyle çelişir.

### Libreboot'ta Sorun Yaşıyorum. Ne Yapmalıyım?

1. [SSS sayfasını](faq.html) kontrol edin - sorununuzun yanıtı burada olabilir.
2. [Belgeleri](docs/) okuyun - sorununuzla ilgili bilgi burada olabilir.
3. [IRC kanalına](https://web.libera.chat/#libreboot) katılın - burada yardım isteyebilirsiniz.
4. [E-posta listesine](https://lists.sr.ht/~libreboot/libreboot) yazın - burada da yardım isteyebilirsiniz.
5. [Hata takipçisinde](https://codeberg.org/libreboot/lbmk/issues) sorununuzu arayın veya yeni bir hata raporu oluşturun.

### Libreboot'a Nasıl Katkıda Bulunabilirim?

Libreboot'a birçok şekilde katkıda bulunabilirsiniz:

1. [Yeni anakartlar ekleyerek](docs/maintain/porting.md)
2. [Test ederek](docs/maintain/testing.md)
3. [Belgeleri geliştirerek](docs/maintain/)
4. [Çeviri yaparak](news/translations.md)
5. [Hataları düzelterek](https://codeberg.org/libreboot/lbmk/issues)

### Libreboot'un Geleceği Nedir?

Libreboot, özgür yazılım topluluğunun bir parçası olarak gelişmeye devam edecektir. Hedeflerimiz:

1. Daha fazla donanım desteği eklemek
2. Kullanıcı deneyimini iyileştirmek
3. Güvenliği artırmak
4. Belgeleri geliştirmek
5. Topluluk katılımını artırmak

### Libreboot'u Ticari Olarak Kullanabilir miyim?

Evet, Libreboot GNU Genel Kamu Lisansı altında lisanslanmıştır. Bu, yazılımı özgürce kullanabileceğiniz, değiştirebileceğiniz ve dağıtabileceğiniz anlamına gelir. Ancak, değişikliklerinizi de aynı lisans altında paylaşmanız gerekir. 
