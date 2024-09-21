---
layout: post
title: "RepRap Prusa I3 Yapımı"
date: 2015-06-28 17:33
categories: ["Teknoloji", "3D-Printer"]
tags: ["3d-printer", "arduino", "arduino-mega", "reprap", "diy", "embedded-system"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/reprap-prusa-i3-yapimi-66eea9f34652f.webp"
  alt: "RepRap Prusa I3 Yapımı"
---

Merhaba, RepRap Prusa I3 Yapımı hakkında ArduinoTurk.org de yazdığım yazıyı web arşivlerden zar zor buldum ve derledim, tekrardan blogumda yayınlıyorum. Yavaş yavaş tüm aşamaları tekrar düzenleyip, güzel bir kaynak olarak sunmayı düşünüyorum. Aşağıda devam edelim:

İlk yazımızı ~~**buraya**~~ tıklayıp okuyabilirsiniz. Gelelim RepRap Prusa I3 Yapımı ile ilgili başlangıç makalemize:

# RepRap Prusa I3 Yapımı
**RepRap Prusa I3 Yapımı** için ihtiyacımız olan malzemelerin listesini vereceğim. Bu malzemelerin çoğunu Türkiye'de bulmanız mümkün. Tabii yurt dışı fiyatlarına göre biraz daha farklı olacaktır fiyatlar. Bunun sebebini kısaca açıklamak isterim;

Ürünler Türkiye'ye girdiği zaman otomatik olarak vergilendirme dilimine tabi olmaktadır. Dolayısıyla %18 KDV ve %20 gelir vergisi kazançları eklenir. Ve bunun üstünde satıcının kar etmesi, sürdürülebilirliğini devam ettirmesi ve ürün temini sırasında yaptığı masrafları da eklemesi gerekir. Ve ek bir durum daha, bu ürünü satan kişinin, getiren kişiden aldıktan sonra üstüne kar payı eklemesi durumudur. Dolayısıyla Türkiye'de aşağıda ki listede göreceğiniz ürünleri farklı fiyatlar ile görmeniz mümkündür. Bu sebeple lütfen Türkiye'de kazıklanıyoruz, bizi soyuyorlar gibi kötü terimler kullanmayalım. Bu konuda bilinçli olmanız dileğiyle.

## Gerekli Parça Listesi
- ABS Basılmış Plastik Parçalar
- Extruder
- Hotend - Isıtıcı Uç
- Saplama ve Çelik Parçalar
- Mekanik Parçalar
- Isıtıcı Yatak Malzemeler - Heated bed
- Elektronik Parçalar
- Vida, Somun, Pul ve Diğer Küçük Parçalar
- Çerçeve - Frame

## Nereden Temin Edilebilir?

### ABS Basılmış Plastik Parçalar
RepRap Prusa I3 Yapımı, 3D Printerinizi birleştirmek için kullanacağınız esas parçalardır. Hem hareketli hemde sabit parçaları içerir. X, Y ve Z akslarını, kayışları, motorları, tabla ve ısıtıcıları birleştirmek için kullanılacaktır. Arduino Washer ise, çıplak olarak elektronik devrenizi, alüminyum kasa üzerinde tutabilmeniz ve yalıtım sağlaması amacıyla kullanılacaktır.
Bu parçaları ve bir sonra ki maddede bulunan parçaları 100TL ile 150 TL arasında bastırabilirsiniz. Tabii doluluk oranları en az %60 olması tavsiyemdir.

Baskı parçaları ile ilgili dosya yine aşağıda ki linkte bulunmaktadır.

- 1 Adet Carriage
- 1 Adet X End idler
- 1 Adet X End Motor
- 1 Adet Y Belt Holder
- 4 Adet Y Corner
- 1 Adet Y Motor
- 1 Adet Y idler
- 1 Adet Z Axis Top Left
- 1 Adet Z Axis Top Right
- 1 Adet Endstop Z Holder
- 1 Adet Z Axis Bottom Left
- 1 Adet Z Axis Bottom Right
- 3 Adet Arduino Washer

Parçaların STL formatında baskı dosyalarını indirmek için ~~https://www.burakdursun.com/wp-content/uploads/2015/06/Prusa-I3-Rework-Parts.rar~~ Prusa I3 Rework ABS Parts tıklayın.
**Dosya Şifresi:** arduinoTURK

### Extruder
Extruder, filament malzemesini eritici uca (hot end) hobbelt bolt ve dişli ile, motor vasıtasyıla süren parçadır. Ne kadar filamentin gönderileceği, ne kadar geri çekileceği bu parça ile sağlanır. Extruder'i yapmak için aşağıda ki parçaları birleştirmeniz gerekmektedir. Üzerine bir adet motor ve hotend (ısıtıcı uç) takılır. Bu parçaları hazır olarak bulabilirsiniz. Genelde bu parçalar 3D Printer vasıtasıyla basılır. Zaten RepRap'ın çıkış noktası budur: "3D Printer, 3D Printer basabilir mi?". Piyasada 100TL ile 150 TL arasında basanlar vardır. Fakat burada önemli nokta, parçaların kalitesi ve baskı doluluk oranıdır. Benim önerim %60 civarı doluluk ile üretilmiş parçaları tercih etmenizdir. Tabii doluluk oranı = fiyat. Bence hemen 3D printeri olan bir arkadaş edinin :) Ben tanıdık bir arkadaşıma malzeme ücreti karşılığında bastırdım. Yaklaşık olarak o da 80 TL civarına geldi. Tabii yukarıda ki diğer basılabilir parçalar dahil bu fiyata.

