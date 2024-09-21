---
layout: post
title: "Reprap Prusa I3 Rework Tanıyalım"
date: 2015-06-24 11:44
categories: ["Teknoloji", "3D-Printer"]
tags: ["3d-printer", "arduino", "arduino-mega", "reprap", "diy", "embedded-system", "adrian bowyer"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/reprap-prusa-i3.jpg"
  alt: "Reprap Prusa I3 Rework Tanıyalım"
---

## RepRap Nedir ?
Adrian Bowyer isimli İngiliz öğretim görevlisi ve çevresindeki öğrenciler tarafından 2005-2006 yılları arasında başlatılmış açık kaynak donanım hareketidir. 

Adrian Bowyer’in çalıştığı alan olan biomemetik(Doğadan benzeşim yolu ile teknoloji aktarımı) ile Arı – Bal – Çiçek ilişkisinden yola çıkılarak oluşturulmuştur. RepRap 3B yazıcı ile üretiğimiz ürünler bizim için bal son faydadır. Biz bu bal için yeni bir RepRap yaptığımızda bize bal veren çiçeğin üremesini sağlamış oluruz. Bu düşünce yapısı ile üretilen her 3 boyutlu yazıcı bir RepRap değildir diyebiliriz. RepRap’ların en temel özelliği olabildiğince kendilerini kopyalayabilir olmalarıdır. RepRap kelimesi ingilizce “Self Replicating Rapid Prototyping” tanımının kısaltmasıdır. Kabaca Türkçeye çevirdiğimizde bu tanım “Kendini Kopyalayabilen Hızlı Prototipleyici” olacaktır.

## 3D Yazıcı Nedir?
3D Printer, 3 boyutlu bilgisayar datasını katı, elinizle tutabileceğiniz gerçek nesnelere dönüştüren bir makinedir. Bu teknoloji geleneksel imalat yöntemleri gerçekleştirilmesi mümkün olmayan geometrileri üretebilmektedir.

3D yazıcı teknolojisi aslında yeni bir teknoloji değil ilk uygulaması 1984′e dayanıyor ancak geçtiğimiz 20 yılda bu yöntem hızlı prototipleme alanının dışında çok fazla ilgi görmedi. 2006 da başlayan Reprap projesi ile çok daha geniş kitlelere ulaştı. Bu proje sayesinde bir çok sıradan kullanıcı, hobi severler, kendin yap kültürüne sahip kişiler bir 3d yazıcıya sahip oldu. Hatta projenin başlangıcından 3 yıl sonra bir çok şirket açık kaynak 3d yazıcı üretmek ve satmak için Reprap projesinin getirdiklerinden faydalanarak teknolojiyi çok daha geniş bir kullanıcı kitlesine yaymayı başardı.

## Neler Yapılabilir ?
3D Printer teknolojiler ile bir çok farklı malzeme ve yöntem kombinasyonları kullanılarak oldukça geniş bir alana hitap edebilmektedir. Metal tozları ve SLS yöntemi kullanarak uçak parçaları, kişiye özel implantlar üretilebilir. Öte yandan, FDM ve plastik malzemeler kullanılarak döküm modelleri prototipler, plastik oyuncaklar ve diğer bir çok plastik obje üretimi gerçekleştirilebilir.

## Reprap Prusa I3 Rework Tanıyalım
Genel hatları ile kavradıysak şimdi tam olarak Reprap Prusa I3 Rework Tanıyalım; Prusa I3, RepRap çekirdek geliştiriclerinin en son modelidir. Daha önce geliştirilen Prusa modellerinden öğrenilen iyileştirmeler, düzenlemeler ile birlikte karşılaşılan hataların geliştirilmiş versiyonudur. Günümüzde en çok bu versiyon tercih edilmektedir. Bende bu versiyonu yaptığım için ve üzerinde uzun süredir çalıştığım için en detaylı anlatım yapabileceğim yazıcı olduğunu düşünüyorum.

Yazı dizisinin bu ilk bölümünde, başlangıç seviyesinde ihtiyaç duyacağınız tüm bilgileri aktardığıma inanıyorum. Sormak istediklerinizi buradan sorabilirsiniz. Bir sonra ki yazımda parça listesi ve parça temini, fiyatları hakkında bilgi vereceğim.

Orjinal Josef Prusa web sayfasını ziyaret etmek için [tıklayın](https://www.prusa3d.com/)