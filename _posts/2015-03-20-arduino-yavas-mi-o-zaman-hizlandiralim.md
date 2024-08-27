---
layout: post
title: "Arduino yavaş mı? O zaman hızlandıralım"
date: 2015-03-20 10:25
categories: ["Yazılım", "Embedded System"]
tags: ["arduino", "atmega", "true-basic"]
toc: true
---

Arduino ile ilgili ilk yazım biraz spesifik bir konuda olacak. İlk yazımı böyle bir konuda seçmemin sebebi ise, biriktirdiðim ve deneyimlediðim bir çok konu olmasına raðmen, yazmak için doðru platformu bulmakta zorlanmış olmam. Öyle ki, Arduino Merkezi adlı bir projem vardı fakat iş güç ve hayat yorgunluðundan ötürü bu projeyi hayata geçiremedim. Bir çok alanı neredeyse tamamlandı ama hiç bir zaman "açayım artık" diyemediðim kadar yetersiz.

Neyse, uzatmadan konuya girelim :) Arduino'nun hızı bazen size yetersiz gelebilir. Tabii bu durum biraz daha extreme kullanıcılar için geçerli. Çaylak kullanıcılar bu tür hack denemeleri yapmamalıdır. Sonra elinizde ki arduino'dan da olursunuz :)

Eðer arduino'nuzu yavaş buluyorsanız, bunun sebebi arduino kodlarken kullandıðınız yazılım dilinden kaynaklıdır. Arduino için C benzeri basic kullanıyoruz. Fakat Arduino'nun beyni, yani atmel çipler true c denilen bir dil kullanıyor. Dolayısıyla bir çok komut ile biz çip'e bir şeyler anlatmak istiyoruz, fakat çip'e bunu yalın anlatamıyoruz.

Örnek vereceğim kod, hepimizin bol bol kullandığı digitalWrite komutu.

digitalWrite komutu ile bir pin'i HIGH yada LOW yaparız. Bu komut, çip'e ilgili pin'e voltaj gönderip göndermemesini söyler. Fakat biz bunu ingilizce söylediğimiz düşünelim. Fakat atmel ingilizce bilen bir ispanyol olsun :) Ona ne yapması gerektiğini ispanyolca mı söylersek daha hızlı yapar, yoksa ingilizce mi? Tabiki de ispanyolca, yani kendi anladığı dilde.

## Atmega 168 Pin Yapısı
![Atmega 168 pin yapısı](assets/img/0d6482d867b07eec4d0d6c209f922418.jpg)

Gördüğünüz gibi çiplerde bazı farklı terimler var. Bunlar, normalde arduino üzerinde kullanamadığınız özellikleri de listeler.

Tabii arduino da tanımlı pinlerin adları da burada daha farklıdır. Örneğin; D13 pini çip üzerinde esasen PB5 olarak port edilir.

## Arduino Dili ve True C
Önce bildiğimiz arduino dili ile yazalım

```javascript
digitalWrite(13,LOW);
digitalWrite(13,HIGH);
```

Şimdi de true c dili ile yazalım.

```javascript
PORTB |= _BV(PB5);
PORTB &amp;= ~_BV(PB5);
```

Bu iki komutta 13 numaralı pin'i HIGH ve LOW yapıyor. Peki hangisi daha hızlı?

Bunu aşağıda ki komut ile test edelim

```javascript

void setup(){
  Serial.begin(9600);
}

void loop(){
  int initial = 0;
  int final = 0;
  initial = micros();
  for (int i = 0; i &lt; 500; i++){
    digitalWrite(13, HIGH);
    digitalWrite(13, LOW);
  }
  final = micros();

  Serial.print("Time for digitalWrite(): ");
  Serial.print(final - initial);
  Serial.println("");

  initial = micros();

  for (int i = 0; i &lt; 500; i++){
    PORTB |= _BV(PB5);
    PORTB &amp;= ~_BV(PB5);
  }

  final = micros();
  Serial.print("Time for true c command: ");
  Serial.print(final - initial);
  while (1);
}
```

Seri ekranı açalım, ve sonucu görelim:

```log
Time for digitalWrite(): 3804
Time for true c command: 348
```

Sonuçlar milisaniye cinsindedir. Gördüğünüz gibi true c olarak girilen kod, basic'e göre 10 kat daha hızlı işlenmektedir.

[Kaynak instructables](https://www.instructables.com/id/Arduino-is-Slow-and-how-to-fix-it/?ALLSTEPS)

Daha detaylı bilgi edinmek isterseniz aþağıda ki linklere göz gezdirebilirsiniz.

https://www.avrfreaks.net/index.php?name=PNphpBB2&amp;file=viewtopic&amp;t=37871
https://www.arduino.cc/cgi-bin/yabb2/YaBB.pl?num=1230286016