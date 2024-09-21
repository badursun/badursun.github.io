---
layout: post
title: "CPLD Ekran (CPLD Screen) Nedir?"
date: 2015-06-19 12:22
categories: ["Yazılım", "Embedded System"]
tags: ["arduino", "atmega", "arduino-mega", "arduino-adk", "cpld-screen", "cpld", "tft", "lcd"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/cpld-screen.jpg"
  alt: "CPLD Ekran (CPLD Screen) Nedir?"
---

Arduino ile birlikte kullanmak istediğimiz ekranların´da çeşitli özellikleri vardır. Bunlardan bir tanesi de CPLD ekran / CPLD Screen modelleridir.

## CPLD Ekran (CPLD Screen) Nedir?
CPLD; (Complex Programmable Logic Device) yani "Karmaşık Programlanabilir Lojik Aygıt" 'ın kısaltılmasıdır. CPLD destekli ekranlar içinde bir de SDRAM bulunur, bu bildiğiniz RAM´ler gibi bir hafızadır. CPLD dili ileri düzey programlama dilleri oldukları için ve yeterli kaynak bulunmadığı için ülkemizde çok az kullanıcısı vardır.

Gelelim CPLD destekli ekranlara; buradan ve buradan iki farklı boyutta CPLD destekli ekran görebilirsiniz.

Bu ekranların en büyük özelliği, hızlı olmalarıdır. Standart TFT-LCD ekranlarda, Arduino ve benzeri platformlar ile ekrana bir yazı yada çizim yaptırmak istediğinizde paging effect (sayfalama efekti) denilen etki görülür. 

Bunun sebebi, ekranlara verinin piksel piksel yazılmasıdır. Ve bu kullandığınız işlemcinin hızına bağlı olarak daha hızlı yada daha yavaş olabilir. Örnek verecek olursak, Arduino MEGA 16MHz lik bir hıza sahiptir ve Arduino DUE ise 84 MHz lik bir hıza sahiptir. Yani DUE kart, MEGA´ya göre yaklaşık 5 kat daha hızlı ekranı doldurabilir.

Bu noktada işte imdadımıza CPLD ekranlar geliyor. CPLD ekranlarda, ekrana vermek istediğiniz verileri, ilk açılışta sayfa olarak yüklüyoruz. CPLD ekranlarda genelde 8 sayfa vardır. Ve bu sayfalara komutlar ile veriler yüklenir. Daha sonra sayfa değiştirilirek görüntü ekrana hızlı getirilir.

<p style="text-align: center;"><iframe src="https://www.youtube.com/embed/B8Vax30hkoo?rel=0&amp;showinfo=0" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe></p>


Örnek bir kod vermek gerekirse; kendi projemde kullandığım sayfaya yazdırma ve sayfayı görüntüleme kodu şu şekildedir.

```javascript
myGLCD.setWritePage(1); // 1 Numaralı Ekrana Yaz
myFiles.load(0, 0, 800, 480, "EKRAN0.RAW"); // SD Karttan EKRAN0.RAW görselini yükle
delay(5); // Stabilite için 5ms bekletelim.
myGLCD.setDisplayPage(1); // 1 Numaralı Ekranı Göster
myGLCD.setWritePage(2); // 2 Numaralı Ekrana Yaz
myFiles.load(0, 0, 800, 480, "EKRAN1.RAW"); // SD Karttan EKRAN1.RAW görselini yükle
delay(5);
myGLCD.setWritePage(3); // 3 Numaralı Ekrana Yaz
myFiles.load(0, 0, 800, 480, "EKRAN2.RAW"); // SD Karttan EKRAN2.RAW görselini yükle
delay(5);
myGLCD.setWritePage(4); // 4 Numaralı Ekrana Yaz
myFiles.load(0, 0, 800, 480, "EKRAN3.RAW"); // SD Karttan EKRAN3.RAW görselini yükle
delay(5);
myGLCD.setWritePage(5); // 5 Numaralı Ekrana Yaz
myFiles.load(0, 0, 800, 480, "EKRAN4.RAW"); // SD Karttan EKRAN4.RAW görselini yükle
delay(5);
```

### Gerekli kütüphaneler
```
#include <SdFat.h>
#include <UTFT.h>
#include <UTouch.h>
#include <UTFT_SdRaw.h>
```

Benim projemde ki örnek videolar ile kıyaslamayı görmeniz için iki adet link veriyorum:

<strong>CPLD Olmadan:</strong> ~~[https://instagram.com/p/tS4S3MPdNr/](https://instagram.com/p/tS4S3MPdNr/)~~

<strong>CPLD İle:</strong> ~~[https://instagram.com/p/3Tk5B9vdLQ/](https://instagram.com/p/3Tk5B9vdLQ/)~~