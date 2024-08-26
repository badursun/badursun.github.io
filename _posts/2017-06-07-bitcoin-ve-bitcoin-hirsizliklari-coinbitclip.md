---
layout: post
title: "Bitcoin ve Bitcoin Hırsızlıkları (coinbitclip)"
date: 2017-06-07 17:36
categories: ["Teknoloji", "Blockchain"]
tags: ["bitcoin", "satoshi", "coinbitclip"]
---

Bitcoin dijital bir para birimi olması nedeniyle dijital dünyanın da saldırısı altında. Merkezi bir otoritesininin de olmaması tabiki dolaylı yoldan hukuki bir savunma mekanizması da olmamasına neden oluyor. Yani birisi size ait bir bitcoini çalarsa, yapabileceğiniz hiç bir şey yok. Olaki buldunuz, kendi usulünüze göre çözmeye çalıştınız (dayak gibi :)), bu sefer siz suçlu oluyorsunuz. Yani yapmanız gereken en önemli birinci adım, güvenlik. İkinci adım güvenlik, üçüncü adım, güvenlik... Dijital dünyada açgözlü ve kötü niyetli bir çok insan anonim bir şekilde emellerine yönelik çalışmalarına aralıksız devam etmektedir. Haliyle, paranızı ve bilginizi güvende tutmak çok önemli. Geçtiğimiz günlerde patlak vermeye başlayan bir virüsden bahsedeceğim, adı coinbitclip veya nam-ı diğer trojan.coinbitclip. Yapısı gereği bulaştığı bilgisayarlarda (windows) bitcoinle para transfer edilirken siz fark etmeden bitcoin adresini içinde bulunan ve kopyaladığınız adrese en benzer adres ile değiştiriyor ve farkında olmadan kendi elinizle saldırganın hesabına parayı göndermiş oluyorsunuz.

Cüzdan bilgisi anonim olduğu için bulma şansınız yok. Merkezi bir otorite olmadığı için iptal etme şansınız da yok. Haliyle artık 2 kere değil 10 kere kontrol etmeden gönderim yapmamanız çok önemli.

Yaklaşık 3 ay kadar önce koinim'in bu konu ile ilgili kullanıcılarına bir mail atmıştı.

Bu virüsü bir yazılımcı olarak değerlendirirsem, çok akıllı ve sinsice bulduğumu söyleyebilirim. Her gün yeni dolandırıcılık vakaları görüyoruz. Her geçen gün taktikler, sistemler gelişiyor ve değişiyor. Yani yazılım dünyası tarafından bakarsak, çıta sürekli yükseliyor. Daha akıllı, zeki yazılımcılar türüyor. İşte tam bu noktada etik değerler devreye giriyor. Kişi kötü niyetliyse kendisi yararına iyi niyetli ve saf insanları kullanıyor. Filmlerde ki gibi sonu her zaman mutlu sonla bittiğini sanmıyorum.

