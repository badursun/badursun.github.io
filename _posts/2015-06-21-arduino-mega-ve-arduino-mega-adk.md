---
layout: post
title: "Arduino Mega ve Arduino Mega ADK"
date: 2015-06-21 18:53
categories: ["Yazılım", "Embedded System"]
tags: ["arduino", "atmega", "arduino-mega", "arduino-adk"]
toc: true
---

Arduino MEGA ve MEGA ADK arasında aslında görsel olarak çok büyük bir fark yok. Fakat ikisi arasında çok ince bir nokta var; bu da, MEGA ADK´nın Android geliştirme ortamı için yapılmış olması. Bu noktada en büyük fark USB HOST özellikleri olarak geliyor.

Aşağıda iki ürünü kıyaslamanız için özellikleri bir araya getirdim. Eğer tek amacınız daha çok pin, daha stabil bir kart ise; MEGA sizin için yeterli olacaktır. Fakat üst düzey bir geliştirme yapacaksanız ve işin içine Android ortamı da girecekse o zaman sizin için doğru olan kart Arduino MEGA ADK dır.

## Arduino Mega ADK
Arduino Mega ADK, ATmega 2560 mikrodenetleyiciye sahip Arduino ailesinin bir ürünüdür. Arduino Mega 2560’tan farklı olarak Android işletim sistemine sahip akıllı telefonlar ile rahatlıkla haberleşmeyi sağlayan USB host arayüzüne sahiptir. Bütün "Android's Accessory Development Kit” örnekleriyle uyumludur.

**Arduino Mega ADK üzerinde;**

- 14’ü PWM çıkış özelliğine sahip 54 adet dijital giriş/çıkış (I/O)
- 16 adet analog I/O
- 4 adet donanımsal UART arayüzüne sahip I/O pinleri
- 16MHz kristal osilatör
- USB bağlantı konnektörü
- Güç girişi
- ICSP programlama header

Reset butonu mevcuttur.

## Arduino Mega
Arduino Mega 2560, basit bir I/O kartı ve İşleme/Kablolama dilini uygulayan geliştirme çevresi temelleri üzerine kurulu açık kaynaklı fiziksel hesaplama platformudur. Arduino tek başına interaktif nesnelerin geliştirilmesi için kullanılabileceği gibi bilgisayarınız üzerindeki yazılımlara da bağlanabilir (örn. Flaş, İşleme, MaxMPS). Açık kaynak IDE (ardufun.com), ücretsiz olarak indirilebilir (mevcut olarak Mac OS X, Windows, ve Linux desteklenmektedir).

Arduino Mega Atmega2560 tabanlı bir mikroişlemci kartıdır. Üzerinde 54 dijital giriş / çıkış pini (bunlardan 14'ü PWM çıkışları olarak kullanılabilir), 16 analog girişten, 4 UART’tan (donanımsal seri portları), bir 16 MHz kristal osilatör, bir USB bağlantısı, bir güç girişi, bir ICSP bağlantısı ve bir reset butonundan oluşmaktadır. Bir mikroişlemciyi kontrol etmek için her şeye ihtiyaç duymaktadır; yalnızca bir USB kablosuyla beraber bilgisayara bağlayınız ya da başlamak için bir AC’den DC’ye çevirici adaptör ile ya da batarya ile çalıştırın. Mega, Arduino UNO için tasarlanmış pek çok shield ile uyumludur.

Bu Arduino ailesinin son versiyonu Arduino Mega 2560'in bir önceki versiyonundan farkı FTDI çipi yerine ATmega8U2 çipini kullanması. Bu çip daha hızlı transfer geçişine ve Linux ve Mac işletim sistemlerinde sürücüye ihtiyaç tanımadan direk tanımasını sağlayacaktır(Windows için .inf dosyası).

**Arduino Mega Özellikleri:**

- ATmega2560 mikrodenetleyici
- Giriş voltajı -> 7-12V
- 54 Dijital I / O Pini (14 pini PWM çıkışı olarak kulanılabilir)
- 16 Analog Girişleri
- 256k Flaş Bellek
- 16Mhz Hızı    