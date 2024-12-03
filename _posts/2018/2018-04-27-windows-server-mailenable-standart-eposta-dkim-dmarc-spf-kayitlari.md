---
layout: post
title: "Windows Server - Mailenable Standart ve E-Posta DKIM, DMARC, SPF Kayıtları"
date: 2018-04-27 13:41
categories: ["Yazilim", "Windows Server"]
tags: ["windows", "mail-enable", "spf", "dkim", "dmarc", "email-setup", "email-server"]
toc: true
---

Mülkiyeti size ait olan domainlerden sadece sizin email gönderebileceğinizi ve bunun da gerçekten siz olduğunuzu belirlemenizi sağlayacak 3 ana teknoloji çözümü geliştirildi:

- DKIM – Sunucu tarafından şifreleme sistemi kullanılarak gönderilen her emailin barkodlanması
- SPF – Hangi sunucu IP adreslerinden sizin adınıza email gönderebileceğinin belirlenmesi
- DMARC – Üstteki iki güvenlik önlemini aldıktan sonra sizin adınıza email gönderilip gönderilmediğinin raporlanması

DKIM ve SPF konfigürasyonu, emaillerin ulaşmasını sağlamak için olmazsa olmazdır. “Bunları yaptıktan sonra ağız tadıyla 1 milyon email gönderirim” derseniz tabi yine yanlış olur çünkü gönderim yapacağınız bütün IP adresleri ve domainler kara listeye girecektir ve bir daha bu domainlerden ve IP’lerden bir daha gönderim yapamazsınız. Ama benim niyetim iyi, emaillerimin müşterilerime ve alıcılarına ulaşmasını sağlamak diyorsanız (DKIM ve SPF) + DMARC ile raporlama yapmak email ile ilgili bütün ipleri elinize almaya sağlayacaktır.

Elinizde bir windows sunucu, içinde yüklü MailEnable Standart sürümü varsa işte DKIM, SPF, DMARC kayıtları nasıl yapılır, nasıl kurulur, nasıl eklenir ve aktifleştirilir anlatmaya başlıyorum:

### MailEnable.MSC İçerisinden DKIM Oluşturmak
Sunucu içerisinde MailEnable arayüzünü açın. Sol bölümden MailEnable Management &gt; Messaging Manager &gt; Post Office &gt; AlanAdı &gt; Domains bölümüne girin ve sağ panelde açılan alan adınıza sağ tıklayın Properties menüsüne tıklayın. Açılan kutuda sağ bölümde DKIM opsiyonunu göreceksiniz. Burada Configration butonuna tıklayın ve açılan kutuyu doğru şekilde doldurun.

Ayarlarınızı yaptıktan sonra yukarıda ki tiki işaretliyoruz, selectors bölümüne selector bilgimizi ekliyoruz. Ben **adjans** olarak ekledim. Böylece imza içerisinde bu selector tanımlandı. Aynı şekilde mailenable imzalanan mailin içeriğine de bu selectoru işleyecek. Böylelikle mailimizi alan sunucu, o selector bilgisini DNS kaydını okumak için arayacak. Selectoru ekledikten sonra UPDATE butonuna basmayı unutmayın. Herşey tamam ise aşağıda üretilen DKIM kodunu alıyoruz ve sonra aşağıda anlattığım bölümde DNS e eklenecek veriler arasına ekliyoruz. Unutmayın DKIM için 2 adet TXT verisini DNS içeriiğine ekleyeceğiz. Birisi DKIM kaydı, diğeri DKIM selector kaydı.

### SPF Kaydı Oluşturmak
SPF (Sender Policy Framework) kaydı, Microsoft tarafından oluşturulmuştur ve spam mailleri kontrol etmek amacıyla kullanılan, alan adınız için hangi e-posta sunucuları üzerinden e-posta gönderme iznine sahip olduğunu tanımlayan DNS kayıtlarından biridir. TXT kaydı olarak DNS Kayıtları içerisine eklenmektedir. SPF Kaydının amacı, SPAM iletilerin engellenmesidir. Kısacası "Kimlik İbrazı" diyebiliriz.

SPF kaydı her zaman “v=” ile başlar. Bu, kullanılan SPF sürümünü gösterir. Şu anki sürüm spf1 olmalı. Çünkü spf1, şu an aktif olarak en yaygın kullanılan SPF türüdür. Sürüm göstergesinin ardında birden fazla terim vardır. Bu terimler hostların domain adına mail göndermesine izin verir ya da SPF kaydına ek bilgi sağlar. Terimler; mekanizmalar ve belirteçlerden oluşur. all, include, a, mx, ip4, ip6, exists gibi parametreleri içerir.

