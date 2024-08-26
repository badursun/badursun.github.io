---
layout: post
title: "Klasik ASP ve Bellek Yönetimi (Classic ASP and Memory Management)"
date: 2019-04-05 11:31
categories: ["Yazılım", "Classic Asp"]
tags: ["classic asp", "asp", "memory management"]
---

Baştan söylemeliyim, detaylı ve uzun bir yazı olacak. Bu yazıyı yazmamda ki sebep; son bir kaç hafta içerisinde farklı insanlar ile Klasik ASP üzerine uzun sohbetler yaptık. Asla gelenekselci bir kafada değilim, Klasik ASP iyidir demiyorum ama kötüdür de diyemem. Çünkü bir yazılımı başarılı ve performanslı kılan, onun mimarı ve mimarın teknikleridir.

## İyi Bir Yazılım
İyi bir yazılım nasıl olmalıdır? Bu soru aslında oldukça detaylandırılabilir bir konu fakat genel hatlarıyla toparlayacak olursak, iyi bir yazılım ortaya koyabilmek için sadece düzenli kod yazmak, set ettiğiniz objeleri kapatmak yeterli değildir. Yazılımın tüm iletişim katmanlarında doğru bir yapı ve iletişim prosedürü oluşturmak gerekir. Eğer bugüne kadar sadece basit web sitesi yazılımları yaptıysanız, bu yazıdan sonra emin olun ufkunuz genişleyecek. Yıllardır tek bir dil üzerine kendimi eğittim, o da klasik asp. Bu şekilde ilerlemek için geçerli sebeplerim vardı. En önemlisi; çok yalın ve basit bir dil olmasına rağmen, söylenenlerin aksine oldukça başarılı bir dil olması ve artık günümüz zamanında ki internet (bilgisizlik) bilgi kirliliğinden dolayı başarısızlıkla damgalanması.

Hadi gelin en baştan başlayalım ve size bugüne kadar edindiğim deneyimleri bir çırpıda örnekleriyle birlikte anlatayım. Ama şunu unutmayın, burada anlatacağım herşey bol miktarda terim, farklı yazılım katmanlarına dair prosesler ve en az seviyede sunucu yönetimi içeriyor olacak.  Mümkün olduğu kadar açıklayıcı anlatmaya çalışacağım. Bu süreçlerden geçerken oldukça yüksek miktarda dataları bulunan ve anlık ziyaretçi sayıları yüksek olan web siteleri için geliştirdiğim çözümlerden edindiğim deneyimlerimi de anlatıyor olacağım.

## Hız Her Şey'dir
Genel olarak mottom budur. Hız, Her Şeydir.. Yoğun yüklü bir web sayfası sunucu tarafında ciddi yükler oluşturur. Bunun sebebi, yazdığınız bir web sayfası kod öbeğinin çalışırken bir çok katmandan geçmesi, algoritmik işlemler yapması ve taleplere cevap vermesi gerekir. Örnek verecek olursak, bir kullanıcı, bir web sitesinin ana sayfasını çağırdığı anda;

- İstek sunucu tarafından işlenir
- İlgili komut dosyası çalıştırılır
- İlgili sorgular veritabanı sistemine iletilir
- Veritabanından gelen sonuçlar işlenir ve belki tekrar bu süreç yaşanır
- tüm veriler toplantığında derlenir
- varsa dinamik yada statik sıkıştırma uygulanır
- kullanıcıya iletilir. Fakat süreç burada bitmez;
- Kullanıcı tarafından alınan dosya içinde ki statik kaynaklar (css, js, imaj vb) sunucudan talep edilir
- sunucu bu taleplere tek tek cevap verir

Bu en basit halidir sürecin. Bir de arka planda görmediğiniz ve çoğu zaman dikkate bile almadığınız bir OTURUM (session) süreci vardır. İşte bu süreç doğru yönetilmediğinde bellek tüketimine sebep veren en büyük sorunlar arasında 1. sıradadır.

## Doğru Oturum Yönetimi
Neredeyse hiç kimsenin umrunda olmayan fakat yeri geldimi çok kullandığı **session** objesi, doğru bir şekilde kullanılmadığı zaman çok baş ağrıtabilir. Nerede kullanıyoruz bu session objelerini hemen bir örnek vereyim; kullanıcı login işlemlerinde.. Bazen de hafıza tutmak ve farklı kod dosyaları arasında veri taşımak için kullanıyoruz. Peki; bu objeler ile işiniz bittiğinde sonlandırıyoru musunuz?

