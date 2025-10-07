---
title: Git'i Nasıl Kullanırsınız ve/veya Libreboot'a Yama Gönderirsiniz
x-toc-enable: true
...

SPDX HEADERS
------------

When submitting new documentation pages (e.g. Markdown files), make
sure they are explicitly licensed. You can do this by adding an SPDX
header at the bottom of the file.

Please look at other pages in the repository, to know how this is done.

`GFDL-1.3-or-later` is assumed, as that is what most of the site uses.
You can add this as a comment, in Markdown files. Existing pages show
how to do this (look at the bottom of an existing Markdown file).

A generic notice is also placed stating GFDL, in footer `.include`
files.

If in doubt, simply place this at the bottom of your new Markdown
file when submitting it for review:

```
[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
```

If you're working on an existing Markdown file present in the
website repository, this will probably already be there.

Libreboot Depoları
-----------------

`libreboot` projesinin 3 ana Git deposu vardır:

* Derleme sistemi: <https://codeberg.org/libreboot/lbmk>
* Web sitesi (+belgeler): <https://codeberg.org/libreboot/lbwww>
* Görseller (web sitesi için): <https://codeberg.org/libreboot/lbwww-img>
* Pico-serprog: <https://codeberg.org/libreboot/pico-serprog>
* Libreboot Statik Site Oluşturucu: <https://codeberg.org/libreboot/lbssg>

Ayrıca bakınız: [Libreboot Statik Site Oluşturucu](docs/sitegen/).

