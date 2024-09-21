---
layout: post
title: "Yeni Blog, Yeni Sorunlar"
date: 2024-09-04 21:09
categories: ["Gunluk"]
tags: ["gunluk", "blog", "jekyll", "github", "ruby", "cms", "blog", "markdown", "liquid", "jekyll-build-issues", "jekyll-performance"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/badursun-jekyll-blog-deploy-problem.jpg"
  alt: "Yeni Blog, Yeni Sorunlar"
---

Jekyll, basit ve hızlı bir statik site oluşturucu olsa da içerik miktarı arttıkça build süreleri de hızla uzayabiliyor. Başlangıçta sadece 4-5 saniye olan build sürem, 40-45 yazı sonrasında 160 saniyeye kadar çıktı. Bu durumla karşılaştıktan sonra bazı araştırmalar yaptım ve build sürelerini etkileyen birkaç temel neden tespit ettim. Ancak asıl problemi **assets** klasöründe depoladığım **statik dosyalar (özellikle imajlar)** oluşturuyordu.

## Statik Dosyaların Build Süresine Etkisi
Jekyll, build işlemi sırasında her sayfayı ve her statik dosyayı kontrol eder. Özellikle imajlar ve diğer medya dosyaları build süresini uzatır çünkü her bir dosya içerikte kontrol edilir ve yeniden kopyalanır. Bu işlem, içerik miktarı arttıkça build süresini ciddi şekilde etkiler. Başlarda fark etmediğim bu durum, zamanla build süremi önemli ölçüde uzatmaya başladı.

Statik dosyaların kontrol edilmesi, bu sürenin artmasındaki en önemli etkendi. Özellikle her build'de imajların işlenmesi ve kopyalanması, build süresini katladı.

Build süresini optimize etmek için **statik dosyaları** (özellikle imajlar) ayrı bir repo'da depolama fikrini denemeye karar verdim. Bu sayede, Jekyll'in her build sırasında bu dosyaları tekrar tekrar işlememesi sağlandı. Özetle izlediğim adımlar şöyle:

## İzlediğim Yol
1. **İkinci bir repo oluşturma**: Tüm statik içerikleri, özellikle imajları bu ikinci repoda depolamaya başladım.
2. **Raw dosya yollarını kullanma**: Jekyll içeriklerinde, bu ikinci repodaki dosyalara ulaşmak için doğrudan raw URL'leri kullandım. Bu, imajların ve diğer dosyaların Jekyll tarafından işlenmesini tamamen devre dışı bıraktı.
3. **Yeniden yönlendirme**: Tüm blog yazılarımda, statik dosyalara giden yolları güncelledim ve bu dosyaların yeni depolama alanındaki raw URL'lerine yönlendirdim.

Bu değişiklikten sonra build sürelerinde ciddi bir iyileşme fark ettim. Şu anda yaklaşık 14-16 saniye civarında bir build süresine sahibim, bu da oldukça kabul edilebilir bir seviye. Yani başta 4-8 saniyelerde başlayıp sonra 160-180 saniyelere çıktıktan sonra, şimdi tekrar 14-16 saniyelere inmiş durumda.

Eğer benzer bir problemle karşılaşıyorsanız ve benim çözümüm tam olarak sizin için uygun değilse, işte birkaç alternatif çözüm:

- Jekyll Cache Eklentisi: Jekyll’in önbellekleme eklentilerini kullanarak build süresini azaltabilirsiniz.
- Daha Hafif Tema Kullanımı: Bazı temalar build süresini uzatabilir. Daha hafif ve optimize edilmiş bir tema tercih edebilirsiniz.
- Büyük Medya Dosyalarını Dışarı Taşımak: Medya dosyalarını, CDN ya da dış servislerde tutmak build süresini önemli ölçüde azaltabilir. 

Jekyll projelerinde build süresinin zamanla uzaması, özellikle statik dosyalarla çalışanlar için büyük bir problem olabilir. Ancak, dosyaların doğru şekilde yönetilmesi ve içerik ile statik dosyaların ayrıştırılması bu sorunu çözmenin etkili yollarından biri. Benim kullandığım yöntemle build süresinde ciddi bir düşüş sağladım. Bu sayede hem hızlı build sürelerine geri döndüm hem de projeyi daha verimli hale getirdim.

Eğer siz de benzer bir sorun yaşıyorsanız, bu yöntemi deneyebilir ya da yukarıdaki alternatif çözümlerden birini kullanabilirsiniz. Build sürenizi optimize etmek, büyük projelerde zaman kazanmanın önemli bir adımıdır.

> Jekyll Build Time Performance Issues