Yine ben cevap vereyim sizin adınıza :) Ne gerek var? Zaten yok oluyor. Çünkü IIS (Internet Information Service) bu oturumları 20 dakika da bir terminate (sonlandırma) işlemi yapıyor nasılsa. Peki; bu oturumu biz sonlandırmazsak yada kontrol etmezsek ne olur?

Kısa bir örnek vereyim: Default değeri 20 dakika olan bir session objesine atanmış bir verimiz olduğunu düşünelim. Sunucu gelen her bir kullanıcı için açtığı objeyi 20 dakika boyunca, kullanıcı tamamen oturumu terk etse dahi tutmaya devam eder. Anlık olarak 1000 kişinin birer dakika arayla siteye ard arda girdiğini düşünelim. Toplam sunucuda bekletinden objelerin süresi 20.000 dakika yapar (teoride). Dolayısıyla bu yükü siz hafızaya yani RAM'e (memory) yüklemiş olursunuz. Bu veriler disk üzerine yazılmaz, geçici bellekte, sunucunun RAM sisteminde ayrılan sanal belleklerde tutulur. Çünkü erişilmesi hızlı olmalıdır.

Klasik ASP ile diğer diller arasında ki önemli noktalardan birisi budur. Diğer dil çatılarında Garbage Management vardır, atıkları toplamanız için bazı betikler sunar. Fakat  Klasik ASP de bunları sizin yapmanız gerekiyor. İşte iyi bir yazılımı yapan mimar bunu unutmamalı. Peki nasıl olacak?

## Global.asa
Önce basit bir tabir ile global.asa nedir bir bakalım. Sunucuya bir erişim gerçekleştiğinde veya sunucu çalışmaya başladığında yapılacak işleri sıralayan bir çeşit ASP işlem dosyasıdır. Uzantısı ASP olmadıgı gibi kodları içerisinde de yazılmayan bir VBScript’tir diyebiliriz. Yazım dili yani bildiğiniz ASP kodları gibi değildir. Peki global.asa prosesleri nelerdir ?

- Web sunucusu çalışmaya başladığı zaman
- Bir kullanıcı web sitesine giriş yaptığı zaman
- Bir kullanıcı web sitesinden çıkış yaptığı zaman
- Web sunucusu yeniden başlatıldığında (recycle)

tetiklenen bir takım alt prosedürleri bulunur. Bunlar global.asa dosyasının içinde tanımlanır ve bir rutin olarak çalışırlar. Şimdi örnek bir global.asa kod betik yapsına bakalım.

### global.asa

```vb
<script language="vbscript" runat="server">
	sub Application_OnStart
		'burada sunucu çalışmaya başladığında yapılacak rutinler eklenir.
	end sub

	sub Application_OnEnd
		'burada uygulama çalışmasını bitirdiğinde yapılacak rutinler eklenir.
	end sub

	sub Session_OnStart
		'burada oturum başladığında (yeni bir kullanıcı geldiğinde) yapılacak rutinler eklenir.
	end sub

	sub Session_OnEnd
		'burada oturum bittiğinde (default 20dk) yapılacak rutinler eklenir.
	end sub
</script>
```

Genel hatları ile kodu incelediğinizde 4 adet blok alt prosedür göreceksiniz.

- Application_OnStart
- Application_OnEnd
- Session_OnStart
- Session_OnEnd

Biraz ingilizceniz varsa zaten rutinlerin tanımlarından ne olduğunu kavrayabilirsiniz. Gelelim bu rutinleri daha detaylı açıklamaya. Fakat bu sefer sırayı biraz değiştireceğim ve rutinlerin sunucu tarafında oluşum sıralarına göre ilerleyeceğim. Böylece rutin yapısını sunucu katmanında gerçekleşme sırasına göre kavramanızı istiyorum.

