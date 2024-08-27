---
layout: post
title: "ESP8266 ile Proje Geliştirmeye Başlamak"
date: 2017-01-28 15:28
categories: ["Yazılım", "Embedded System"]
tags: ["esp8266", "wifi", "arduino", "nodemcu"]
toc: true
---

Gömülü sistemlere ilk adımı attıysanız, sanıyorum ki bu dünyadan zevk almaya başlamışsınızdır. Bu sonsuz hazzı devam ettirmezseniz, içinizi yemeye başlayabilir. Bir sonraki adımda ne yapacağınızı düşünüyorsanız, büyük ihtimalle ESP8266 serisi çiplerle uzaktan da olsa tanışmışsınızdır. ESP8266 serisi çipler, Wi-Fi ile internete bağlanabilen, hem küçük boyutlu hem de uygun fiyatlı çip kartlarıdır. Günümüzde internete bağlanamayan neredeyse hiçbir cihaz kalmadı: buzdolabı, çamaşır makinesi, evler, televizyonlar... Aklınıza gelebilecek neredeyse her cihaz artık internete bağlanabiliyor. Sanırım bir tek tencere ve ütüler kaldı :)

ESP8266 ile yapabileceğiniz birçok şey mevcut. Ancak, Türkçe kaynak azlığından dolayı muhtemelen "ESP8266 nasıl programlanır", "ESP8266 nasıl kullanılır" gibi kelimelerle sadece İngilizce kılavuzlara ve yazılara ulaşıyorsunuz. Bu nedenle, nasıl başlanır ve nasıl proje üretilir gibi kilit noktaları geçememiş olabilirsiniz.

Geçen yılın başında aklımdaki bir proje için ESP8266 serisi ile bir cihaz tasarlamak gelmişti. Fakat gerek iş yoğunluğu, gerek zamansızlık, gerekse Türkiye'nin içinde bulunduğu özel durumlardan ötürü bir türlü projeye başlayamadım. Bir diğer nedenim de, böyle bir projeyi tek başıma yapmak yerine arkamda bir destekle ilerlemek istememe rağmen güvenimin kalmamış olmasıydı. Ülkemizde TÜBİTAK, KOSGEB gibi kurumlar saçma sapan projelere destek verirken, ülkemize katma değer katacak projelere maalesef her türlü engeli koyuyorlar. Beni az çok tanıdıysanız ne iş yaptığımı, bir yandan iş adamı olduğumu biliyorsunuzdur. Bu sebeple, ülkedeki işleyişe ve neyin nasıl gittiğine dair her zaman izlenimlerim olmuştur.

Hal böyle olunca da pek bir adım atma taraftarı olmadım. Ancak son dönemdeki aşırı durgunluklar, ekonomik hareketsizlik ve benzeri etkenler beni evde "katma değer üretecek" projelere yöneltti ve tekrardan kapattığım defterleri açtım. Şu anda projem için ESP8266 ile çalışmaya başladım ve yetersiz kaynaklardan ötürü blogumda böyle bir yazıya yer verme kararı aldım. Haydi başlayalım!

## ESP8266 Nedir?

![ESP8266 E-01 modülünün örnek dış görünümü](assets/img/ESP8266-Wifi.jpg)
*ESP8266 E-01 modülünün örnek dış görünümü.*

ESP8266 ve bu çipi barındıran geliştirme kartları, gerçekten küçük bir devrim niteliğindeler. ESP8266’nın üretiliş amacı, öncelikle Arduino’yu Wi-Fi üzerinden internete bağlamaktı. XBee, Ethernet shield gibi pahalı parçalar kullanmak yerine, 2-3 dolar maliyetinde olan bu modülü kullanmak istediler. Bana kalırsa, bu haliyle bile çok başarılı bir ürün olurdu. ESP8266 Wi-Fi modülü, piyasadaki en uygun Wi-Fi modüllerindendir ve tüm Arduino projelerinizde kullanabilirsiniz. Arduino ile haberleşmeyi seri port üzerinden yapıyor. Bu nedenle, TX ve RX pinlerini kullanarak, Serial ile haberleşme yapabilirsiniz. Modül ile olan bağlantınızı debug etmek için USB-Serial kartı kullanabilirsiniz. Ancak test modunda Serial Monitor kullanmak isterseniz, ESP8266 seri bağlantısını SoftwareSerial ile de yapabilirsiniz. En önemli avantajları ise birincisi fiyatının diğer modüllere göre ucuz olması, ikincisi ise Arduino IDE üzerinden tıpkı Arduino programlar gibi USB-TTL serial dönüştürücü ile programlanabilmesidir.

