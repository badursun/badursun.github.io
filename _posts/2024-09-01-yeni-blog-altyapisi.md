---
layout: post
title: "Yeni Blog Altyapısı"
date: 2024-09-01 13:13
categories: ["Gunluk"]
tags: ["gunluk", "blog", "jekyll", "github", "ruby", "cms", "blog", "markdown", "liquid"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/badursun-jekyll-blog-66eea92d1a7be.webp"
  alt: "Yeni Blog Altyapısı"
---

Jekyll, GitHub Pages ile tamamen uyumlu olan, statik site oluşturmak için kullanılan bir Ruby tabanlı jeneratördür. Dinamik içerik yönetim sistemlerinden (CMS) farklı olarak, Jekyll veri tabanı gerektirmez ve içerikleri düz HTML dosyalarına dönüştürerek sunar. Bu, özellikle hız, güvenlik ve ölçeklenebilirlik açısından büyük avantaj sağlar.

Kendi deneyimimle söyleyebilirim ki, Jekyll başlangıçta sadece kişisel bloglar için geliştirilmiş gibi görünse de, sunduğu esneklik sayesinde portfolyo sitelerinden dökümantasyon sayfalarına kadar her türlü projede kullanılabilir. Markdown ve Liquid kullanarak yazılarınızı kolayca yapılandırabiliyorsunuz. 

Üstelik, tema desteği sayesinde sitenizin görünümünü hızlıca değiştirebilirsiniz.

Jekyll'ın en güçlü yanlarından biri, otomatik sayfa oluşturma özelliği. _posts klasörüne yerleştirdiğiniz her .md dosyası otomatik olarak bir blog yazısına dönüşüyor. Yani herhangi bir dinamik CMS karmaşası olmadan, tamamen kontrol sizde! Kendi blogumda kullanırken fark ettiğim bir diğer detay, sürekli entegrasyon (CI/CD) süreçlerine çok kolay entegre olabilmesi. Özellikle GitHub Pages ile birleştiğinde, her push işleminde siteniz anında güncelleniyor. A-Z'ye herşeyi düşünülmüş.

Biliyorsunuz, "teriz kendi söküğünü dikemez" diye bir laf vardır, bir noktadan sonra yazılımcıların en büyük sorunu kendi işlerini/projelerini kalan zamanları içerisinde çözümlendirememek. Uzar da uzar.. Benim blog işide böyle oldu.

En son wordpress üzerinde kurduğum blog'u güncellemedim, içerik girmeye üşendim, sürekli not aldım, aldığım notlar birikti ama asla eritemedim. Sonunda bir gün farklı bir sistem mi kullanayım yoksa kendim mi geliştireyim diye düşünürken, Jekyll'a rastladım. Bir kaç mıncırma sonrasında Ta-Daaa..

Eğer sizde basit, güvenli ve hızlı bir web sitesi istiyorsanız, Jekyll kesinlikle doğru araç. Ama unutmayın, Ruby ve bazı terminal komutlarıyla haşır neşir olmanız gerekebilir ve bir paneliniz olmadığını, her blog yazısı için markdown yazıp, git-push yapmanız gerekiyor. Bu da işin heyecanlı kısmı!

Zamanla muhtemelen Jekyll için daha fazla yazı yazarım.