- 1 Adet Wade Extruder Body
- 1 Adet Extruder idler
- 1 Adet Fan Duct
- 1 Adet Wade Small Gear
- 1 Adet Wade Big Gear
- 1 Adet Hobbed Bolt
- 1 Adet Fax 4x4cm
- 2 Adet yay
- 1.75mm Filament

### Hotend - Isıtıcı Uç
Isıtıcı uç (Hot End) ekstrüderden gelen filaman malzemeyi eriten ve ince bir ip haline getiren parçadır. Bu nedenle 3d yazıcının performansını direkt olarak etkileyen en kritik parçadır diyebiliriz. İyi tasarlanmış ve kaliteli olarak üretilmiş bir ısıtıcı uç çok iyi sonuçlar verirken, kalitesiz olarak üretilmiş (genellikle Çinden gelenler) bir ısıtıcı uç 3d printer işinden tümüyle nefret etmenize sebep olabilir. Bunlara ek olarak kaliteli ısıtıcı uçlar farklı malzemeler ile kullanıldıklarında da sorun yaratabilmektedir. Bu nedenle Makerbot / Ultimaker gibi 3d printer üreticileri filaman üreticileri ile anlaşmalar yaparak kendi makineleri için filaman makine uyumluluğunu garanti altına almaktadırlar. Çeşitli varyasonlarda ve özel üretim şekillerinde bulunabilir. Ben görselde ki gibi bir hotend kullanıyorum. Türkiye'de bu malzemeyi 90 TL civarında bulabilirsiniz. Yine yurt dışında ise 9$ civarında satılmaktadır. Bu ısıtıcıyı alırken dikkat etmeniz gereken bir nokta var. Nozzle ve Filament kalınlığı. Genelde 1.75mm lik filament ile 0.30mm nozzle (uç) kullanılmaktadır. Bu tercihinizi baştan yapmanızda fayda var. Yine ikisi arasında ki farkları bir başlıkta anlatacağım. Benim tercihim 1.75mm lik filament ile 0.30mm nozzle oldu. Ayarları bu yazı dizisinde buna göre anlatacağım. Isıtıcı uç (Hot End) ekstrüderden gelen filaman malzemeyi eriten ve ince bir ip haline getiren parçadır.

