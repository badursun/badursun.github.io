---
layout: post
title: "Arduino ve Değişken Kullanım Performansları"
date: 2018-03-19 13:29
categories: ["Yazilim", "Embedded System"]
tags: ["arduino", "mcu", "arduino-mega", "true-c", "arduino-performance"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/arduino-66eea929e7c97.webp"
  alt: "Arduino"
---

Arduino, amatör kullanıcılar için oldukça verimli bir programlama platformudur. Ancak zamanla işin profesyonelleşmesiyle birlikte, ihtiyaçlar bazen tıkanma noktasına gelebilir. Çünkü öğrenme aşamasında, platformun gereksinimleri ne kadar karşıladığını göz ardı edebiliriz. Bazen daha hızlı olmasını isteriz, bazen de daha fazla bellek talep ederiz. Fakat, 16 MHz'lik bir işlemciye sahip Arduino Mega ile bile harikalar yaratabilirsiniz! Yeter ki değişken kullanımına ve kodlama yapısına hakim olun.

Arduino Mega 2560, ATmega2560 mikrodenetleyicisini içeren bir Arduino kartıdır ve Arduino Uno'dan sonra en çok tercih edilen kartlardan biridir. Bir üst model olan Arduino DUE ile karşılaştırıldığında platform farkı olduğu unutulmamalıdır. DUE, 32-bit ARM Cortex mikro işlemcisini kullanırken, Mega 2560 ise 16 MHz hızında bir işlemciye sahiptir. DUE, 84 MHz'lik işlemci hızıyla neredeyse 5 kat daha hızlıdır.

Zaman zaman, Arduino’yu nasıl daha performanslı kodlayacağınızı anlatan yazılar hazırlıyorum. Arduino IDE’siyle yapılan standart kodlamanın yanı sıra, makine diline daha yakın olan True C ile kodlamanın performans açısından ne kadar önemli olduğunu sıkça vurguladığımı biliyorsunuzdur.

Örneğin, `digitalWrite(13,LOW);` komutunu kullanmak yerine `PORTB |= _BV(PB5);` gibi komutlar kullanabilirsiniz.

İlk başta bu tarz komutlar anlamsız ve öğrenmesi zor gelebilir. Ancak ilk komutun yaptığı işlemi anladığınızda, yani bir portu açıp kapattığınızı fark ettiğinizde, bu yapı yavaş yavaş anlam kazanmaya başlayacaktır.

Aşağıda, 6 adet değişkeni standart olarak int ile tanımladığım bir kodu görebilirsiniz:

```javascript
int thermoCLK = 5;
int thermoCS = 6;
int thermoDO = 7;
int SSR_PIN = A5;
int PIN_SD_CS = 53;
int PIN_BUZZER = A4;
```

> Çalışmanız, programın 30560 baytını (%12) kullandı. Maksimum 253952 bayt. Global değişkenler, belleğin 3254 baytını (%39) kullanıyor. Yerel değişkenler için 4938 bayt yer kalıyor. Maksimum 8192 bayt kullanılabilir.

Aynı değişkenleri **#define** ile tanımladığımızda ise:

```javascript
#define thermoCLK 5
#define thermoCS 6
#define thermoDO 7
#define SSR_PIN A5
#define PIN_SD_CS 53
#define PIN_BUZZER A4
```

> Çalışmanız, programın 30502 baytını (%12) kullandı. Maksimum 253952 bayt. Global değişkenler, belleğin 3248 baytını (%39) kullanıyor. Yerel değişkenler için 4944 bayt yer kalıyor. Maksimum 8192 bayt kullanılabilir.

Gördüğünüz gibi, sadece #define kullanarak yerel değişkenlerde ve program alanında küçük bir optimizasyon sağladık. Belki 10 bayt tasarruf ufak görünebilir, ancak şu anda üzerinde çalıştığım programda birçok değişken, dizi (array) ve başka tanımlamalar var. Siz de programınız büyüdükçe bu tip optimizasyonlara ihtiyaç duyacaksınız. Artık, ihtiyaç duyduğunuz performans iyileştirmelerine nereden başlamanız gerektiğini biliyorsunuz.