Birinci kullanım şekli, UART seri haberleşme protokolü üzerinden RX-TX pinleri ile Arduino veya başka kartları Wi-Fi ile internete bağlamaktır. Bu sayede internet üzerinden kontrollü devreler oluşturmanıza olanak sağlar. İkinci kullanım şekli ise Arduino IDE’ye ESP8266 kütüphanelerini yükleyerek, bu kartı programlamaktır. Bu kullanım şeklinde ise programlandıktan sonra başka hiçbir şeye ihtiyaç duymadan internet üzerinden kontrol edilen projeler yapabilirsiniz.

## ESP8266 Pin Dağılımı
![ESP-01 Pin Dağılımı ve Kart Şeması](assets/img/ESP8266-12E-NodeMCU-Development-Board-pinout.webp)
*ESP-01 Pin Dağılımı ve Kart Şeması.*

ESP8266 modülünün versiyonlarına göre pin dağılımları değişmektedir. Yukarıda gördüğünüz ESP-01 modülü için pin dağılımı URXD, GPIO0, GPIO1, GND, VCC, RST, CH_PD, UTXD olarak görülmektedir. Bu kısaltmaları açıklamıyorum; buraya kadar geldiyseniz zaten neyin ne olduğunu biliyorsunuzdur. Eğer bilmiyorsanız, yakında bununla ilgili de bir yazı yazacağım :) Takipte kalın.

Bu pin dağılımı ile ESP8266 modülünü ister başka bir MCU ile haberleştirebilir, isterseniz de kendi MCU'su üzerinden iletişim kurarak fiziksel dünyaya müdahale edebilirsiniz. Bu modüllerin versiyonlarına göre GPIO port sayısı değişmekte ve özellikleri de bu modellere göre farklılık göstermektedir. Aşağıda bir kıyaslama tablosu bulabilirsiniz. Hangi kart işinize yarıyorsa o kartı kullanabilirsiniz.

## ESP8266 Kullanmaya Başlamak
Sabırsızlıkla gelmek istediğiniz nokta. Ancak Arduino'ya aşinaysanız bu noktaya gelmeden önce bilmeniz gereken birkaç şey var. ESP serisini Arduino ile programlamak isterseniz, USB kablosunu karta takıp, "upload" butonuna basmakla iş bitmiyor. Muhtemelen elinizdeki ESP serisi boş olarak gelmiş olacaktır. Dolayısıyla programlamak için önce bir nevi BIOS yüklemesi yapmanız gerekiyor. Yani Firmware ile Flash’ına yazmanız lazım. Arduino UNO'daki Bootloader gibi düşünebilirsiniz.

İşimizi daha da kolaylaştırmak için, ESP8266’yı NodeMCU aygıt yazılımı ile güncellemeyi inceleyeceğiz. Daha önce de belirttiğim gibi, ESP8266 modüllerin pin yapısı ve boyutu sebebiyle doğrudan programlamanıza izin veren kolay bağlantılar içermiyor. Ancak NodeMCU ve WeMos ile bu işlemler çok daha hızlandı. Modülü doğrudan micro USB ile Arduino'da programlayabilirsiniz. Ayrıca NodeMCU ile ESP8266 kodlamaya başlarsanız, hem pin yapısı hem de uyumluluğu sebebiyle çok tatlı ve kolay olduğunu fark edeceksiniz.

> **Not:** Arduino üzerinde bulunan TX, RX pinleri 5V ile çalıştığından dolayı ESP8266’ya zarar verebilir. Arduino'nun 3.3V pininin verebildiği akım miktarı ESP8266 için teorik olarak yeterli olmadığından, TTL üzerindeki 3.3V çıkışını kullanıyoruz.

## ESP8266 Firmware Güncelleme
![ESP8266 FTDI Bağlantı Şeması](assets/img/FVIBNJ9IPEID45H.webp)
*ESP8266 FTDI Bağlantı Şeması.*

Yukarıdaki bağlantı şemasına uygun FTDI bağlantınızı yaptıysanız devam edelim. Yazılım yüklerken GPIO 0 (sıfır) pininin toprağa gittiğinden emin olun. Normal kullanımda bu bağlantı yapılmıyor. Aşağıdaki link üzerinden NodeMCU’nun son yayınlanmış olan sürümünü indirmeniz gerekiyor. Integer versiyonunu kullanabilirsiniz.

