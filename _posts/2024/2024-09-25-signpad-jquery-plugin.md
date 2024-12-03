---
layout: post
title: "SignPad jQuery Plugin"
date: 2024-09-25 14:52
categories: ["Eklentiler", "jQuery"]
tags: ["jquery", "signpad", "javascript", "plugin", "freebie"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/signpad-jquery-plugin-66f3f845b3e16.webp"
  alt: "SignPad jQuery Plugin"
---

Bugünde bunu geliştirdik, ve blog'a yeni bir kategori eklendi. Bu durum yüzünden şimdi tüm freebie'lerimi toplayıp, derleyip sonsuza kadar internette var olacak bir şekilde, düzenli arşivlemem gerekecek... Bu huzursuzluğumu seviyorum :)

## Yeni Eklenti

Yine boş durmadım. Daha öncede bahsettiğim RabbitCMS yazılımım ile ilgili sürekli geliştirmeler yapıyorum. Tabii bu geliştirmeleri ilk önce farklı bir ortamda üretim, bunu esnek bir eklenti haline getiriyorum ve sonra yazılımıma entegre ediyorum. Böylece bazı zamanlarda böyle güzel "freebie"'ler ortaya çıkabiliyor.

Yine bir geliştirme ortasında, sözleşmeleri imzalamak için checkbox işaretlemenin bir tık daha ötesine geçerek, sanal ortamı daha gerçekçi kılmak için bir imza modülü yapmak istedim. Böylece kullanıcılar, bir sözleşmeyi imzalaması gerekirse, gerçek manada imzalamış oluyorlar.

Tabii işin yasal boyutunda bunun tutarlılığı yada bağlayıcılığı konusunda bir avukat düzeyinde fikir sahibi değilim ama genel verileri paketlerken imza, atıldığı tarih, atan bağlantının ip adresi ve tanımlıysa özel kimlik numarası vb gibi elementler ile bir tanımlayıcı oluşturarak saklamanıza izin verdiği için, -gerekmez ama- gerekirse size oldukça tutarlı bir takım veriler sunabiliyor.

Geliştirme sürecine devam edeceğim zaman buldukça. Repo'yu ziyaret etmek isterseniz [SignPad-jQuery-Plugin](https://github.com/badursun/SignPad-jQuery-Plugin) adresini takip edebilirsiniz.

## SignPad jQuery Plugin Nedir?

SignPad, web sitelerine entegre edilebilen bir jQuery imza defteri eklentisidir. Kullanıcıların bir sözleşmeyi kabul etmek için imza atmalarını sağlar. Bu eklenti, geri alma işlevi ve imza verilerini JSON biçiminde sunucuya gönderme yeteneği gibi özelliklerle birlikte gelir. Dinamik boyutlandırma, stil seçenekleri ve kullanım kolaylığı sunar.

### Demo Var mı?

Şuradan [https://burakdursun.com/SignPad-jQuery-Plugin/](https://burakdursun.com/SignPad-jQuery-Plugin/) demoyu deneyebilirsiniz.

#### Özellikler

- Kullanıcıların imzalarını imzalamalarını ve kaydetmelerini sağlar.
- Geri Al ve Temizle işlevleri kullanıcı deneyimini iyileştirir.
- İmza, ek ayrıntılarla (zaman damgası, kullanıcı kimliği) birlikte PNG veya DATAPNG formatında sunucuya gönderilir.
- Dinamik genişlik, yükseklik ve stil parametreleri ile özelleştirilebilir, responsive uyumludur.

#### Kullanımı Nasıl?

Gerekli dosyaları ekledikten sonra, eklentiyi başlatmak için aşağıdaki örnekte gösterildiği gibi SignPad işlevini çağırın:

```javascript
$('#signpad').SignPad({
    width   : 400,    // Canvas width
    height  : 200,    // Canvas height
    userId  : 123,    // User ID (optional)
    canvasId: 'signature-pad-canvas',  // Canvas ID
    styles  : {
        clearBtn    : "btn btn-sm",    // CSS class for the Clear button
        undoBtn     : "btn btn-sm",    // CSS class for the Undo button
        saveBtn     : "btn btn-sm"     // CSS class for the Save button
    },
    onSave  : async (postData) => {
        // Actions after the signature is saved
        console.log("Signature saved with data:", postData);
    }
});
```

Daha detaylı kurulum ve klavuz için [SignPad-jQuery-Plugin](https://github.com/badursun/SignPad-jQuery-Plugin) repo'yu inceleyebilirsiniz. Tüm güncellemeler orada olacaktır.