**Application_OnStart**
Bu rutin, sunucu çalışmaya başladığı zaman gerçekleşir. Burada ki sunucu fiziksel anlamda ki sunucu değil, yazılımınızı derleyen ve bütün prosesleri işleyen IIS sunucusudur ve zaten bu yüzden Application (Uygulama) olarak tabir edilir. Her web sitesi, işlemleri ilerletebilmek için kendisine ayrılmış bir Application Pool (Uygulama Havuzu) üzerinde kendine ayrılan limitlenmiş kaynaklar kullanır. İşte bu kaynaklar ilk çalışmaya başladığı anda bu rutin devreye girer ve altında bulunan kodları çalıştırır. Peki ne zaman OnEnd olur? Bunu ileride detaylandıracağım fakat şunu şimdiden bilin; her uygulamanın belirli şartlarda bir recycle (yenilenme) zamanı vardır. Bu şartlar tamamiyle değiştirilebilir fakat default değerlerde uygulamanın recycle (yenilenme) zamanı, ilk başladığı andan itibaren 3600 dakikadır. Yani çalışan uygulama 3600 dakika da bir açtığı proses kimliklerini, oturumları, hafızada ki değişkenleri siler ve yeni bir işlem başlatır.

**Session_OnStart**
Bu rutin aslında tamamen benzersiz kullanıcılara bağlı bir rutindir. Her benzersiz bir kullanıcı uygulamaya eriştiği anda, sistemde onun için bir bellek ayrılır. Tanımlanmış değişkenler, oturum bilgileri, set edilen objeler ( str_ad = "burak", session("bilgi")="deneme", set rs = conn.execute gibi) gelen kullanıcı için ayrılan alanda saklanır. Yazının başında bahsettiğim 20.000 dakikalık süreçler, bu verileri geçici olarak saklamak ile ilgilidir. İşte her bir oturum açıldığında bunlar olur. Küçük bir bilgi, her oturumun benzersiz bir ID yani kimlik numarası vardır, bu kimlik numarası ile OnStart ve OnEnd işlemleri ayrı ayrı yürütülür.

**Session_OnEnd**
Bu rutin ise, bir üst bölümde bahsettiğim rutin işleminde başlatılan oturum sonlandığı anda gerçekleşir. Yine hatırlatmakta fayda var; siz değiştirmediğiniz sürece bu oturum IIS de 20 dakika süreyle set edilmiştir. Yani ilk oturum başladı, siteye bir kişi geldi ve gitti, o oturum 20 dakika boyunca bellekte tutulacaktır. 20 dakikanın sonunda oturum sahibi hala bir harekette bulunmadıysa bu rutin, o oturum için devreye girer. OnStart esnasında alınan işlem kimlik numarası ile OnEnd işlemleri gerçekleşir.

**Application_OnEnd**
Ve geldik en son rutinimize. Application_OnStart da bahsettiğimiz recycle süreci yada soft-reset süreci gerçekleşmediği sürece bu prosedür çalışmayacaktır. Fakat bu prosedürü tetiklemenin bir diğer yolu da, global.asa dosyanızda değişiklik yapıp kaydetmenizdir. Yani global.asa dosyası fiziksel dosya olarak her yenilendiğinde IIS üzerinde ki Application Pool uygulamanız yeniden başlar. Bu küçük çaplı bir recycle etkisi yaratır.

## Peki global.asa ve Performans Etkisi Nedir ?
Aslında bir etkisi yok fakat performans kazanmak, en kötü ihtimalde performansı korumak için etkili bir biçimde kullanılabilir.

Örnek vermek gerekirse yazının başında anlattığım 20.000 dakikalık memory yorgunluğunu gidermek için kullanılabilir.

Misal, uygulama başlatıldığı andan itibaren oturum (session) ve App Pool (Application) üzerinde tutulan geçici bilgiler temizlenmek için, hem oturum bazında hemde global bazda kullanılabilir. Yani bir kullanıcı oturumu kapandığında bütün geçici bilgileri boşaltabilirsiniz.

Bunu en optimize biçimde tutmak için bir **global.asa** dosyası hazırladım. Kodları direkt sunucunuzun ana dizinine (www veya httpdocs) atabilirsiniz.

```vb
<script language="vbscript" runat="server">
sub Application_OnStart
	Application("visitors")=0
end sub

sub Session_OnStart
	Application.Lock
	Application("visitors")=Application("visitors")+1
	Application.UnLock
end sub

sub Session_OnEnd
	Application.Lock
	Application("visitors")=Application("visitors")-1
	Application.UnLock

	'Application.Contents.RemoveAll
	Session.Contents.RemoveAll
	Session.Abandon
end sub

sub Application_OnEnd
	Application.Contents.RemoveAll
	Session.Contents.RemoveAll
end sub
</script>
```