Mesela genel anlamda kullanabileceğiniz SPF Kaydı şöyledir;

> v=spf1 mx a ptr ip4:185.92.1.56/32 ip4:185.92.1.58/32 a:mail.alanadi.com ~all

Eğer hiç uğraşmak istemiyor veya gelişmiş ayarlar yapmak istiyorsanız şu adresi kullanabilirsiniz: https://www.dynu.com/NetworkTools/SPFGenerator

Bu kaydı da bir köşeye atıyoruz ve daha sonra aşağıda DNS Eklenecek Veriler bölümünde anlattığım gibi ekliyoruz.

### DMARC Kaydı Oluşturmak
Tam olarak açılımı "Domain-based Message Authentication, Reporting &amp; Conformance" anlamına gelecek olursak da "Domaininiz üzerinden gönderilen maillerin raporlanması" olarak çevirebiliriz, ne işe yarıyor kısmı ise en basit kısmı. Günümüzde tüm mail sağlayıcıları SPF ve DKIM ile mail göndereni doğrulamak adına çalışmalar yapıyorlar. Büyük mail sağlayıcıları kendilerine göre algoritmalar geliştirip mailin sizin gelen kutunuza mı, spam kutunuza mı gönderilmesi konusunda epey çaba sarfediyorlar bu çabanın karşılığını da size raporluyorlar. İşte Dmarc tam olarak bu işe yarıyor.

Yapmanız gereken işlem çok basit. Domaininize _dmarc.domain.com şeklinde bir TXT kaydı ekliyorsunuz ve bu TXT kaydında zorunlu sadece 2 alan bulunmakta. Bunlar ise v ve p değerleri. v özelliği kullanılan DMARC protokolünü p ise politikanız. P özelliği: none, quarantine, reject değerlerinden birini alabilir. Biraz daha kabul edilebilir bir DMARC kalıbı şöyle olmalıdır;

> v=DMARC1; p=none; sp=none; rua=mailto:abuse@alanadi.com; ruf=mailto:abuse@alanadi.com; rf=afrf; pct=100; ri=86400

rua, ruf parametrelerine var olan bir e-posta adresini (aynı domainde) ekleyip aşağıda anlatacağım üzere DNS bilgileirnize eklemeniz gerekiyor.

### DNS Eklenecek Veriler
Geldik son cilalama işlemlerini yapmaya. Buraya kadar tüm verileri oluşturduysanız işte DNS bilgilerinize ekleyeceğiniz kayıtlar aşağıda ki gibidir. Kayıt türü olarak TXT eklemeniz gerekiyor. Aksi belirtilmediği sürece ön alan adı eki kullanmıyoruz, boş bırakıyoruz.

- TXT Formatında Alan Adı <strong>[selector]._domainkey</strong> (Ör; <strong>adjans._domainkey</strong>.adjans.com.tr )
- TXT Kaydı &gt; MailEnabledan Alınan <strong>v=DKIM1;…..</strong> (MailEnable size bu kodu veriyor)
- TXT Formatında <strong>_domainkey</strong> (Alan adı bölümüne bu gelecek,<strong>_domainkey.</strong>adjans.com.tr )
- TXT Kaydı &gt; <strong>o=~</strong> (Kayıt olarak sadece bu veri eklenecek.)
- TXT Formatında
- TXT Kaydı &gt; v=spf1 mx a ptr ip4:185.92.1.56/32 ip4:185.92.1.58/32 a:mail.alanadi.com ~all
- TXT Formatında _DMARC.
- TXT Kaydı &gt; v=DMARC1; p=none; sp=none; rua=mailto:abuse@alanadi.com; ruf=mailto:abuse@alanadi.com; rf=afrf; pct=100; ri=86400

[selector] bölümüne istediğinizi yazabilirsiniz fakat önemli olan oluşturulan sertifika ve public keyde tanımlı olan selector verisidir. Yani DKIM kodunu oluştururken tanımladığınız selector, DNS kaydınızda tanımlayacağınız ile aynı olmalı. Bu eşleşme haricinde oraya ne yazdığınızın önemi yoktur.

DNS verilerinizi de eklediyseniz hemen bir test yapmak üzere şu adresi ziyaret edin, verilen e-posta adresine kurulum yaptığınız e-posta adresinde boş bir e-posta gönderin. Sonuç kötüyse endişelenmeyin, 48 saat içinde tekrar deneyin :)

https://www.mail-tester.com/