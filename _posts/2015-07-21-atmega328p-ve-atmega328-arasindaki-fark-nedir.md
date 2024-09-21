---
layout: post
title: "Atmega328P ve Atmega328 Arasındaki Fark Nedir?"
date: 2015-07-21 12:38
categories: ["Yazılım", "Embedded System"]
tags: ["arduino", "atmega", "atmega328p", "atmega328"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/atmega328-66eea92b8ea03.webp"
  alt: "Arduino"
---

Bu güne kadar Atmega entegrelerinin ilginç isimlendirmelerine fazla kafa yormamıştım. Dün Atmega328 satın almak için web'de gezinirken Atmega328P ve Atmega328 arasındaki farklılıkları anlayarak satın alma kararı verme gereği duydum (ne aldığımı en ince ayrıntısına kadar bilmeyi severim). 

İlk gözüme çarpan fiyat farkı oldu. Net farkı anlamak için www.atmel.com'a girip karşılaştırma yaptım. Atmel 1000 adet fiyatını vermiş, o yüzden ucuz gibi görünebilir, normalde bu fiyatlara bulmak pek mümkün değil. ATMEGA328-PU 1.69$ iken ATMEGA328P-PU 1.85$. Türkiye şartlarında satın alırken bu fiyatlar 9 TL ve 12 TL gibi oluyor, yani 1$ civarı fark var. 

Peki bu fiyat farkı neden var? Umarım bu bilgiler faydalı olur.

## Atmega328P ve Atmega328 Arasındaki Fark Nedir?
- Atmega328 ve Atmega328P aynı mimari yapıya sahip olduğu için 328'i 328P yerinde ya da tam tersi şekilde kullanabilirsiniz.
- Atmega328P Atmega328'e göre daha az enerji tüketiyor. Datasheet'leri incelediğimde 328P'nin 60nm işlemle üretildiğini gördüm, 328 ise 90nm ile üretiliyor. Yani bir kaç mikroamper daha az güç tüketimi için biraz daha fazla ödememiz gerekiyor.
- 328P ile 328'in çip imzaları farklı. Arduino IDE'deki avrdude programı gibi bir program iki çip arasındaki farkı anlayabilir, eğer bu çiplerden birini zorunlu kılan bir durum varsa diğerini taktığınızda hata ile karşılaşabilirsiniz. Yani çip imzasına dikkat!
- 2 numaralı konuda ifade ettiğim gibi üretim sürecindeki hassasiyetten kaynaklanan bir durum daha var; QFN paketleme. QFN paketleme üretim hassasiyetten dolayı sadece 328P'de var. Daha küçük çip için daha iyi üretim gerekiyor.
- Bir kaç mikroamper önemli değil, 1$ 1$'dır diyosanız Atmega328 sizin için ideal. Güç tüketimi önemli ve ya SMD kullanacaksanız Atmega328P size göre. Tabi çip imzalarına da dikkat edin :)

Yazar: [Emre Konca](https://www.youtube.com/@EmreKonca)
    