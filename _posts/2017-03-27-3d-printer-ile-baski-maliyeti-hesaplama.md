---
layout: post
title: "3D Printer ile Baskı Maliyeti Hesaplama"
date: 2017-03-27 21:59
categories: ["Teknoloji", "3D-Printer"]
tags: ["3d-printer", "baski-maliyeti", "diy", "embedded-system"]
toc: true
---

Uzun zamandır aklımda olan fakat iş yoğunluğumdan zaman bulamadığım yazımı yazmak bugüne kısmetmiş. Facebook ve diğer 3D gruplarında pıtırcık gibi çoğalan bir furya var, **Hand Spinner** baskıları. Eh tabi bende bu konuda nasibimi alıp, ufak bir seri üretim yaptım fakat bu tamamen seri baskı denemeseydi.

Konuya girmeden ortamı yumuşatıyorum, uzun bir yazı olacak :)) 3D Printer konusunda daha buralar dutlukken çalışmaya başlamıştım ama bir yandan mevcut işim bir yandan hobi, yürütülmesi zor bir seçenekti. Çünkü 3D printer konusu gerçekten zaman isteyen ve hassasiyet isteyen bir iş. Üzerine düşünmeden, kulaktan dolma bilgiler ile deneyim kazanılamadığı gibi, geliştirme konusunda da ilerlemesi zor.

Örnek vermek gerekirse, gerçekten kaliteli bir baskı almak istiyorsanız, hangi printeri yaptığınız yada hangisini hazır satın aldığınızın bir önemi yok, kaliteli baskı için bir çok etmen söz konusu. Bu etmenler ise deneyimleyerek öğreniliyor. 

Yani elinizde bir printer var, uzaktan "ya burak abi bende bu printer var, baskı sorunu yaşıyorum ne yapayım" diye sormak size çözüm getirmyecek çünkü neredeyse her printer kendine özgür bir yapıya sahip. Dolayısıyla benim yada bir başkasının size şunu yap düzelir demesi çözüm yoluna giden yönde sadece bir kaç yol taşı olur.

Konuya girmeden önce yukarıda bahsettiğim etmenleri sıralamak istiyorum. Kaliteli bir baskı için

- Printerinizde kullandığınız malzemelerin gerçekten kaliteli olması gerekiyor (Kayış, Mil, Tabla, Rulman, Motor, Driver, Power Supply, Filament, Nozzle, Extruder...)
- Kalibrasyonlarınızın çok doğru yapılması gerekiyor. Çünkü printeriniz 100 mikron yani 0.10mm lik hassasiyetlere iniyor.
- Baskı ortamızın her zaman aynı olması gerekiyor (Sıcaklık, Nem, EMF Etmenleri vb)
- Printerinizi iyi tanımanız gerekiyor, bu arabanızın ne kadar hızla giderken frene bastığınızda ne zaman duracağını bilmeniz gibi
- Dilimleme programlarını ve özelliklerini iyi bilmeniz gerekiyor.
- Seçeceğiniz firmware ve ayarlarının iyi yapılması gerekiyor.
- En en en önemlisi, basacağınız parçayı nasıl doğru basacağınızı bilmeniz gerekiyor. Bu biraz da fizik bilgisi gerektiriyor.

Neden derseniz, dediğim gibi, her printer aslında eşsiz bir yapıya sahip, özellikle maker olarak kendiniz yaptıysanız. Onu da en iyi siz tanırsınız. Araba kullanıyorsanz, kendi arabanızı değiştirdiğinizde bocalarsınız, istediğiniz tepkiyi vermez, yolda giderken bir tuşa basmanız gerekirse nerde olduğunu bulamazsınız, dikkatiniz dağılır, aynı bunun gibi bir şeydir 3D Print konusu.

Kaliteli bir baskı almanın incelikleri ile ilgili ayrıca bir yazı yazacağım çünkü çok derin bir konu. Hatta bu konuda yazıya katkıda bulunmak isteyen, piyasaca artık bilinen bir kaç isimi de davet etmeyi planlıyorum.

Kahveler hazır, sigaralar yakıldı mı? Hadi bakalım, aydınlanıyoruz:

## 3D Print / 3D Baskı Maliyet Hesaplama
Yazının başında dediğim gibi, gruplarda falan bir sürü hand spinner baskıcısı görüyorum. Hemen hemen bir çoğu, işi olmayan, öğrenci olan, ailesinin yanında yaşayan ve harçlığını çıkartmak isteyen genç girişimci kardeşlerimiz. Dolayısıyla onlar için mantık şu; 1 kg filament 60 lira, 1 hand spinner 10 gram filament harcıyor 0.60 Kuruş filament maliyeti var, rulman 4 tane kullanıyor, tanesi 1 TL den 4 TL, toplamda 4.60 TL maliyeti var, kargoyuda ekleyelim, hop 25 TL den satsam, 15 TL kemiksiz bana kalır, 100 kişiye satarsam 1.500 TL para yapar, allah bereket versin.