### Saplama ve Çelik Parçalar
Bu parçalar X, Y ve Z eksenin hareketlerini sağlayan en önemli parçalardır. Bu sebeple paslanmaz ve kaliteli olmalarına önem gösterin. Ben Civa Çeliği olarak çevirdim çünkü en sağlam çelik türüdür. Fiyat olarak normal çeliklere göre bir tık daha pahalıdır ama bütçenizi sarsacak kadar değil. Bu parçaları aldığınız yerde kestirmeniz gerekebilir, çünkü civa çeliğini bir makine olmadan kesmeniz oldukça zordur. Ben kendi torna tezgahımda kestiğim için fazla zorlanmadım. Ölçüler oldukça önemlidir bu yüzden milimetrik olarak bütün olmalarına dikkat edin. Diğer önemli bir nokta ise parçaların düzgünlüğü. Eğer eğim, çıkıntı ve benzeri pürüzler olursa, baskı kaliteine yansıyacak ve diğer çevre elemanları da bozmanıza neden olabilir. Örneğin, motorları yada motor sürücüleri yakabilirsiniz. Bu yüzden pürüssüz, sinek kaydı olması önemli. Aldığınız yerde ve kestirirken mutlaka kontrol edin. Sonradan canınız sıkılmasın. Bu parçalara ise yaklaşık olarak 70 TL ödemeniz gerekecek. Ben İstanbul, Karaköy'de perşembe pazarı denen yerden aldım.

**Civa Çeliği (Pürüssüz Mil)**
- 2 adet Civa Çeliği Ø8x320 mm
- 2 adet Civa Çeliği Ø8x350 mm
- 2 adet Civa Çeliği Ø8x370 mm

**Dişli Çubuk (Saplama)**
- 2 adet Dişli Çubuk Saplama (metrik) M5x300 mm
- 4 adet Dişli Çubuk Saplama (metrik) M10x210 mm
- 2 adet Dişli Çubuk Saplama (metrik) M10x380 mm

### Merkanik Parçalar
Mekanik parçalar aslında hareketi sağlayan parçalardır. Bu parçaları yurt dışından alabileceğiniz gibi Türkiye'den de temin edebilirsiniz. Ben acemilik ile Türkiye'den almayı tercih ettim. Ve yaklaşık 100 TL civarında ödeme yaptım. GT2 kayışı biraz zor bulunuyor, onu mutlaka yurt dışından getirtmek zorunda kalabilirsiniz. Hem daha ucuz olduğu bir gerçek. Adetlerin yanında yazan kodlar önemli, çünkü sistem metrik hesapları bu ölçüler üzerinden yapmaktadır. Aldığınız rulmanların kaliteli olmasına önem gösterin. Çünkü bu hareketli parçalar hem ses yapabilir hemde titreşim yaparak baskının bozulmasına sebep verebilir. Daha sonradan söküp değiştirmekte oldukça zahmetli olacağı için önceden önleminizi almanızı tavsiye ederim. Yurt dışından bu parçalar 10'lu paketler halinde satılmaktadır. NEMA 17 motor ise en önemli motorumuz. Adım sayısı, dönüş dereceleri sistem tarafından bilinerek hesaplanıyor. Farklı motor kullanmanız sıkıntı yaratabilir. Bu arada NEMA 17, motorun adı değil, standart ölçü birimidir. Farklı marka ve modellerde NEMA 17 motor bulabilirsiniz. Türkiye de 5 adet için ödemeniz gereken tutar 200 TL ile 350 TL arasında değişiyor. Yurt dışından getirtebilirsiniz fakat adetli olacağı için gümrükde sıkıntı yaşanabilir. Forum'da ki satış bölümlerini incelemenizi tavsiye ederim. Başka modele geçenler muhtemelen ekonomi yapmak adına satıyor olacaklardır. Bu motorların bağlantı şemaları ile ilgili bilgiyi ilerleyen bölümlerde anlatacağım.

