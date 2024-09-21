---
layout: post
title: WhatsApp Gizlilik Politikası Güncellemesi Bilmeniz Gereken Her Şey Burada
date: 2021-01-12 08:46:00
categories: ["Teknoloji", "Veri Güvenliği"]
tags: ["coverme", "whatsapp", "privacy"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/whatsapp-privacy-policy-66eea9f911f9f.webp"
  alt: "Javascript"
---

Yeni güncellenmiş WhatsApp Gizlilik Politikası 8 Şubat 2021’de yürürlüğe girecek. Bunların çoğu veri toplama için detaylandırma ve gerekçelendirme olsa da, satırlar arasında çok şey oluyor. Yeni WhatsApp Gizlilik Politikası, platformun Facebook’unki de dahil olmak üzere üçüncü taraf entegrasyonuyla nasıl davrandığına odaklanıyor.

Er ya da geç, Instagram ve WhatsApp dahil tüm Facebook hizmetleri, kullanıcı verilerinin birleşik tanımlayıcıları olarak hizmet verecek. Bu size mükemmel bir senkronize deneyim sunacak olsa da, aynı zamanda bir bedeli de olacaktır.

## WhatsApp’ın Güncellenmiş Politikası Ne Diyor?
Güncellenen WhatsApp gizlilik politikası, çeşitli özellikleri etkinleştirmek için şirket tarafından toplanan verileri detaylandırıyor. Varsayılan olarak WhatsApp, diğer şeylerin yanı sıra WhatsApp gruplarınızın adı ve ekran resimleri ve konumlarınız dahil olmak üzere uygulama kullanımınızda ki her türlü veriyi saklar.

Sözleşmeden küçük bir bölümde şöyle bahsediliyor:

> “Hizmetlerimizi kurduğunuzda, eriştiğinizde veya kullandığınızda cihaza ve bağlantıya özgü bilgileri topluyoruz. Bu, donanım modeli, işletim sistemi bilgileri, pil seviyesi, sinyal gücü, uygulama sürümü, tarayıcı bilgileri, mobil ağ, bağlantı bilgileri (telefon numarası, mobil operatör veya ISS dahil), dil ve saat dilimi, IP adresi, cihaz işlemleri gibi bilgileri içerir bilgiler ve tanımlayıcılar (aynı cihaz veya hesapla ilişkili Facebook Şirket Ürünlerine özgü tanımlayıcılar dahil). “

## Peki, Whatsapp verilerime erişebilir mi?
İşin aslı, teoride WhatsApp yalnızca meta veri dediğimiz yazışma istatistiklerinize erişebilir (ne zaman online olduğunuz, konumunuz, kiminle yazıştığınız, pay ile ödemeleriniz vs.) sohbetin içeriğine ve yazışmalara dair hiçbir bilgiye whatsapp erişemez. Nasıl biliyoruz? WhatsApp; yukarıda bahsettiğim meta veriler dışında her şeyi Signal protokolü ile uçtan uca şifreleme yaparak gönderir. Peki signal protokolü güvenli midir? Bir çok dökümana ve açık kaynaklılığına bakarsa cevap evet olacak. Signal Protokolü Açık Kaynaktır, araştırmacılar tarafından testlere tabii tutulmuş ve güvenilir olduğu kanıtlanmıştır.

> Signal Protokolü (eskiden TextSecure Protokolü olarak bilinir), sesli aramalar, video görüşmeleri ve anlık mesajlaşma konuşmaları için uçtan-uca şifreleme sağlamak için kullanılabilen federe edilmemiş bir şifreleme protokolüdür. Protokol, Open Whisper Systems tarafından 2013 yılında geliştirildi ve daha sonra Signal olan açık kaynak TextSecure uygulamasında tanıtıldı . O zamandan beri, “dünya çapında bir milyardan fazla insanın” sohbetlerini şifrelediği söylenen WhatsApp gibi kapalı kaynak uygulamaları içine uygulanmıştır. Ayrıca Google Allo’nun “gizli mod” için uyguladığı gibi Facebook Messenger da isteğe bağlı “gizli sohbetler” için signal protokolünü sunduğunu belirtmiştir. Protokol Double Ratchet Algoritması , prekeys ve üçlü Diffie-Hellman (3-DH) el sıkışma ‘ yı kombine eder ve Curve25519 , AES-256 ve HMAC- SHA256’yı temel olarak kullanır.

Dolayısıyla WhatsApp uçtan uca şifrelenmiş sohbetlerinizi hiç bir şekilde okuyamaz. Ayrıca WhatsApp sohbetlerinizi yedeklemek için bir sunucu kullanmıyor, yedeklerinizi siz telefonunuza ait yazılımın barındırma servislerinde yedekliyorsunuz. Sohbetlerinizi Drive veya iCloud’a yedeklerseniz bile, yedeklenen sohbetler Google ve Apple’ın gizlilik politikaları kapsamına girer . Basitçe söylemek gerekirse, WhatsApp, üçüncü taraf entegrasyonunu nasıl kullandığınızdan sorumlu değildir. Ayrıca kendisi verilerinizi yedeklemediği için verilerinize de erişmesi mümkün değildir.

## Tamam ama Meta Verilerimide paylaşmak istemiyorum
Meta-verilerimizi de paylaşmak istemiyoruz, buu doğru bir yaklaşımdır. Nereden online olduğum, ne zaman olduğum vs gibi bilgileri WhatsApp, dolayısıyla Facebook şirketleriyle paylaşmak istemeyebilirim. Bu nedenle tepki gösteriyorsam bu anlaşılır bir tepkidir. Ama akıllı telefonumuzu internete çıkardığımız anda zaten Google veya Apple’a bu bilgileri veriyoruz. Hatta istesenizde istemesenizde servis sağlayıcınıza (Turkcell, Vodafone, TTNet vb) da bu bilgileri veriyorsunuz.

## O zaman ne kullanacağız?
Bu ucu çok açık bir sorudur. İşin en basite indirgenmiş halinde, devlet sırrı saklamıyorsanız ve paylaşmıyorsanız, pek korkacak birşeyiniz yok 🙂 Ama yinede diyelim ki WhatsApp’a meta verilerimizi vermek istemedik ve Telegram’a geçtik.

Telegram; varsayılan olarak uçtan uca şifreleme yapmıyor. Yani gönderdiğiniz her ileti telegram sunucularında okunuyor, şifreleniyor ve karşıya iletiliyor. Peki secret-chat özelliğini aktif ederseniz ne oluyor? Bu durumda telegram’ın geliştirdiği MTProto 2.* olarak adlandırılan bir protokol ile uçtan uca şifreleme devreye giriyor ama MTProto güvenilir midir, bir arka kapısı var mıdır bunu hiçbir zaman bilemeyeceğiz çünkü protokol kapalı kod geliştirilmiştir. Yalnızca Relegram’ın beyanını esas alabilirsiniz. Bu durumda Relegram, WhatsApp’tan güvenilir demek abesle iştigaldir.

Peki uçtan uca şifreleme yapan, açık kaynak bir uygulama var mıdır? Evet, Signal uygulaması da WhatsApp gibi uçtan uca şifreleme yapmaktadır. Signal, tüm güvenlik ihtiyaçlarımızı karşılar, uçtan uca şifreleme yapar, açık kaynaktır. Bulut tabanlı bir veri yönetim sistemi yoktur. Yani telefonunuzu değiştirecek olsanız yedeklerinizi telefondan almanız lazım. Telefondan başka yerde kopyası yoktur. Telefonunuz bozulsa veya çalınsa içindeki Signal verileri de kaybolacaktır.

## Uçtan Uca Şifreleme Protokolü Nasıl Çalışır?
Uçtan uca şifrelendiğinde mesajlarınızın, fotoğraflarınızın, videolarınızın, sesli mesajlarınızın, belgelerinizin, durum güncellemelerinizin ve aramalarınızın yanlış kişilerin eline geçmesi engellenmiş olur. Uçtan uca şifreleme özelliği, gönderilen içeriklerin yalnızca siz ve iletişim kurduğunuz kişi tarafından okunabilmesini veya dinlenebilmesini sağlar. Bunun nedeni, uçtan uca şifreleme özelliğinin mesajlarınızı bir kilitle güvence altına alması ve kilidi açıp mesajları okuyabilmek için gereken özel anahtarın yalnızca mesajlarınızın alıcısında ve sizde bulunmasıdır.

Örnekler ile verecek olursak;

### 1-) Açık Veri Transferi
Bu en az güvenli olan seçenektir. Örneğin, SMS ile gönderilen veriler şifrelenmez, yani teoride herhangi biri onu yakalayabilir. Neyse ki, pratikte bunu yapmak özel ekipman gerektirir ve bu da kısa mesajlarınızı gören kişileri bir şekilde kısıtlamış olur.

### 2-) Aktarım Sırasında Şifreleme
Orta güvenlikte olan seçenektir. iletilerin gönderen tarafında şifrelendiği ve sunucuya iletildiği, şifrenin sunucuda çözülerek yeniden şifrelendiği ve ardından alıcıya teslim edildiği, en sonunda alıcıda şifrenin çözüldüğü aktarım sırasında şifrelemedir. Daha basitçe; mesajlarınız hedefe gitmeden önce aracı sunuculara gönderilir. Bunun için Açık Veri olarak yada Gönderici-Aracı arasında şifrelenmiş olarak iletilir. Aracı sunucu mesajınızı Aracı-Alıcı arasında tekrar şifreler gönderir fakat Aracı Sunucu verilerinize açık olarak erişebilmektedir. Aktarım sırasında şifreleme, iletim sırasında bilgileri korur ancak bu şifrelemeyi kullanmak, iletim zincirindeki ara bağlantının — sunucunun — içeriği görmesine izin verir. Sunucunun ne kadar güvenilir olduğuna bağlı olarak, bu bir sorun olabilir.

### 3-) Uçtan Uca Şifreleme
Şu anda en yüksek güvenlik olan seçenektir. Temelde, iletişime geçen iki kişi arasında üretilen gizli ve açık anahtarlar vasıtasıyla, iki taraf arasında ki iletişimin (mesaj, dosya, her türlü veri) gizli anahtar ile şifrelenerek direkt birbirine iletilmesi, karşılıklı çözülerek okunmasıdır. Uçtan uca şifrelemenin temel avantajı, iletilen verileri alıcı dışındaki herkesten kısıtlamasıdır. Tıpkı bir mektubu postaladığınız kullanılan posta kutusu gibi düşünün. Bu posta kutusunu alıcı hariç hiç kimse balyoz veya testere kullanmadan açamaz. Uçtan uca şifreleme, iletişiminizin gizliliğini sağlar.

Açılması imkânsız bir kutu yaratmak fiziksel dünyada gerçekten mümkün değil, ama bilgi dünyasında bu mümkün. Uzman matematikçiler sürekli olarak yeni şifreleme sistemleri geliştiriyor ve eski sistemlerin gücünü artırıyor. Tabii bazı uzmanlarda bu sistemi çözmek için çaba sarf ediyor ve bu çaba aslında esas sistemin daha güçlenmesini sağlıyor.

## Uçtan uca şifrelemenin korumadığı şey nedir?
Buraya kadar Uçtan uca şifrelemeyi pohpohlamış olsak ta, Uçtan uca şifrelemenin de sınırlamaları vardır. Uçtan uca şifrelemenin kullanılması mesajınızın içeriğini gizlemenize izin verse de belirli bir kişiye mesaj gönderdiğiniz (veya ondan bir mesaj aldığınız) anlaşılacaktır. Sunucu, mesajların içeriğini okuyamaz ancak belirli bir gün ve belirli bir saatte mesajlaştığınızı bilir. Kötü niyetli birisi iletişim kurmak için kullandığınız cihaza erişim sağlarsa sizin adınıza mesaj yazıp göndermenin yanı sıra tüm mesajlarınızı da okuyabilir. Bu nedenle, uçtan uca şifrelemenin korunması, cihazlara ve uygulamaya erişimin de sadece bir PIN kodu ile bile olsa korunmasını gerektirir. Ve esas en önemli nokta; tüm cihazlarınızı korumaya özen gösterseniz ve içindeki mesajlara kimsenin erişemeyeceğinden emin olsanız bile, konuştuğunuz kişinin cihazından emin olamazsınız.

Unutmayın, en güvenli elektronik cihaz, fişi çekilmiş olandır 🙂

Peki ben ne mi kullanıyorum, ben wire kullanıyorum. Tüm iletişim için uçtan uca şifreleme özelliğini kullanıyor, bazı ufak eklentiler eğlenceli özellikler sunuyor. Whatsapp kadar popüler değil ama bir gün neden olmasın.