Yemin ediyorum size, liseye dönüp ticarete başlayasım geldi :)

Arkadaşlar, ticaretin bir maliyet hesabı vardır, eminim bunu okuyan bir kaç kişi bunu biliyordur ama %90 çoğunuz bu hesabı yapmıyorsunuz. Bir yandan da bu yazıyı yazarak sanıyorum piyasanın topuklarına sıkmış olacağım ama ticaret başka bir şeydir. Kazanmak, ahlaklı şekilde akıllıca kazanmak önemli.

## Masraf Kalemleri
Şimdi bakalım masraf kalemlerimiz neler:

- Makine Masrafları
- Malzeme Giderleri
- Operasyon Masrafları
- İşlem Sonu Masrafları

### Makine Masrafları
Nedir bu makine masrafları. Kullandığınız 3d printer size gökten zembille inmedi, onu almak yada yapmak için para harcadınız. Eğer ki siz yaptıysanız, paradan daha önemli olan şey, zaman harcadınız. Dolayısıyla bunların hepsi bir maliyettir.

Masraf kalemleri içinde en önemli kalem budur. Ayrıca bu kalem, yazıcınızın saatlik çalışma sürecinde birim maliyetini belirleyecek önemli bir noktadır. Makine masrafları kendi için bir kaç kaleme ayrılır ve kullanıldığı dönemde ki giderlerinizi ölçümlemenize yarar. Nedir peki bu kalemler,

- Yazıcı ve Bakım Maliyeti
- Güç Tüketimi

Dedik ya, gökten zembille gelmedi diye :) Baya parayı parça parça yada topluca bayıldınız. Kullandığınız sürede keyfine eskimedi ya, bozuldu, eskidi değiştirdiğiniz parçalar oldu. Eh elektrik de yakıyor, hemde cayır cayır.. Peki kabaca amortisman yani Makine Masraflarımızı hesaplayıp, bu kalemi çıkartalım.

Yazıcıyı toplamak için yada satın alırken ödediğimiz bedel diyelim ki 4.000 TL (Burada düz rakamlar yazıyorum, yazıda bahsettiğim gibi kaliteli bir printer yaparsanız maliyetleriniz çıkacaktır. Ben bu rakamın için olası nozzle, psu, rulman, elektronik parça vb değişimlerini de farazi olarak ekledim.) ödedik. Bu yazıcıyı 3 sene kullanacağımızı düşünelim. Yılda 1000 saat baskı yapacağımızı varsayalım.

**Yazıcı ve Bakım Maliyetleri** = 4000 TL (Yazıcı Maliyetleri) / 3000 (3 Yıl * 1000 Saat) = 1,33 TL

Bize saat başı bu printerin 1,33 TL gelir getirmesi lazım. Ve bizim yılda tam 1000 saat baskı çıkartmamız gerekiyor. Diğer kalemlere gelirsek,

Güç Tüketimi maliyetleri.. Her yazıcı kullanımına göre farklı enerji tüketimi gösterir. Bu kullandığınız hotend yada tabla sıcaklığından tutunda, PSU dan çıkan tüm enerji tüketicilerine (led, diğer extruderlar vb) göre artabilir fakat ortalama tüm yazıcılar 0,1 kW/saat enerji tüketir. Hesaplamak için daha ince formulasyona girmek gerekiyor ama bu yazıdan sonra mantığını anladığınız için oturup kendi "eşsiz" printerinize göre hesaplama yapabilirsiniz.

Türkiye'de Elektrik Üretmeye ve Satmaya tek yetkili TEDAŞ dır. TEDAŞ tarifesi üzerinden şöyle bir tablo görüyoruz:

Yani 1kW Elektrik tükettiğinizde cebinizden çıkan tutar 0,44 kuruş, herşey dahil 4,91 TL. Bu 4,47 TL lik tutarı ayrıca hesaplamakta ve müşteri başına bölmekte fayda var. Siz bunu her güne 1 baskıdan 30 güne bölerek ana enerji tüketim maliyetinize ekleyebilirsiniz. Yani günlük 0,15 TL den 1kW için toplam maliyetinizi 0,59 TL baz alabilirsiniz.

**Güç Tüketimi** = 0,59 TL

Şimdi Makine Masrafları kalemimizin bedeli ortaya çıktı.  **Makine Masrafları =  (Güç Tüketimi) 0,59 TL + (Yazıcı ve Bakım Maliyeti) 1,33 TL **