- [NodeMCU Firmware Releases](https://github.com/nodemcu/nodemcu-firmware/releases)

Daha sonra bu aygıt yazılımını cihaza yüklemek için gerekli aracı aşağıdaki linkten edinmeniz gerekiyor.

- [nodemcu-flasher](https://github.com/nodemcu/nodemcu-flasher)

Burada Config altından, ayarlar simgesine tıklayarak indirdiğiniz son Nodemcu relase’i seçmeniz gerekiyor. Daha sonra Operation sayfasına gelerek yükleme işlemini başlatabilirsiniz. Bu işlem esnasında Mac adresleri, firmware flasher dosyasının bulunduğu klasörde Config içine kayıt ediliyor. Daha sonra buradan bunlara erişebilirsiniz. İlerleme esnasında gidişatı Log ekranından takip edebilirsiniz. Flashlama bittikten sonra sol alt köşede yeşil bir tik işareti gelecektir.

Artık FTDI üzerinden Arduino IDE ile ESP8266 modülünüzü programlayabilirsiniz. Ayrıca ufak bir bilgi vereyim, ESP8266 E-12 modülü Arduino Mega ve mod ile Arduino DUE den daha hızlı CPUya sahip (160 Mhz).

## WiFiManager Kütüphanesi
İşte wifi ile kontrolün en güzel tarafı bu kütüphane. Modül küçük olunca, projeyi küçültme gerekliliği artıyor, projeyi küçültme gerekliliği artınca devre kümesini de küçültmek istiyoruz. ESP ile ekstra Arduino yada Atmel gibi bir MCU kullanmamıza gerek kalmadığı gibi, küçük bir kart ile internete çıkıp, fiziksel dünyayı GPIO portları ile kontrol ediyorken, bu cihazı ortamlarda WiFi bağlantısını aracısız nasıl yapacağız? Yani projemizi yaptık, küçük bir kutuya cihazı koyduk. Açtık çalışıyor ama serial yok, ortamda ki wifi sisteme gömülmedi, peki nasıl iletişim kuracak?

İşte bu noktada ESP modülünün AP (Access Point) özelliği devreye giriyor. Bu modülü bir wifi bağlantısı yapmak için kullanmanın dışında bir wifi modem gibi de kullanabiliyoruz. Ve bu özellik ile WiFiManager kütüphanesinin gücü birleştiğinde dışarıdan kontrol edilebilen bir sistem doğuyor.

Starbuck ve benzeri yerlerde ki wifi sistemine bağlandığınızda internete çıkmadan karşınıza bir pencere açılır, bazı bilgiler ister, doğru girerseniz internete çıkarsınız. İşte AP modda WiFiManager tam olarak bunu yapmanıza olanak sağlıyor. Ayrıca karşılama ekranını bir template vasıtasıyla biraz HTML biliyorsanız rahatlıkla güncelleyebilir ve özelleştirebilirsiniz.

Öncelikle ESP sistemini yüklediyseniz arduino IDE için ve içinde WiFiManager yoksa [https://github.com/tzapu/WiFiManager](https://github.com/tzapu/WiFiManager) adresinden indirebilirsiniz. Nasıl yüklemeniz gerektiğini anlatmama gerek yok sanırım :) Bildiğiniz kütüphane yani.

Hazır example/örneklerden bir tanesini uygulayarak hemen kurcalamaya başlayabilirsiniz. Bu kütüphane ile çalışır hale getirdiğiniz sistem şu aşamaları izleyecektir;

- Gömülü sistemde tanımlı SSID bulunuyor mu? Bulunuyorsa bağlan.
- Bulunmuyorsa AP moduna geç. Bağlantı bekle
- AP Modda bağlantı geldiyse HTTPServer başlat
- Kullanıcıya seçenekleri sun, WiFi Tara, Seçtir, Bilgileri Al, Flasha Yaz, Yeniden Başla
- Kayıtlı Bilgiler ile Bağlantı varsa, Bağlan, görevlerini işle. Bilgiler hatalıysa ve bağlanadamıysan AP modu tekrar başlat, bağlantı bekle.

u prosedür doğrultusunda ESP modülünüz artık WiFi bulamadığı zaman dışarıdan bir WiFi bağlantılı cihaz (Telefon, Tablet, Bilgisayar) ile işlenebiliyor. Herşeyi ambalajladıktan sonra "ben bu cihazı wifi ye nasıl bağlayacağım" diye düşünmenize gerek yok.

Ayrıca default açılan arayüzü editlemek isterseniz kütüphane dosyaları içinde **WiFiManager.h** adlı dosyayı bir editör ile açarsanız ilgili bölümleri göreceksiniz. Eğer gördüğünüz kodlara dair bir fikriniz yoksa bence hiç dokunmayın derim. Ve mutlaka yedek alın. Göreceğiniz bölümde değişiklik yapmak ve kendi atınızı koşturmak isterseniz bilmeniz gerekenler; HTML, CSS, Native JavaScript olacaktır..