Ayrıca lbmk'yi Libreboot'un kendi cgit örneğinde *gezinebilirsiniz*, ancak bu geliştirme için tasarlanmamıştır (bunun için codeberg'i kullanın):\
<https://browse.libreboot.org/lbmk.git/>

[Görevler](tasks/) sayfası, uygulamak istediğimiz (hepsi değil) bazı şeyleri listeler, henüz uygulanmamışsa. Yamalar hoş karşılanır!

Ana Git depoları çalışmıyorsa, `lbmk` ve `lbwww`'nin yansıları bu sayfada aşağıda listelenmiştir.

Libreboot daha önce NotABug'ı kullanıyordu, ancak muhtemelen çok fazla kişi kullandığı için genellikle akşamları HTTP 500 hataları döndürmesi nedeniyle sürekli güvenilirlik sorunları yaşandı; Libreboot'un daha kararlı bir şeye ihtiyacı olduğuna karar verildi, bu yüzden şimdi Libreboot codeberg'de barındırılıyor. Bakınız: [8 Nisan 2023'te codeberg'e taşınma duyurusu](news/codeberg.md)

Ayrıca Libreboot projesi tarafından barındırılan ve libreboot'un ya önerdiği ya da kullandığı şu programlar da var:

Eski `ich9utils` ve `bucts` depoları notabug'da mevcuttur:

* Bucts (yardımcı program): <https://notabug.org/libreboot/bucts>
* ich9utils (yardımcı program): <https://notabug.org/libreboot/ich9utils>

Bu depolardan herhangi birini indirebilir, istediğiniz değişiklikleri yapabilir ve ardından aşağıdaki talimatları kullanarak değişikliklerinizi gönderebilirsiniz.

Libreboot'u (tüm parçalarını) bir Linux dağıtımında derlemeniz önerilir. Örneğin, derleme sistemi (lbmk) BSD sistemlerinde test edilmemiştir. Linux sisteminize `git`'i kurun ve depolardan birini indirin.

Libreboot'un geliştirilmesi Git sürüm kontrol sistemi kullanılarak yapılır. Git'i nasıl kullanacağınızı bilmiyorsanız [resmi Git belgelerine](https://git-scm.com/doc) bakın.

`bucts` deposu, son kontrol ettiğimizde `stuge.se`'deki orijinal depo artık mevcut olmadığı için libreboot projesi tarafından barındırılmaktadır. `bucts` programı Peter Stuge tarafından yazılmıştır. Şu anda orijinal Lenovo BIOS'unu çalıştıran bir ThinkPad X60 veya T60'a dahili olarak bir libreboot ROM'u yüklüyorsanız `bucts`'ye ihtiyacınız var. Bunun için talimatlar burada mevcuttur:\
[libreboot kurulum kılavuzları](docs/install/)

### lbmk (libreboot-make)

Bu, libreboot'taki çekirdek derleme sistemidir. `lbmk`'nin libreboot *olduğunu* söyleyebilirsiniz! Git deposunu indirin:

    git clone https://codeberg.org/libreboot/lbmk

Yukarıda görülen `git` komutu, libreboot derleme sistemi `lbmk`'yi indirecektir. Ardından şu şekilde içine girebilirsiniz:

    cd lbmk

İstediğiniz değişiklikleri yapın veya sadece derleyin. `lbmk`'yi nasıl derleyeceğinize dair talimatlar için [derleme talimatlarına](docs/build/) bakın.

Derleme sisteminin kendisi ve nasıl çalıştığı hakkında bilgi [lbmk bakım kılavuzunda](docs/maintain/) mevcuttur.

### lbwww ve lbwww-img

Libreboot web sitesi ve belgelerin *tamamı* bir Git deposunda barındırılmaktadır. Şu şekilde indirin:

    git clone https://codeberg.org/libreboot/lbwww

Görseller <https://av.libreboot.org/> adresinde barındırılır ve ayrı bir depoda mevcuttur:

    git clone https://codeberg.org/libreboot/lbwww-img

İstediğiniz değişiklikleri yapın. Yama gönderme hakkında aşağıdaki notlara bakın.

Web sitesinin tamamı Markdown'da, özellikle Pandoc versiyonunda yazılmıştır. Statik HTML sayfaları [lbssg](docs/sitegen/) ile oluşturulur. Libreboot'un kurucusu Leah Rowe, aynı zamanda Libreboot statik site oluşturucu projesinin de kurucusudur.

İsterseniz yerel bir HTTP sunucusu kurabilir ve web sitesinin kendi yerel sürümünüzü oluşturabilirsiniz. Lütfen görsellerin hala <https://av.libreboot.org/> adresinde barındırılanlarla bağlantılı olacağını unutmayın, bu nedenle `lbwww-img`'ye eklediğiniz görseller (eklediğiniz görseller için) görsel bağlantılarını `av.libreboot.org`'a bağlarsanız yerel `lbwww` sitenizde görünmeyecektir. Ancak, bu tür görsellerin av.libreboot.org'da barındırılması gereklidir.

Bu nedenle, web sitesine görsel eklemek istiyorsanız, lütfen her biri için bağlantıları <https://av.libreboot.org/path/to/your/new/image/in/lbwww-img> olacak şekilde `lbwww-img` deposuna da gönderin. Libreboot web sitesinde birleştirildiğinde, görselleriniz canlı olarak görünecektir.

Bir fotoğraf ekliyorsanız, web dağıtımı için sıkıştırın. Görseller genellikle 800px genişliğinde ve genellikle 100KiB'den küçük boyutta olmalıdır:

İlk olarak, favori görsel işleme programınızı kullanarak görselinizi yaklaşık 800px genişliğe ölçeklendirin. Örneğin, `imagemagick` ile şunu yapabilirsiniz (görüntünün zaten tercih edilenden daha küçük veya eşit olmadığından emin olun).

    convert original.jpg -resize 600000@ -quality 70% web.jpg

Göndermeden önce jpg görseller üzerinde her zaman `jpegoptim` çalıştırmalısınız. Gereksiz meta verileri kaldırır ve içlerinde kullanılan huffman tablolarını akıllıca yeniden düzenleyerek *kayıpsız* olarak daha da optimize eder.

    jpegoptim -s --all-progressive web.jpg

Görsel bir (çizgi) çizimse, bitmap'lere göre vektör grafikleri tercih edilir. Bu nedenle, mümkünse bunları SVG olarak kaydedin. Bunlar değiştirilmesi kolaydır ve çevirmenlerin işini de kesinlikle kolaylaştıracaktır.

PNG görseller `zopfli` ile optimize edilmelidir (bu da kayıpsızdır). Örneğin, bu Libreboot önyükleme logosunu yaklaşık 11k'dan 3k'ya düşürdü:

    zopflipng -ym image.png image.png

Geliştirme amaçları için, önce görsellerinizi yerel bağlantılar yapabilir ve ardından belge/web sitesi yamalarınızı gönderirken URL'leri ayarlayabilirsiniz.

Web sitesinin yerel sürümünüzü nasıl kuracağınıza dair talimatlar lbssg kılavuzunda mevcuttur. Untitled'ı indirin ve untitled dizininizin içinde `www/` adlı bir dizin oluşturun, ardından www dizininin içine girin ve `lbwww` deposunu oraya klonlayın. Yerel HTTP sunucunuzu buna göre yapılandırın.

Yine, bu amaç için talimatlar lbssg kılavuzunda mevcuttur.

### İsim Gerekli Değil

Yaptığınız katkılar, herkesin erişebileceği bir Git deposunda kamuya açık olarak kaydedilir. Bu, katkıda bulunanın adını ve e-posta adresini içerir.

Git'te, yazar adı ve e-posta adresi için tanımlayıcı veriler kullanmanız gerekmez. `libreboot Contributor` kullanabilir ve e-posta adresiniz contributor@libreboot.org olarak belirtilebilir. Gizliliğinizi korumak istiyorsanız bunu yapmanıza izin verilir. Gizliliğe inanıyoruz. Anonim kalmayı seçerseniz, buna saygı duyacağız.

Tabii ki, istediğiniz adı ve/veya e-posta adresini kullanabilirsiniz.

Yasal olarak konuşursak, uluslararası telif hakkı yasasının Bern Sözleşmesi kapsamında tüm telif hakları otomatiktir. Hangi adı kullandığınız, hatta telif hakkı beyan edip etmediğiniz önemli değildir (ancak belirli telif hakkı lisanslarının kullanılmasını gerektiriyoruz - bu sayfada bunun hakkında daha fazla bilgi okuyun).

Commit'lerinizde/yamalarınızda farklı bir ad ve e-posta adresi kullanırsanız, oldukça anonim olmalısınız. Değişiklikleri genel bir Git deposuna göndermeden önce onaylamak için [git log](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History) ve [git show](https://git-scm.com/docs/git-show) kullanın.

### Lisanslar (katkıda bulunanlar için)

Çalışmanızı özgür bir lisans altında özgürce lisansladığınızdan emin olun. Libreboot artık hangi lisansların kabul edildiğine dair keyfi kısıtlamalar koymamaktadır ve halihazırda birçok lisans mevcuttur. Katkınızı denetleyeceğiz ve sorun varsa size bildireceğiz (örn. lisans yok).

Çalışmanızda *her zaman* bir lisans beyan edin! Bir lisans beyan etmemek, varsayılan, kısıtlayıcı telif hakkı yasalarının geçerli olduğu anlamına gelir, bu da çalışmanızı özel mülk yapar ve aynı kısıtlamalara tabi olur.

MIT lisansı başlamak için iyi bir lisanstır ve Libreboot'taki tüm yeni çalışmalar için tercih edilen lisanstır, ancak seçici değiliz. Libreboot tarihsel olarak çoğunlukla GPL lisanslaması kullanmıştır; bunların çoğu kalmıştır ve muhtemelen kalacaktır. Bu sizin çalışmanız; açıkçası, mevcut bir çalışmadan türetiyorsanız, lisans uyumluluğu için katkınızda aynı lisansı kullanmak mantıklı olabilir.

[Burada](https://opensource.org/licenses) yaygın lisans örnekleri bulabilirsiniz.

Mevcut bir çalışmadan *türetiyorsanız*, katkınızın lisansının türetildiği çalışmanın lisanslamasıyla uyumlu olması önemlidir. MIT lisansı iyidir çünkü birçok diğer lisansla geniş çapta uyumludur ve diğer lisansların vermediği birçok özgürlüğe (alt lisanslama özgürlüğü gibi) izin verir:

<https://opensource.org/licenses/MIT>

### Yama Gönderme

<https://codeberg.org/> adresinde bir hesap oluşturun ve üzerinde çalışmak istediğiniz depoya (giriş yapmış olarak) gidin. *Fork*'a tıklayın ve hesabınızda kendi libreboot deponuz olacak. Deponuzu klonlayın, istediğiniz değişiklikleri yapın ve ardından Codeberg'deki hesabınızdaki deponuza gönderin. İsterseniz bunu yeni bir dalda da yapabilirsiniz.

Codeberg hesabınızda, resmi libreboot deposuna gidebilir ve bir Çekme İsteği gönderebilirsiniz. Çalışma şekli, insanların bu günlerde kullandığı diğer popüler web tabanlı Git platformlarına benzer.

Yamalarınızı oraya gönderebilirsiniz. Alternatif olarak, libreboot IRC kanalına giriş yapabilir ve yamalarla kendi Git deponuz varsa, hangi yamaların incelenmesini istediğinizi kanala bildirebilirsiniz.

Bir Çekme İsteği gönderdikten sonra, libreboot bakımcıları e-posta yoluyla bilgilendirilecektir. Projeden yeterince hızlı yanıt alamazsanız, Libera Chat'teki `#libreboot` kanalı aracılığıyla projeyi bilgilendirebilirsiniz.

Yama göndermenin başka bir yolu da doğrudan Leah Rowe'a e-posta göndermektir:
[info@minifree.org](mailto:info@minifree.org) Leah'nın proje e-posta adresidir.

Ancak, kod inceleme sürecinin şeffaflığı için, şimdilik Codeberg'i kullanmanız önerilir.

### E-posta Listesi

Libreboot'un şu e-posta listesi vardır:
<https://lists.sr.ht/~libreboot/libreboot>

E-posta adresi: [~libreboot/libreboot@lists.sr.ht](mailto:~libreboot/libreboot@lists.sr.ht)

lbmk.git'in Yansıları
--------------------

`lbmk` deposu, Libreboot sürümlerini (derlenmiş ROM imajları dahil) üreten Libreboot'un otomatik derleme sistemini içerir.

Bu bağlantılardan herhangi biri üzerinde `git clone` çalıştırabilirsiniz (bağlantılar ayrıca Web tarayıcınızda değişiklikleri görüntülemek için tıklanabilir):

* <https://git.sr.ht/~libreboot/lbmk>
* <https://git.disroot.org/libreboot/lbmk>
* <https://gitea.treehouse.systems/libreboot/lbmk>
* <https://git.fosscommunity.in/libreboot/lbmk>
* <https://0xacab.org/libreboot/lbmk/>
* <https://framagit.org/libreboot/libreboot>
* <https://gitlab.com/libreboot/lbmk>
* <https://pagure.io/libreboot>
* <https://rocketgit.com/libreboot/libreboot> 

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