- 11 Adet LM8UU Yatay Rulman
- 1 Adet 624 Bilyalı Rulman
- 1 Adet 608 Bilyalı Rulman
- 1 Adet GT2 Kayış (76 cm uzunluk)
- 1 Adet GT2 Kayış (90 cm uzunluk)
- 2 Adet Kupling 5x5 Ölçülerinde
- 5 Adet Nema 17 Ölçülerinde Motor
- 2 Adet GT2 Kasnak (Kayış Tutucu Dişli)

### Isıtıcı Yatak Malzemeler - Heated bed
Isıtıcı yatak ve diğer malzemeler ise yine oldukça önemlidir. Isıtıcı yatak, eritilen malzemenin bu yatak üzerine dökülürken, baskı esnasında malzemenin yatağa yapışık kalmasını sağlar. Yaklaşık 75 ile 90 derece arasında çalışmaktadır. Bu yüzden sıcakken dokunmak oldukça tehlikeli olabilir. Burada önemli nokta, yatağın da plastik malzemeden olması. Bu sebeple de eritilen malzemenin daha sonra kolay olarak sökülebilmesi için üstüne cam plaka konur. Yatak camı ısıtır, ısınan camın üzerine modelleme yapılır. Bu yatak ile ilgli de önemli ayarlar bulunmaktadır, ilerleyen süreçte bunları da ayrıca anlatacağım. Thermistor malzemesi ise, 3D Printer'ın beynine ve yazılıma ilgili sıcaklık durumunu ileten sıcaklık sensörüdür. Farklı değerlerde bulunabilir. Biz 10K değerinde bir thermistor kullanacağız. Ayarlarımız da buna göre olacak. Bu yüzden doğru değerde thermistor kullanmaya önem gösterin. Polimid bant ise bu ısınan parçaların bir araya gelmesi için kullanacağız. Aslında koca bir bant almanıza gerek yok. 10cm lik bir bant işinizi görecektir. Türkiye de bu bant 30TL civarında satılıyor o yüzden temin edebiliyorsanız bir yerden 10cm kadar almanız işinizi görür. Eğer bulamazsanız da ısıya dayanıklı ve iletken olmayan bir bant kullanabilirsiniz. Geniş tutucu ataç ise ısıtıcı yatak üstünde bulunan camı sabitlemek için kullanacağız, en küçük boy yada bir büyük boyu tercih edebilirsiniz. 2 adet yeterli oluyor ama sağlama almak için 4 adet kullanabilirsiniz. Isıtıcı yatak Türkiye'de 50TL ile 90 TL arasında satılıyor. Yine yurt dışından 9$ ile 15$ arasında satın alabilirsiniz. Cam'ı ise bütün camcılardan temin edebilirsiniz.

- 1 Adet PCB Isıtı Yatay
- 1 Adet Cam Tabaka
- 1 Adet Polimid Bant (Opsiyonel)
- 4 Adet Geniş Tutucu Ataç - Binder Clip
- 1 Adet Thermistor

