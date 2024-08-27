---
layout: post
title: "Greylisting Nedir? Neden E-Posta Gitmez?"
date: 2010-05-25 22:47
categories: ["Yazılım", "DevOps"]
tags: ["mail-server", "mail-enable", "spf", "dkim", "dmarc", "email-setup", "email-server", "mta", "greylisting", "rfc-2821", "spam", "smtp", "gmaili", "qmail", "triplet", "rejected-mail"]
---

Greylisting, enteresan ve efektif bir spam ile başetme metodudur. Bu metod, gönderilen her bir mailin source ip’sini, gonderen e-mail adresini ve alıcı e-mail adresini kontrol ediyor (Bu üç bilgiye triplet deniyor). Eğer gönderilen bir maile ait bu üç bilgi daha önce rastlanılmamış bir triplet ise "geçici olarak servis dışı"ymış gibi davranarak ilgili maili reject ediyor ve "Daha sonra tekrar deneyiniz" şeklinde bir hata mesajı gönderiyor.

Server servis dışı olmadığı halde gönderilen maili geçici bir süre için reject etti. ışte greylist uygulamasının enteresan yönü bu.

Bu yalancı reject durumunun nedeni aslında çok basit; SMTP’nin standartların belirtildiği RFC 2821 uyarınca, bir mail geçici olarak reject edildiği zaman, gönderide bulunan MTA ya da maili gönderen uygulama bir müddet sonra aynı maili tekrar göndermek için teşebbüsde bulunur; bulunmuyorsa o mail spamdir.

Çünkü her aklı başında MTA ya da uygulama normal olarak RFC standartlarına göre yazılmıştır ve bu kurallar çerçevesinde hareket eder. Ancak spammerlerin, spam yapmak üzere kullandıkları yazılımlar genellikle herhangi bir standarda uymaksızın, "Maili gönder, gönderemiyorsan salla gitsin." prensibi ile çalışırlar. Zira atacak milyonlarca mail olduğundan dolayı yolda dökülenleri umursamazlar.

İşte bu kurnaz yöntemle spam konusunda epey bir başarı elde edebilirsiniz.

Ancak, güzel bir yöntem olmasına rağmen bazi kötü yönleri de bulunmaktadır. Mesela, bazı eski SMTP sunucularının ve uygulamalarının, SMTP ile ilgili olarak yenilenmiş RFC 2821 yerine artık eskimiş olan RFC 821 e uygun olarak yazılmalarından kaynaklı olarak, temporary rejection durumlarına, permanent rejection gibi davranmaları ve geçici olarak reject edilen mailler için ikinci bir gönderme denemesinde bulunmamalarıdır. 

Böyle bir durum söz konusu olduğu zaman, gönderdiği maillerin geri döndüğünden şikayet eden insanların sorunları, kendi smtp sunucularının eski olmasından kaynaklanmasından dolayı sizin tarafınızda yapacak pek de bir şey kalmıyor. Ancak elbet bu durum çok sık karşılaşılmamakla birlikte karşılaşılması durumunda insanların SMTP serverlarını güncellemeleri için iyi bir fırsat sunar.

Ayrıca, triplet’i daha önceden görülmemiş ama spam olmayan masum bir mail ilk denemede reject edilir, ikincide kabul edilir. Bu durum mailin ulaşma süresini biraz uzatıcı bir faktördür. Zira, mail serverların geçici olarak reject edilmiş mailler için yeniden deneme süreleri birbirinden farklı olmakla birlikte biraz uzundur. Bu nedenle mailleşmelerde bir miktar gecikme yaşanması olası bir durumdur.

Bu kullanışlı ve kurnaz yöntemle ilgili olarak işin detaylarını anlatan bir iki kaynak: [http://projects.puremagic.com/greylisting/](http://projects.puremagic.com/greylisting/), [http://en.wikipedia.org/wiki/Greylisting](http://en.wikipedia.org/wiki/Greylisting)

Ayrıca, spamdyke qmail için greylisting desteği de veriyor.