### Malzeme Giderleri
Malzeme giderleri dediğimizde aklınıza öyle çok şey gelmesin :) Ama tabii iç kalemlere ayırabiliriz bunu. Fakat ana kalemimiz filament tabiki de. Alt kalemleri soracak olursanız, kapton yada mavi banttan tutun da, prit yada saç spreyine kadar sayabiliriz ama bunlarla kafa karıştırmayalım. Malzeme giderinizi ana kalemden hesaplamak için basit bir formul var.

Aldığınız filament varsayalım ki 1 KG yani 1.000 gr. Alış fiyatınız ise 90 TL olsun. Kaliteli bir filament malesef ki biraz masraflı olur. Ben ortalama bir kalem fiyatı baz aldım. Bu filamentin gramaj başına maliyeti hesaplayalım:

Malzeme Giderleri = 90,00 TL (Filament) / 1000 (gram) = 0,090 TL birim maliyetimiz oluyor.

Yani bastığınız malzemenin gramajı * Malzeme Giderleri fiyatımız. Fakat burada unutmamanız gereken bir nokta var, fire (ingilizce fayr değil, bildiğiniz fire). Yani üretim esnasında kontrol dışı kayıplarınız. Bunun içine support/destek den tutunda, boşa akan malzeme yada yaptığınız hatalardan ötürü çöpe giden malzemeleriniz sayılabilir. Elektrik kesintisinde yarım kalan iş de firedir. Bütün bunları müşterilerinize yansıtamazsınız ama ortalama bir rakam ile tüm işlerinize bunu yayabilirsiniz. Böylece adil bir gider karşılaması yapmış olursunuz. Dolayısıyla 1000 gram da %5 fire verdiğinizi varsayabilirsiniz. Yani 50 gram. Bedel ufak çıkacak ama uzun vadede bu bir masraf ve işletme olarak bunu hesaplamanız sizin için önemli. Şimdi tekrar hesaplayalım

Fire Bedelimiz = 50 * 0,09 TL = 4,5 TL toplam 1000 gramda firenizin maliyeti.

**Malzeme Giderleri Maliyetiniz** =  90,00 TL (Filament) + 4,5 TL (Fire) / 1000 (gram) = 0,094 TL

### Operasyon Masrafları
Bu maliyet, baskının alınması ile ilgili işçilik maliyetdir. Ve genelde en çok atlanan ve yanlış hesaplanan kalem budur. Örnek vermek gerekirse, işçinizin saat maliyetini hesaplarken dolaylı ve dolaysız giderlerini hesaplamamanız gibi.

**Sıkıcı bulanları aşağıdan devam edebilir**
Örneği genişletecek olursak, bir işçinin bir şirkete asgari ücretten maliyeti bilinen aksine 1.404,06 TL değildir.  Sigorta, Stopaj, Yol, Yemek kalemleri de birer maliyettir ve ortalama hesaptan asgari ücretli bir çalışanın iş verene maliyeti + 2.363,00 TL dir. Minimum yemek, minimum yol masrafları üzerinden ulaşılan rakamdır. Hal böyle olunca haftada 5 gün, günde 8 saat çalıştığını var sayarsak;

- 1404 TL den işçinin saat ücreti : 8,77 TL
- 2363 TL den işçinin saat ücreti: 14,76 TL yapar.

Yanlış hesabın size yılda ki zararı 5,99 TL (Saatbaşı Kayıp Farkı) * 160 (aylık saat) * 12 (yıllık) = 11.500,08 TL (on bir bin beşyüz türk lirası)

Gördüğünüz gibi gözden kaçan ufak bir hesap hatası, yıl sonunda size pahalıya patlayabilir. Neden o 11 bin TL sizin cebinizde olmasın?

**Sıkıcı bulanlar hadi geçmiş olsun, ara konu bitti**

Burada işçi sizsiniz. Sonuçta bir tane butona basıp, çıktıyı almıyorsunuz. O parçanın üretim sürecinde bir çok işçilik gerekebilir. Yerine tam oturmayan bir parçayı rendelemeniz, ürettiğiniz hand spinner içine rulman gömmeniz, rulmanı temizleme süreciniz (ki tiner yada benzeri ağartıcılar ekstra olarak malzeme giderleri bölümüne eklenmeli), bir den fazla baskıda basılıp birleştirilmesi gereken çoklu parçaların uğraş sonucunda birleştirilmesi gibi bir çok işçilik kalemi mevcut. Hatta ilk kalemde bulunan tamir masraflarına da işçilik ekleyebilirsiniz. 

Minimum işçilik süresini 1 saat tutmanız gerekiyor. Yani 15 dakikalık bir baskınız da olsa, alt limitiniz 1 saat birim ücreti olacak. 1 saati 1 dakika aşan süreler için de +1 saat yuvarlamanız önemli. Yani 1 saat 20 dakika ise, 2 saatlik birim işçilik ücreti hesaplanır. Birim fiyatı hesaplamadan önce kendinize biçeceğiniz bir fiyat olacaktır fakat bunu tarafsız gözle hesaplamanızı tavsiye ederim. Yani "benim aylık maaşım 5.000 TL olmalı" diyorsanız, evet haklı olabilirsiniz **kendinizce** fakat gerçek ederiniz bundan daha az yada fazla olabilir. 