Bu yapıda bir kaç ufak kod daha ekledim. İlki; uygulama terminate edildiğinde Application'ı temizliyor ve Oturum terminate edildiğinde session'ları temizliyor. İkincisi se Web sitenizde oturumu düşmemiş kaç kullanıcı bulunuyor, diğer tanımıyla kaç kullanıcı online bulunuyor, bu kod yapısında takip edebilirsiniz.

Online kullanıcıları göstermek istediğiniz yere aşağıda ki kodu yapıştırmanız yeterli.

```vb
Response.Write Application("visitors")
```

Online kullanıcı kısmını açmak gerekirse; ne demiştik? Session_OnStart ve Session_OnEnd prosedürleri; yeni bir kullanıcı oturumu açıldığı ve kullanıcı sistemi terk ettikten sonra x süresinde terminate edildiğinde çalışan prosedürlerdir. Application nesnesi ise global anlamda veri tutabildiğiniz geçici bellek erişiminizdir. Biz bu iki prosedürün bize sağladığı farklı iki etkileşim katmanını kullanarak, her kullanıcı giriş yaptığında Application içinde "visitors" alanına 1 kişiyi ekledik/çıkarttık ve toplam online kişi sayısını almış olduk.

## Daha.. Daha fazla..
Her zaman daha fazlasını isteriz. İstemekte haklıyız, çünkü her zaman daha fazlasını alabiliriz (bir yere kadar). Ama bunun için dediğim gibi, en başından doğru bir mimariyle başlamalıyız. Bir kaç adımda genel olarak neleri optimize etmelisiniz kısaca bahsedip, konuya dönelim;

- Sunucu Optimizasyonu
- Sunucu Taraflı Yazılım Optimizasyonu
- İstemci Taraflı Yazılım Optimizasyonu
- Veritabanı Optimizasyonu

Bunları yazının ilerleyen kısımlarında anlatacağım, şimdilik bir imleme olarak aklınızda bulunsun.

Daha fazlası dedik ama nasıl daha fazlası. Öncelikle yazılım hazırlarken bir çok objeyi deklare ederiz, yani tanımlarız. Bunlar geçici kullandığımız değişkenler, bağlantı ve component'ler için açtığımız objeler yada ucu açık bırakılmış array'lar olabilir.

Bunları mutlaka işiniz bitince kapatın. Hatta bir response.end komutundan önce mutlaka kapatın ki, yinede açık kalmasın. Çünkü response.end komutundan sonra hiç bir komutunuz çalışmaz.

Bunu alışkanlık edinemiyorsak veya proje uzadıkça yenileri ekleniyorsa, hoşgeldin **OOP** diyelim.

## Nedir Bu OOP (Obejct Oriented Programming)
Hakikaten, nedir bu OOP? OOP, yani Object Oriented Programmin, nam-ı diğer Nesne Yönelimli Programlama özetle şöyle açıklanır; hayatı nesnelere bölmektedir. Kullandığımız yordamları direk uygulama kodunu yazmayıp, sınıflar içine yazıyor ve bu sınıflardan türettiğimiz nesneler üzerinden çağırıyor isek OOP yapıyoruz demektir.

OOP Programlama temelinde 3 prensibe sahiptir. **Encapsulation, Inheritance, Polymorphism**

Bu kısımları kısaca açıklamak gerekirse;

1- Encapsulation; Nesne hakkındaki bilgiler anlamına gelir. Örneğin bir arabanın rengi, modeli gibi.
2- Inheritance;Bir nesnenin bir başka nesne üzerine kurulmasıdır. Ortak özelliklerin bağlanmasıdır da diyebiliriz.
3- Polymorphism; Belli bir işlemin birçok nesne tarafından kullanılmasıdır.

OOP hakkında bu kadar detay yeter, çünkü çok uzun bir konu, ama bu özet ufak bir aydınlanma için ateş olacaktır. Classic ASP de bu yapıyı kurmak için Class sınıfını ve alt objelerini bilmeniz gerekiyor. Aslında kullanımı oldukça basittir ve tıpkı global.asa da olduğu gibi class (sınıf) çalıştığında ve tamamlandığında (terminate yada dispose) tekrarlayabileceğiniz iki sabit alt prosedürü bulunur. İşte bu size "zorla alışkanlık" kazandırma dürstüsünü hafifletecek bir yoldur.

