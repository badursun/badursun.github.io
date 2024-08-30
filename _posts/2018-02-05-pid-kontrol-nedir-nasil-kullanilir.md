---
layout: post
title: "PID Kontrol Nedir, Nasıl Kullanılır?"
date: 2018-02-05 12:22
categories: ["Yazılım", "Embedded System"]
tags: ["pid", "proportional", "integral", "derivative"]
toc: true
---

PID Kontrol Nedir oldukça karmaşık, başlangıçta anlaşılması zor bir sistemdir. Özellikle ne üzerinde PID yapacağınız çok önemlidir. Sıvı, Hava, Motor gibi kontrollerde PID çeşiti, kat sayı seçeneklerini iyi analiz etmek gerekir. PID Kontrol Nedir, Nasıl Kullanılır, nasıl tanımlarım diye düşünürken internette bulduğum bir yazıyı size sunmak istedim. Yazan İskender adlı arkadaşın emeğine sağlık, oldukça başarılı bir anlatım yaparak P, I ve D nin ne olduğunu tanımlamış.

PID (proportional-integral-derivative) kontrol sistemdeki hatayı; hatanın miktarı, hatadaki değişim hızı ve yapılan toplam hatayı hesaplayarak giderme yöntemidir. Bu değerlerin etkisini kontrol etmek için katsayılar kullanılır.

P katsayısı sistemin hataya karşı vereceği tepkiyi belirler. Büyük olursa hatayı hızlı kapatır ama denge noktasını geçer. Bu etkiyi frenlemek için hatadaki değişim hesaplanır. Hatadaki değişimin etkisi de D katsayısı belirler. Toplam hata, sistemin geçmişte yaptığı hatayı telafi etmek için hesaplanır ve etkisi I katsayısıyla belirlenir. Basit bir örnekle açıklarsak;

PID kontrolle bir aracın A noktasından B noktasına ortalama 90km/s hızla gitmesini sağlayalım. Araç hareketsizken ilk kontrolde P "hızmız 0km/s 90km/s hıza çıkmamız lazım hata 90km/s , gazla" der. İkinci kontrolde aracın hızı 50km/s olsun P "hata 40km/s gazlamaya devam" der, D "hatayı çok hızlı kapatıyorsun böyle gazlamaya devam edersen hedef hızı geçeceksin" der ve gaz biraz kapatılır. Araç 90km/s hıza ulaşana kadar bu atışma devam eder. D, P nin tepkisini yumuşatıcı etki yaptığı için araç 90km/s hıza ulaştığında ivme sıfırlanmış olacaktır ki, I atılır "ortalama hız 90km/s olacak 0 dan 90 a çıkana kadar zaman kaybettik, bu zamanı telafi etmemiz lazım hızlanmaya devam" der ve araç hızlanmaya devam eder. Kaybettikleri zamanı telafi ettikten sonra aracın hızını 90km/s e düşürürler ve mutlu mesut B noktasına ulaşırlar.

P, I ve D aynı anda bir sistemde kullanılmak zorunda değildir. Mesela aracımızın sadece 90km/s sabit hızla gitmesi istenseydi I kullanmamıza gerek kalmazdı.

[Kaynak](https://www.arduinobeyinleri.com/pid-nedir-nasil-kullanilir/)