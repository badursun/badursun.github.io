---
layout: post
title: "PID Kontrol Nedir, Nasıl Kullanılır?"
date: 2018-02-05 12:22
categories: ["Yazilim", "Embedded System"]
tags: ["pid", "proportional", "integral", "derivative"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/pid-control-66eea9d85e168.webp"
  alt: "PID Kontrol Nedir, Nasıl Kullanılır?"
---

PID (Proportional-Integral-Derivative) kontrol sistemi, oldukça karmaşık ve başlangıçta anlaşılması zor olabilen bir yapıya sahiptir. Özellikle PID kontrol sistemini ne üzerinde uygulayacağınız, sistemin başarısını belirleyen en önemli faktörlerden biridir. Sıvı, hava veya motor gibi farklı ortamlarda PID kontrol uygulanırken, kullanılan katsayıları iyi analiz etmek ve doğru ayarlamak gerekir. Bu yazıda, PID kontrolün ne olduğunu, nasıl çalıştığını ve nasıl tanımlanması gerektiğini daha iyi anlamanız için temel bilgilere yer vereceğim.

## PID Kontrol Nedir?
PID, sistemdeki hatayı; hatanın büyüklüğüne, hatadaki değişim hızına ve geçmişte yapılan toplam hataya dayanarak gideren bir kontrol yöntemidir. Bu üç faktörü ayrı ayrı kontrol etmek için üç farklı katsayı kullanılır: P (Proportional), I (Integral) ve D (Derivative). PID kontrolör, sistemin dengede kalmasını sağlamak için bu üç değeri sürekli hesaplar ve uygun müdahaleleri gerçekleştirir.

### P - Oransal Katsayı (Proportional)
P, sistemin hataya karşı anlık olarak vereceği tepkiyi belirler. Yani hata ne kadar büyükse, P katsayısı da o kadar büyük bir düzeltme yapar. Ancak P'nin aşırı büyük olması, sistemin hedefe hızla ulaşmasını sağlarken, denge noktasının aşılmasına (overshoot) neden olabilir. Bu, sistemin hedefe ulaşmaya çalışırken sürekli dalgalanmasına sebep olabilir.

### D - Türev Katsayısı (Derivative)
D katsayısı, hatanın değişim hızını ölçerek sistemin aşırı tepki vermesini engeller. Yani, P katsayısının hızlı bir şekilde hatayı azaltmasına karşı frenleyici bir etki yapar. Böylece, sistemin hedefe yaklaşırken yavaşlamasını ve dengeye oturmasını sağlar.

### I - Integral Katsayısı (Integral)
I katsayısı, geçmişte yapılan hataları dikkate alarak sistemi uzun vadede daha dengeli hale getirir. Eğer sistem, hedef değere ulaşana kadar bir süre gecikme yaşamışsa, I katsayısı bu gecikmeyi telafi etmek için sisteme ek düzeltmeler yapar. Özellikle uzun süreli küçük hataların birikmesini önlemek için kullanılır.

## PID Kontrolüne Örnek
Bir örnek üzerinden açıklayalım: Amacımız, bir aracın A noktasından B noktasına ortalama 90 km/s hızla gitmesini sağlamak olsun. Aracın hızı başlangıçta sıfır.

P katsayısı ilk devreye girdiğinde, "Hızımız 0 km/s, 90 km/s'ye çıkmamız lazım, hata 90 km/s, gaz ver!" der.
Aracın hızı 50 km/s'ye çıktığında, P bu sefer "Hata 40 km/s, gazlamaya devam et!" der. Ancak D katsayısı devreye girer ve "Hatayı çok hızlı kapatıyorsun, böyle devam edersen hedef hızı geçeceksin" diye uyarır. Bu durumda gaz kesilir ve hız daha kontrollü bir şekilde artırılır.
Araç 90 km/s'ye yaklaştığında, D katsayısı aracın hızlanmasını iyice azaltır, böylece hedef hıza ulaşırken dalgalanma (overshoot) yaşamaz.
Ancak araç hedefine tam ulaşınca, I katsayısı devreye girer: "Evet, 90 km/s hızdasın ama 0'dan 90'a çıkana kadar zaman kaybettik. Bu süreyi telafi etmek için biraz daha hızlanmalıyız." Araç bir süre hızlanmaya devam eder ve kaybedilen zamanı telafi ettikten sonra hızı sabitler.
Bu kontrol mekanizması sayesinde araç 90 km/s hızla sabitlenir ve B noktasına sorunsuz bir şekilde ulaşır.

## PID Kontrolünün Esnekliği
Her sistemde P, I ve D katsayılarının hepsinin birden kullanılması zorunlu değildir. Örneğin, yalnızca sabit bir hızda (90 km/s) gitmesi gereken bir araç için I katsayısına ihtiyaç olmayabilir. Sadece P ve D katsayıları kullanılarak sistem kontrol edilebilir. PID kontrol sistemi, farklı durumlara göre esnek bir şekilde uygulanabilir. Her bir katsayının ne zaman ve nasıl kullanılacağını iyi analiz etmek, sistemin verimli çalışması için kritiktir.

PID kontrol sistemi, hem mühendislik hem de teknoloji dünyasında yaygın olarak kullanılan güçlü bir yöntemdir. Hata düzeltme ve sistemin stabilizasyonu açısından son derece etkilidir. Ancak, her sistemin ihtiyaçları farklı olabileceğinden, PID katsayılarını doğru bir şekilde ayarlamak oldukça önemlidir. Sisteminize uygun P, I ve D katsayılarını belirleyerek kontrol süreçlerinizi daha verimli hale getirebilirsiniz.