### Elektronik Parçalar
Elektronik parçalar en önemli parçalarımız. RAMPS nedir? diye soruyorsanız, ilgili konuya tıklayarak ulaşabilirsiniz. Arduino MEGA ve RAMPS ikilisi, bilgisayarınız ile 3D Printer'iniz arasında ki beyin olacak. Bu kartı Türkiye'de 70TL ile 100 TL arasında alabilirsiniz. Tabii konunun başında dediğim gibi biraz tuzludur. Çünkü yurt dışından kit olarak 25$ - 35$ arasında ihtiyacınız olan tüm elektronik parçaları (güç kaynağı ve endstop switchler hariç) tek seferde alabilirsiniz. Ben yurt dışından kit olarak aldım. RAMPS 1.4 versiyonu, SainSmart Arduino MEGA, Step Motor Sürücüleri hepsi birlikte geldi. Bir üst versiyon da ise printer üzerine takabileceğiniz bir ekran bulunuyor. Bu ekran opsiyoneldir. Üzerinde ısıtıcı yatak, ısıtıcı uç ısı değerleri, baskı esnadında kalan süre gibi bilgileri göstermektedir. Bir diğer ek modül ise SD kart desteğidir. Baskı için kullanılacak GCODE dediğimiz dönüştürülmiş kordinat dosyalarını bu SD kart vasıtasıyla printere yükleyerek baskı alabiliyorsunuz. İlk başlangıç için bu iki materyali kullanmamayı tercih ettim. Power Supply - Güç kaynağı ise oldukça önemli. Zira printeriniz baskı yaparken belirli bir oranda elektrik gücüne ihtiyaç duyuyor. Bu güç, baskı filamentini eritmek için kullandığı ısıtıcı fişekte kullanılmaktadır. Bu yüzden en az 10 amperlik güç üretebilen bir güç kaynağı kullanmanız gerekmektedir. Bu tür güç kaynakları metal çerçeveli olarak elektronikçilerde satılmasına rağmen, bilgisayar güç kaynağı kullanımı da oldukça yayıngındır. Ve genelde hepsi 350 watt civarında, 40-50 TL ye satıldığı için daha ekonomik tercih sebebidir. Fakat bilgisayar güç kaynağını bağlarken dikkat etmeniz gereken hususlar bulunmaktdır. Bununla ilgili detayları, ilerleyen yazı dizisinde bulabilirsiniz. Bu noktada hangi güç kaynağını tercih edeceğiniz size kalmış. Ben size ikisinide nasıl kullanacağınızı anlatacağım.

- 1 Adet RAMPS 1.4 (Sürücü Kartı)
- 1 Adet Arduino MEGA 2560
- 4 Adet Step Motor Sürücü
- 3 Adet Endstop - Swithc
- 1 Adet Güç Kaynağı

### Vida, Somun, Pul ve Diğer Küçük Parçalar
Listede ki bütün parçaları neredeyse bir arada tutacak materyallerimiz vida, somun ve pullar. En az diğer parçalar kadar hayati önem taşımaktadırlar. Ve ölçüler oldukça önemli. Tavsiyem bu parçaların hepsinden en az 2-4 adet fazla almanız. Çünkü kaybolabilir yada farklı bir modifikasyon ihtiyacınız için kullanabilirsiniz. Ben İstanbul, Karaköy'de bulunan civatacıları gezip, gözüme kestirdiğim bir dükkandan aldım vidaları. Tek tek sattıkları ve adet ile verdikleri için malzemenin toplanması biraz zaman alabiliyor. Koca bir poşet vida, somun ve pul parçalarını yaklaşık 45 dakika da bir araya getirip, 60 TL ücret ödeyerek aldım. Sizinde bu parçaları tahminen bu civarda bir ücrete alacağınızı düşünüyorum. Bu parçaları yurt dışından getirtmek pek akıllıca olmayacaktır. Zira yarım kilodan fazla ve tamamen metal :) Gümrüğe takılma ihtimali var. Ayrıca Türkiye'den almak daha hızlı olacaktır. Muhtemelen forumda da bu parçaları satanlar olacaktır. Takip etmenizi öneririm.

- 41 Adet Metrik 3x14mm Vida
- 3 Adet Metrik 3x24mm Vida
- 4 Adet Metrik 3x30mm Vida
- 2 Adet Metrik 3x60mm Vida
- 6 Adet Metrik 4x20mm Vida
- 1 Adet Metrik 8x30mm Vida
- 1 Adet Metrik 8x20mm İnbus (grub screw)
- 53 Adet 3mm Çap Pul
- 6 Adet 8mm Çap Pul
- 34 Adet 10mm Çap Pul
- 5 Adet Metrik 3x8mm İnbus (grub screw)
- 33 Adet Metrik 3 Somun
- 6 Adet Metrik 4 Somun
- 2 Adet Metrik 5 Somun
- 1 Adet Metrik 8 Somun
- 34 Adet Metrik 10 Somun
- 1 Adet Silikonlu Metrik 8 Stop Somun