Yaşanan olaylardan bir tanesine [şurada](https://community.coinbase.com/t/wallet-address-changed/9970/4) ki forumda görüntüleyebilirsiniz.

Gelelim hesabın durumuna; [buraya](https://blockchain.info/tr/address/19Y4NSiG8kkzwWjMD17euEaQ5PErpwxWkP) tıklayıp, blockchain de hesabın durumu görüntülenebilir.

Yazıyı hazırladığım an itibariyle **737 bin 022** **dolarlık** ödeme alındığı görünüyor. Cidden korkunç bir rakam. Üstelik forum ve gruplardan takip ettiğimde Türk kullanıcıların da bu durumdam muzdarip olduğunu görüyorum. Gerçekten üzücü. Kesinlikle başıma gelmez diye düşünmeyin, kontrolü elden bırakmayın ve virüs programlarını kullanın.
<h1>Bana Virüs İşlemez!</h1>
Hiç böyle düşünmeyin derim, zira bitcoin ve crypto piyasası merakınız varsa muhtemelen bir çok web sitesini geziyor, bilgi edinmeye, yenilikleri takip etmeyi amaçlıyorsunuz fakat farkında olmadan belki de yüzlerce virüs, malware tarzı art niyetli yazılımlara maruz kalıyor olabilirsiniz. Bu konuda önlem almanın sonu yok, paranoyak derecede önlemler alınabilir fakat iyi seviyede önlem almanız için yapmanız gereken bazı basit şeyler var.

- Olmazsa olmazımız, güncel veritabanına sahip bir virüs programı kullanmak.
- Bilmediğiniz güvenmediğiniz web sitelerini ziyaret etmemek de önemlidir. Etmek zorundaysanız, gizli tarayıcı üzerinden bazı javascript, flash gibi bileşenleri kapalı tutarak gezinebilirsiniz.
- bilgisayarınızdan emin değilseniz, tarama yapmadan sizin için önemli olan verilere erişmeye çalışmayın (online bankacılık, walletlar, rig hesapları vb gibi)
- Mutlaka 2FA (Two Factor Authentication - 2 Adımlı Doğrulama) kullanın.
- Bilgisayarınızın herhangi bir yerinde önemli şifrelerinizi yazılı saklamayın
- Zorunlu üyelik yapmanız gerekiyorsa güvenmediğiniz bir siteye farklı bir atıl e-posta adresi ve bu tip siteler için kullandığınız, diğer şifrelerinizden tamamen farklı bir şifre kullanmanızı tavsiye ederim.

## Nedir bu 2FA (Two Factor Authentication - 2 Adımlı Doğrulama)
2FA, kimliğinizi doğrulamanız için birden fazla yöntem gerektirdiği için çok daha iyi bir hesap güvenliği sunar. Yani bir kişi şifrenizi ele geçirse bile, o kişi cep telefonunuzu ya da diğer ikincil doğrulama yöntemlerinden birini elde etmeden hesabınıza erişemez. Bunun için farklı yöntemler mevcuttur. Daha detaylı okumak için [**buraya**](https://www.teknolugat.com/iki-faktorlu-kimlik-dogrulama-2fa-nedir-neden-kullanmaliyim/) tıklayabilirsiniz.

- En yaygın olanı Google tarafından geliştirilen Authentication adlı yazılım. Hesabınız ile entegre olarak bir QRCode okutup, ilgili sitenin 2FA yapısını aktif ettikten sonra, telefonunuzda ki uygulama size 60 saniye boyunca geçerli benzersiz şifreler üretmektedir. Bu şifreler sadece üretildiği andan itibaren 60 saniye içerisinde oluşturulmalıdır. 61. saniyede girdiğiniz şifre geçerli olmayacaktır. Güvenlik, maliyet ve entegrasyon açısından oldukça tercih edilen bir sistemdir. Tabii Gmail hesabınızı da bu türde bir doğrulama sistemine geçirmeyi unutmayın. Ayrıca 2FA Kodlarını okuturken, mutlaka açık anahtarın yedeğini de almalısınız.
- SMS Doğrulama ise daha çok profesyonel yerlerin tercih ettiği, maliyetli bir doğrulama yöntemi olup, sorumluluğu, neredeyse %90 doğrulanabilir bir operatör üzerinden kişiye bağlayan bir mantığa sahiptir. Sonuçta numara, bir operatör üzerinden bir kişiye kayıtlıdır, kullanıldığı yer neredeyse baz istasyonuna bağlı olarak takip edilebilir. Bu sebeple Google Auth`a göre bir nebze daha tercih sebebidir.
- E-Posta Doğrulama, aktif e-posta kullanıcıları için oldukça hızlı bilgilendirme veren ve giriş esnasında size bir link göndererek tıklamanızı isteyen bir doğrulama yöntemidir.

Bu yöntemler arasında ben mesela BlockChain hesabıma girişte e-posta doğrulama ve Google Authentication doğrulama yöntemlerini tercih ediyorum. 2 faktörden 3 faktöre, parolayı da sayarsak 4 faktöre çıkmış bir güvenlik önlemi almış oluyorum. Lakin bu yöntem sadece hesabımın çalınmasını önlemeye yetiyor şimdilik. Konumuzun başında ki Coinbitclip virüsünü ele alacak olursak, bütün bu güvenlik önlemleri bir anda çöp olabilir çünkü virüsün şifrelerime ihtiyacı yok. Sadece bir BTC adresi kopyaladığımı hissetmesi ve arka planda o adresi değiştirmesi bunun için yeterli. Bütün önlemlerim, bir anlık dikkatsizlik yüzünden çöpe gidebilir.

Tedbiri elden bırakmamak önemli.

## Coinbitclip Nasıl Temizlenir?
Eğer böyle bir virüsten muzdaripseniz, temizlemek için Symantec firmasının temizleme uygulamasını [**buraya**](https://security.symantec.com/nbrt/npe.aspx?mc_cid=78c40b6819&amp;mc_eid=d2eb6e46af) tıklayıp kullanabilirsiniz. Tarama yaprak temizliğinizi tamamladıktan sonra aslında benim tavsiyemi soracak olursanız, zamanınız varsa, temiz bir format atıp, tüm şifrelerinizi değiştirmenizi tavsiye ederim.

Ne demişler, en güvenli bilgisayar, fişi çekili olandır.

NOT: Yazıyı tamamladığımda hesapta 735,279.77 USD lik işlem yapılmıştı...