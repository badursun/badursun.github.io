---
layout: post
title: "Arduino ile PWM ve Frekans Kontrolü"
date: 2015-06-21 12:42
categories: ["Yazılım", "Embedded System"]
tags: ["arduino", "atmega", "arduino-pwm", "pwm-scale", "pwm-duty-cycle", "pwm"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/pwm-control-66eea9d8b9ede.webp"
  alt: "Arduino ile PWM ve Frekans Kontrolü"
---

Elektronik, sinyal işleme veya kare dalga dendiğinde genelde akıllara ilk olarak PWM (Pulse Width Modulation) tekniği gelir. Modülasyon işlemi gerçekleştiren bu tekniğin asıl amacı cihazlara verilen elektriğin gücünü kontrol altında tutmaktır.

## Pulse Width Modulation
Açılımı Pulse Width Modulation yani Sinyal Genişlik Modülasyonu olan bu teknik, sinyal işleme veya sinyal aktarma gibi daha çok elektronik devrelerin yanı sıra Arduino veya elektrik makineleri gibi özel uygulama alanlarında da yer alan bir tekniktir.

En basit haliyle bir sinyal modülasyon tekniği olarak tanımlanabilir. Sinyal bilgisinin aktarım için uygun hale çevirilmesi amacının yanı sıra güç kontrolü sağlamak ve elektrik makineleri, güneş pili şarj üniteleri gibi özel devrelere destek olmak amacı da taşır.

Bu kontrolde tamamen anahtarlama ile sağlanır. Anahtarlama ne kadar hızlı yapılırsa, PWM ile aktarılan sinyalin gücü o kadar da artar. Örneğin bir lambaya gönderilen sinyalde PWM tekniğine ihtiyaç duyuluyorsa, bu teknik 120 Hz frekans değerinde uygulandığında maksimum verim elde edilebilir.

### Duty Cycle
"Duty Cycle" yani görev döngüsü olarak tanımlanan bir kavram bulunuyor ve PWM tekniğinde de sıkça karşımıza çıkıyor. Görev döngüsü kavramı aslında yapılan işlemin periyodunu belirtiyor. Bu döngü düşük seviyelerde ise aktarılan güç düşük olurken, döngünün yüksek seviyelerinde yüksek güç aktarılıyor.

PWM ve Frekans kontrolü aslında bir çoğunuzun evinde gördüğü, kısılıp açılabilir lambalar gibidir. Işığın şiddetini ayarlamak için bir nevi potansiyometre kullanılır, burada ise anahtarlama (açma kapama) işlemi olarak uygulanır. Evinizde ki lambanın düğmesini saniyede 50 yada 100 kere açıp kapatmanız gibi. Bunu çok seri yaparsanız, lambanızın belirli bir kısıklıkta kalacağını görürsünüz (mesela tabii, bunu başarma şansınız yok:)

Arduino bünyesinde de PWM tekniği kullanılabiliyor. Arduino bünyesinde kullanılan PWM tekniği ile dijital sonuçlardan analog sonuçlar elde edilebiliyor. Bunun yanı sıra özellikle kontrol için ihtiyaç olan kare dalga üretimi de gerçekleşiyor.

### Kare dalga
Kare dalga, bilindiği gibi "on" ve "off" konumlarını sağlıyor.

Böylece kare dalga gönderildiğinde "on" konumunda 5V uygulanırken, "off" konumunda 0V uygulanmış oluyor. İşte bu "on" kısmının aktif olduğu genişliğe "Pulse Width" yani "Sinyal Genişliği" adı veriliyor. Bu doğrultuda istenilen sinyal genişliği elde etmek için de modülasyon tekniği uygulamanız gerekiyor ki bu da PWM'in temel mantığıdır.

Arduino ile yanıp sönen bir LED devresi kurmak veya Arduino ile DC motor kontrolü gerçekleştirmek için PWM tekniğini kullanmamız gerekiyor. Bu teknik de Arduino'ya gömülen yazılımda yatıyor. "analogWrite(...)" fonksiyonu ile görev döngüsünün miktarı belirleniyor ve kare dalga elde ediliyor.

## PWM ile Neler Yapılabilir ?

- Bir LED kısıp, ışını arttırabilirsiniz.
- Bir analog çıkış sağlayabilirsiniz; tabii dijital çıkışlar filtrelendiyse,
- %0 ile %100 arasında (0v - 5v) bir voltaj çıkışı sağlayabilirsiniz
- Ses sinyalleri üretebilirsiniz
- Motorlar için hız kontrolü sağlayabilirsiniz.

Modüle edilmiş sinyal üretebilirsiniz (Ifrared LED Kumandaları gibi)

### Örnek bir PWM Sinyal Üretim Kodu
```javascript
void setup(){
     pinMode(13, OUTPUT);
}

void loop(){
     digitalWrite(13, HIGH);
     delayMicroseconds(100); // Yaklaşık %10 döngü 1KHz de
     digitalWrite(13, LOW);
     delayMicroseconds(1000 - 100);
}
```

Daha fazla örnek ve detaylı anlatım için [https://www.arduino.cc/en/Tutorial/SecretsOfArduinoPWM](https://www.arduino.cc/en/Tutorial/SecretsOfArduinoPWM) adresini ziyaret edebilirsiniz.