### Çerçeve - Frame
3D Printer üzerinde birleştireceğimiz materyalleri bir arada tutacak kasadır. Genelde Alüminyum yada Ahşap materyalleri tercih edilmektedir. Ahşap biraz daha ucuz olduğu için ve işlemesi kolay olduğu için sıklıkla tercih edilir. Ben ise Alüminyum tercih ettim. Böylece daha sağlam oldu. Ayrıca çizim dosyasında bazı güncellemeler yaptık. Bu güncellemeler ile metrik delikler lazer kesim için standartlaştırıldı. Çizim dosyasını iyi bir lazer kesimcide bastırırsanız sorun yaşamazsınız. Mutlaka en az 6mm kalınlığında malzeme kullanmanızı önereceğim. Birleştirme yaptıktan sonra kesinlikle sallanmayan, eğrilip bükülmeyen bir kasanız olacaktır. Birleştirmeye başlamadan önce sprey oto boyası ile isterseniz boyayabilirsiniz. Böylece daha güzel gözükecektir. Ben henüz boyamadım fakat ikinci printeri yaparken mutlaka boyayacağım.

Parçaları kestirdikten sonra muhtemelen kesim yerlerinde ki çapakları temizlemeyecekler, bunları temizlemek için eğe gibi malzemeye ihtiyaç duyabilirsiniz. Ve mutlaka çok dikkatli olmalısınız, çünkü çapaklar oldukça keskin olduğu kadar, açtığı yara ve kesiklerde bol bol acıyacaktır.

Baskıya hazır çizim dosyasını indirmek için aşağıda ki linki kullanabilirsiniz. Uygulamayı bu çizim üzerinden anlatacağım. Alüminyum yada Ahşap kullanarak lazer CNC de kestirebilirsiniz. Ortalama maliyeti 60 TL ile 90 TL arasında yerine göre değişmektedir. Kullanılacak materyal 6mm kalınlığında olmalıdır.

~~https://www.burakdursun.com/wp-content/uploads/2015/06/prusa_i3_rework_frame_arduinoturk.org_.rar~~ 
Prusa I3 ReWork Metal Çerçeve Çizim Dosyası (DWG Formatı)
**Çizim dosyası RAR Şifresi:** arduinoTURK

### 1.75mm Filament - Baskı Malzemesi
3D yazıcılar olarak ABS ve PLA Filament kartuşlarını kullanmaktadır. Ben printeri yaparken PLA kullanmayı tercih ettim. PLA ve ABS filament arasında ki fark yine forumda bu başlık altında anlatacağım. Filament malzemesi, printer hotend (ısıtıcı uç) tarafından eritilerek, ısıtıcı yatak (heated bed) üzerine dökülerek ortaya modelinizi çıkartmaya yarayan malzemedir. Çeşitli renklerde, boyutlarda ve kalitelerde satılmaktadır. Ben 1.75mm lik PLA filamenti baskı malzemesi olarak kullanmaya karar verdim. Bu yazı dizisinde yine bu malzemeye göre ayarlar yapılacaktır.

Bu malzemeleri 3. yazıya kadar temin edebilirseniz, 3. yazımızda montaja başlıyor olacağız. Yazı dizimiz adım adım fotoğraflı anlatım şeklinde olacaktır.

**NOT:** Yazı üzerinde zaman zaman güncellemeler yapabilirim. Bu başlığı takibe alırsanız, güncellemelerden haberdar olabilirsiniz.