---
layout: post
title: "En Kısa Alan Adı Google Tarafından Alındı"
date: 2008-07-30 22:35
categories: ["Gunun Gunlugu", "Gune Not"]
tags: ["gunluk", "blog", "true-loss", "nodejs", "railway"]
toc: true
---

## True Loss: Güncel Durum ve Gelecek Planları
Bugün True Loss projemle ilgili önemli gelişmeleri paylaşmak istiyorum. Bu proje, mikro interactive fiction (MIF) oyunlarını modern bir platformda sunmayı amaçlıyor. İşte şu ana kadar yaptıklarım ve gelecekteki planlarım:

### Yapılanlar

- True Loss için bir logo tasarladım. Aslında AI ile tasarlattığım bir kaç logoyu birleştirip tasarladım diyebiliriz.
- Asset yapısını statik bir sunucuda tutmaya karar verdim. Ucuz fakat limitsiz host işimizi görecektir.
- API tarafında ise railway üzerinde nodejs koşturmaya karar verdim. Hızlı ve socket desteği var. Hem IDE hemde diğer tüm servisler için anlık cevap verebilmemi sağlayacağını düşünüyorum. Ayrıca railway üzerinde ihtiyaç halinde scale etme şansım var.
- NodeJS tarafında express kullanarak hem api hemde belirli statik yapıları oluşturabileceğimi fark ettim. Özellikle tüm servislerde kullanmak için ortak bir config dosyası oluşturmam gerekiyordu. Bunu şöyle yapmayı planladım:

**configService.js**
```javascript
const express = require('express');
const router = express.Router();

// Dinamik Config Dosyaları
router.get('/config-base/trueloss-config.js', (req, res) => {
    const configFiles = {
        api_base: (process.env.API_BASE || 'http://localhost:8080/api/')
    };
    res.setHeader('Content-Type', 'application/javascript');
    res.send(`let TrueLossConfig = ${JSON.stringify(configFiles)}`);
});

// Dinamik Config Kontrolleri
router.get('/config-base/trueloss-checker.js', (req, res) => {
    res.setHeader('Content-Type', 'application/javascript');
    res.send(`
        // Eğer config yüklü değilse hataya git.
        if (typeof TrueLossConfig !== 'object') {
            location.href='/500-error.html';
            console.log('Config Not Loaded!');
        }
    `);
});

module.exports = router;
```

**Tüm statik sayfalarda**
```html
<!-- Configs -->
<script data-point="trueloss-base" src="/config-base/trueloss-config.js"></script>
<script data-point="trueloss-base" src="/config-base/trueloss-checker.js"></script>
```

Böylece config hatası durumunda tüm statik sayfalarda otomatik olarak bir 500 hatası gösterebileceğim. Tebrikler bana!

### Bugünkü Çalışmalar
Bugün, API tarafında temel yapıları ve route'ları tamamladım. Railway üzerinde ilk defa çalışmaya başladım ve yapıyı anlamak oldukça kolay oldu; neredeyse sıfır hata ile ilerledim. 

Kullanıcı yapısına öncelik verdim ve register, login kısımlarını tamamladıktan sonra güvenliğe önem vererek parola sıfırlama yapısını geliştirdim. Nodemail ile yine statik sunucunun mail servislerini birbirine entegre ettim. O da proje sürecinde beleşe gelecekti :)

IDE kısmında oturum açma işlemini başarıyla bağladım. Bir sonraki çalışma günümde, IDE ile API arasında kullanıcı ilişki yönetimini tamamlayacağım.

### Gelecek Planları

Kullanıcıların sistemle daha iyi etkileşimde bulunabilmesi için kullanıcı yönetim sistemini daha da geliştireceğim. Henüz tek dil veya çoklu dil mi olacak karar veremedim, o yüzden yarısı ingilizce yarısı türkçe saçma sapan bir noktadayım.
  
API ve IDE yapısında kullanıcı deneyimini artırmak için iyileştirmeler yapacağım. Özellikle kullanıcı ilişki yönetimi ve güvenlik özelliklerine odaklanacağım.

True Loss'un topluluk odaklı yapısını güçlendirmek için çeşitli entegrasyonlar ve özellikler ekleyeceğim. Kullanıcıların oyunlarını paylaşmasını ve toplulukla etkileşime geçmesini kolaylaştıracak sistemler geliştireceğim.

Bugünlük bu kadar.