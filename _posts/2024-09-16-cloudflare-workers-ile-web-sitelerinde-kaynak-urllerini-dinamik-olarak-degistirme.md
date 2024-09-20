---
layout: post
title: "Cloudflare Workers ile Web Sitelerinde Kaynak URLlerini Dinamik Olarak Değiştirme"
date: 2024-09-16 11:37
categories: ["Yazılım", "DevOps"]
tags: ["cloudflare", "workers", "serverless", "javascript", "dns-router", "edge-compute"]
toc: true
image:
  path: "/assets/img/cloudflare-workers.jpg"
  alt: "Cloudflare Workers ile Web Sitelerinde Kaynak URLlerini Dinamik Olarak Değiştirme"
---

## Cloudflare Workers Nedir?
Cloudflare Workers, serverless bir JavaScript çalışma ortamıdır. Herhangi bir altyapı yönetimine ihtiyaç duymadan, web sitelerinizin kenar sunucularında (edge servers) kod çalıştırmanızı sağlar. Bu özellik sayesinde, sitenizin ziyaretçilerine en yakın sunucularda dinamik içerik işlemleri yaparak hız ve performans kazanırsınız. Bu da, global ölçekte düşük gecikmeyle yüksek performanslı hizmet sunma anlamına gelir.

![cloudflare-worker-principles](/assets/img/cloudflare-worker-principles.jpg)

### Cloudflare Workers ile Neler Yapabilirsiniz?

Cloudflare Workers ile yapabileceklerinizin sınırı neredeyse yok. Sunucu tarafında yönetim gerektiren birçok işlemi, sunucunuza yük bindirmeden gerçekleştirebilirsiniz. Örneğin:

- HTML manipülasyonları (içerik değiştirme, dinamik veriler ekleme)
- API taleplerini yönlendirme ve işleme
- Otomatik önbellekleme stratejileri uygulama
- Güvenlik ve doğrulama kontrolleri ekleme
- Web sitenizdeki statik ve dinamik içerikleri optimize etme
- Dinamik içerikleri (html, json) ve API isteklerini cache'leme
- Ekstra güvenlik katmanı oluşturma ve daha fazlası...

**Edge Compute** olarak bilinen bu yaklaşım, web geliştiricilerine hem esneklik hem de performans avantajları sağlar.

## Örnek Senaryo: Web Sitesi Kaynak URL'lerini Değiştirme

Diyelim ki, web sitenizdeki bazı kaynakların (resim, script, link gibi) URL'lerini değiştirmek istiyorsunuz. Normal şartlarda tüm dosyalarınızı güncellemek gerekebilir; fakat Cloudflare Workers ile bu işi otomatik hale getirebilirsiniz. Aşağıda, web sitenizin HTML içeriğinde bulunan `img`, `link`, `script` ve `a` etiketlerinin `src` ve `href` değerlerindeki URL'leri dinamik olarak değiştiren bir örnek bulacaksınız. ORL_URL değerinde, değişmesini istediğimiz adresi, NEW_URL de ise değişecek adresimizi yazıyoruz.

### Örnek Kod

```javascript
export default {
    async fetch(request) {
        const OLD_URL = "http://deneme.com/";
        const NEW_URL = "https://www.deneme.com/";

        class AttributeRewriter {
            constructor(attributeName) {
                this.attributeName = attributeName;
            }
            element(element) {
                const attribute = element.getAttribute(this.attributeName);
                if (attribute) {
                    element.setAttribute(
                        this.attributeName,
                        attribute.replace(OLD_URL, NEW_URL),
                    );
                }
            }
            text(text) {
                // Bu örnekte text manipülasyonu yapılmıyor
            }
        }

        const rewriter = new HTMLRewriter()
            .on("a", new AttributeRewriter("href"))
            .on("link", new AttributeRewriter("href"))
            .on("script", new AttributeRewriter("src"))
            .on("img", new AttributeRewriter("src"));

        const res = await fetch(request);
        const contentType = res.headers.get("Content-Type");

        if (contentType.startsWith("text/html")) {
            return rewriter.transform(res);
        } else {
            return res;
        }
    },
};
```