O yüzden benim burada önereceğim rakam Türkiye şartlarında ideal olduğunu düşündüğüm bir rakam. Tabii bu rakamı alırken bir operatör olarak düşünüp, günde 8 saat bir den fazla makineden baskı alan, dilimleme yapan, parça temizleyen bir işçi olarak baz aldım ama işverene masrafları üzerinden asgari ücret ile.. Eğer yukarıda ki sıkıcı bölümü atladıysanız şimdi tam okuma zamanı..

Gelelim hesaplama yöntemine;

**Operasyon Masrafları** = 2363 TL / 160 Saat (Günde 8 Saat, 5 Gün, Ayda Toplam) = 14,76 TL

### İşlem Sonu Masrafları
Yani işletmenizin kar hesapları. Baskı işi için elinizde ki yatırımınızı kullanıyorsunuz, bunun bir eskime payı var, üstüne uzmanlığınızı, baskı yaptığınız yerin kullanımını gibi bir çok şeyi hesapladıktan sonra net bir kar elde etmeniz gerekiyor. Yaklaşık olarak %20 lik bir kar marjı, iyi bir seri üretim yapan için ideal kar oranıdır. Toplam maliyetlerinizi hesapladıktan sonra %20 kar marjınızı eklemek size kar getirisi sağlayacaktır. Çünkü yukarıda ki bir çok kalemde sadece giderlerinizi geri aldınız. 

Hızlı üretimde amortisman tutarları çıktıktan sonra size kar getiri sağlıyor gibi görünebilir fakat işinizi büyütmenin sırrı burada ki amortisman kendi giderini karşıladıktan sonra yerine ikinci bir amortismanlık malzeme koymanıza yaramasıdır. Yani 2 senede size printer masrafını çıkarttıysa, önümüzde ki 2 sene aynı hızda ikinci bir printer almanıza fayda sağlar ve bu böyle böyle katlanarak hem işinizi büyütür, hem kazancınızı büyütür.

Gelelim örnek bir hesaplamaya;

- **Basılacak Malzemenin Birim Ağırlığı:** 12 Gram
- **Toplam Malzeme Gramajı:** 24 Gram
- **Basılacak Malzemenin Adedi:** 2 Adet
- **Baskı Birim Süresi:** 1 Saat 12 Dakika
- **Toplam Baskı Süresi:** 2 Saat 24 Dakika (3 Saat Baz Alınacak)
- **Makine Masrafları:**  5,76 TL (1,92 TL * 3 Saat)
- **MalzemeMasrafları:** 2,25 TL (0,094 TL * 24 Gram)
- **Operasyon Masrafları:** 44,28 TL (14,76 TL * 3 Saat )
- **İşlem Sonu Masrafları (Kar):** 10,10 TL ( Toplam Giderlerin %20 si 50,52 TL)
- **TOPLAM BEDEL: 62,39 TL**

## Sonuç Olarak
Maliyet size yüksek gözükebilir ama bir işletmenin kar etmesi için gereken tüm giderler size bu sonucu veriyor. Ve doğru hesaplama formulü üzerinden giderseniz, kapasiteniz arttıkça yukarıda ki toplam tutarın düştüğünü göreceksiniz. Bu da size sürümden kazanma konusunu gösterir. Üretim kapasitesi arttıkça saat maliyeti düşer, daha ucuza daha çok üretir, daha çok kazanırsınız. Tabii yine burada konuşmadığım bir kalem daha var. Vergi.. Devlete KDV, Muhtasar, Stopaj gibi kalemlerden vergi ödersiniz. Kira verirsiniz. Bunları karşılamak için bu fiyata satmak **ZO-RUN-DA-SI-NIZ** malesef.

Üzgünüm arkadaşlar ama 15 TL, 20 TL ye sattığınız spinner ile para kazanmıyorsunuz, hobinizi tatmin ediyorsunuz. Yakında o iş bitecek. Vergi vermiyorum diye düşünmeyin, maliye bir gün tepenize binerse o gün kazandığınızı düşündüğünüzden çok fazlasını ceza olarak ödersiniz. Bunlar da işin çok çok diğer boyutları. Eğer büyümek istiyorsanız yapmanız gereken maliyet analizidir.

Umarım herkes için faydalı bir yazı olmuştur. Bazı noktalarda atladığım, es geçtiğimiz yerler olabilir, sorarsanız seve seve cevaplarım. Okuduğunuz için teşekkürler.