## Hadi Bir Class Yazalım
Ne demiştik, bir hatırlayalım:

> iyi bir yazılım ortaya koyabilmek için sadece düzenli kod yazmak, set ettiğiniz objeleri kapatmak yeterli değildir.

Ama yinede açtığımız objeleri kapatacağız ve değişkenlerimizi tanımlayacağız :) Şimdi örnek bir Class oluşturmayı, çağırmayı, kullanmayı ve yok etmeyi öğreneceğiz fakat ilk önce Sınıf Yapısını (Class Structure) görelim:

```vb
Class BasitSinif
   Private degisken1, degisken2
   Public degisken3, degisken4

   ' Class Set Edildiğinde Yapılacak Prosedür
   Private Sub Class_Initialize()

   End Sub


   ' Class Terminate Edildiğinde Yapılacak Prosedür
   Private Sub Class_Terminate()
      
   End Sub
End Class
```

Bir Class hazırlayabilmek için bu yapıyı kullanmak gerekiyor. Önce Class ve o Class için isim tanımlanır. Yukarıda ki örnekte BasitSinif olarak verdik. Daha sonra Class set edildiğinde ve Terminate edildiğinde çalışacak prosedürler tanımlanır. Burada dilediğiniz gibi ASP kodlarını kullanabilirsiniz. Ve basit bir şekilde Class objesini bir objeye atayalım:

```vb
Dim YeniObje
Set YeniObje = New BasitSinif
```

Kısaca ne yaptık? Neden yaptık bir değinelim; öncelikle YeniObje isminde bir değişken tanımladık ve performans yönetimimiz için bu objeyi Dim ile yazılıma belirttik. Daha sonra Set komutu ile YeniObje adlı değişkenimizi; BasitSinif adlı Class üzerinden objeye çevirdik. Artık BasitSinif adlı Class içinde ki verilere YeniObje adı ile erişebileceğiz. Şimdi BasitSinif içinde ufak bir fonksiyon tanımlayalım, ardından bunu nasıl kullanacağımıza bakalım;

```vb
Response.Write "BasitSinif Deneme Kodu<br>"

Class BasitSinif
   Private degisken1, degisken2
   Public degisken3, degisken4

   ' Class Set Edildiğinde Yapılacak Prosedür
   Private Sub Class_Initialize()
      Response.Write "BasitSinif: Class_Initialize() Çalıştı!<br>"
   End Sub


   ' Class Terminate Edildiğinde Yapılacak Prosedür
   Private Sub Class_Terminate()
     Response.Write "BasitSinif: Class_Terminate() Çalıştı!<br>"
   End Sub

   Public Function Toplama(veri, digerveri)
      Response.Write "BasitSinif: Toplama() Çağırıldı!<br>"
      Toplama = veri + digerveri
   End Function
End Class

Response.Write "Dim YeniObje Tanımlanıyor!<br>"
Dim YeniObje
Response.Write "Dim YeniObje Tanımlandı!<br>"

Response.Write "Set YeniObje oluşturuluyor!<br>"
Set YeniObje = New BasitSinif
Response.Write "Set YeniObje oluşturuldu!<br>"

Response.Write "Toplama Fonksiyonu ilk defa çalıştırılıyor!<br>"
Response.Write YeniObje.Toplama(10, 15) ' Çıktı 25 olacaktır.
Response.Write "Toplama Fonksiyonu ilk defa çalıştırıldı!<br>"

Response.Write "Toplama Fonksiyonu ikinci defa çalıştırılıyor!<br>"
Response.Write YeniObje.Toplama(955, 5) ' Çıktı 960 olacaktır.
Response.Write "Toplama Fonksiyonu ikinci defa çalıştırıldı!<br>"

Response.Write "YeniObje Terminate Ediliyor<br>"
Set YeniObje = Nothing
Response.Write "YeniObje Terminate Edildi<br>"
```

Gördüğünüz gibi Toplama adında bir fonksiyon hazırladık, bu fonkisyon 2 adet parametre alıyor ve bunları toplayıp, geri döndürüyor. Class nesnemizi oluşturup, tanımladıktan sonra bu fonksiyonu, Class içinden çağırdık, sonucumuz aldık ve en son Set YeniObje = Nothing ile class sınıfımızı sonlandırdık.

Burada dikkat etmeniz gereken iki şey var; İlki: Set YeniObje = New BasitSinif dediğimizde, herhangi bir hata almadıysanız/yapmadıysanız, Class içinde ki **Private Sub Class_Initialize()** devreye girer. Bu yüzden kodu çalıştırdıktan sonra ekranda ki çıktılara dikkat edin, sırasıyla ne çalıştı göreceksiniz.

Bu kod yapısının çalıştığında vereceği çıktı şöyledir; Dikkat etmenizi istediğim şey ise; **BasitSinif: Class_Initialize() Çalıştı!** ve **BasitSinif: Class_Terminate() Çalıştı!** kısımları olacaktır.

**BasitSinif Deneme Çıktısı**
```bash
Dim YeniObje Tanımlanıyor!
Dim YeniObje Tanımlandı!
Set YeniObje oluşturuluyor!
BasitSinif: Class_Initialize() Çalıştı!
Set YeniObje oluşturuldu!
Toplama Fonksiyonu ilk defa çalıştırılıyor!
BasitSinif: Toplama() Çağırıldı!
25Toplama Fonksiyonu ilk defa çalıştırıldı!
Toplama Fonksiyonu ikinci defa çalıştırılıyor!
BasitSinif: Toplama() Çağırıldı!
960Toplama Fonksiyonu ikinci defa çalıştırıldı!
YeniObje Terminate Ediliyor
BasitSinif: Class_Terminate() Çalıştı!
YeniObje Terminate Edildi
```

İşte bu kadar basit. Hani yazının başında bahsettiğim "bazı şeyleri alışkanlık edinmelisiniz" alanında ki alışkanlıklarınıza Nesne Yönelimli programlaya geçerek bir son verebilirsiniz. Daha pratik uygulanmasıyla; her yazdığınız kodun sonunda bir obje silsilesi kapatmaktansa; bu objeleri nesneler ile oluşturup yönetmek ve kendi başlatma/sonlandırma prosedürleri içinde Terminate etmek çok daha kolay, verimli, sağlıklı bir sonuç doğuracaktır.

Class yapısını ileride daha geniş örneklerle ve hazırlamakta olduğum yardımcı bir sınıf kütüphanesiyle sizlere aktaracağım.

## Bu Bitti, Sırada ki Gelsin
Şimdi sırada **web.config** dosyası var. Mutlaka projelerinizde kullanmanız gereken bir dosya. Tabii kullanmak kadar, ne olduğunu da anlamanızı tavsiye ederim. Çünkü burada yapacağınız bir hata, sisteminizin kararsızlaşmasına belkide hiç çalışmamasına sebep olacaktır. Peki nedir web.config dosyası?

Web.config dosyası web sitesinin ortak konfigürasyon ayarlarının tutulduğu XML tabanlı bir dosyadır.Bu dosya sistenin kök dizininde olabileceği gibi belli dizinler altındada olabilir. Sitenin kök dizininde olduğunda dosyadaki bilgiler tüm siteyi etkiler, eğer bir dizin altına yerleştirmişsek dosyadaki bilgiler sadece o dizin altındaki sayfaları etkileyecektir. Web.config dosyası genelde sitenin ortak bilgilerini düzenler.Bu bilgilere örnek olarak veritabanı bağlantısı kurmak için kullandığımız connectionstring leri verebiliriz.

> Bu yazı 5 Nisan 2019 tarihinden sonra güncellenmemiştir fakat 22 Aralık 2022 tarihinde yayına alınmıştır. Dolayısıyla bu yazının üzerine eklenecek yüzlerce yeni deneyim ve araştırma bulunmaktadır. Blog temasını değiştirip, daha sade bir arayüz ile yazıları eklemeye devam edeceğim.

Göz gezdirebileceğiniz 2 repo'mu bırakıyorum:

- [https://github.com/badursun/ClassicASP-Helper](https://github.com/badursun/ClassicASP-Helper)
- [https://github.com/badursun/ClassicASP-Master-Library](https://github.com/badursun/ClassicASP-Master-Library)