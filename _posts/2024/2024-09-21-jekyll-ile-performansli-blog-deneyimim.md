---
layout: post
title: "Jekyll ile Performanslı Blog Deneyimim"
date: 2024-09-21 13:01
categories: ["Yazılım", "DevOps"]
tags: ["cloudflare", "workers", "serverless", "javascript", "dns-router", "edge-compute", "jekyll", "jekyll-performance"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/jekyll-github-cloudflare-66eea9ab88343.webp"
  alt: "Jekyll ile Performanslı Blog Deneyimim"
---

**Jekyll** ile **GitHub** üzerinde oluşturduğum blog, gerçekten harika bir deneyim oldu. Statik bir site oluşturmaya yönelik bu süreçte pek çok şey öğrendim. Ama her zaman olduğu gibi, mükemmeli hedeflemek gibi bir takıntım var. Jekyll’in sağladığı statik HTML dosyalarının derlenmesi sırasında fark ettiğim birkaç sorun vardı. Bu dosyalar, gereksiz satır aralıkları ve boşluklarla doluydu. Bu durum, sayfa yükleme sürelerini olumsuz etkileyebilir. Bunu çözmek için kodlama işine girişebilirdim fakat bunun olumsuz tarafı, yeni bir sürümde tekrar adaptasyon yapmam gerekmesiydi.

![Cloudflare Workers Jekyll Content Minify](https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/compressed-jekyll-content-with-cloudflare-workers-66eea94de424e.webp)

Boşlukları temizlemek için bazı eklentileri araştırırken, **Cloudflare Workers** ile bu işi çözebileceğimi fark ettim. Bildiğiniz gibi, bütünleşik sistemler kurmayı ve bu sistemleri performans arttırmaya yönelik değerlendirmeyi çok severim. Cloudflare’in sunduğu bu harika araçla, HTML çıktılarımı neredeyse %85 oranında sıkıştırabileceğim, çok daha hızlı ve etkili bir sistem geliştirdim. Şimdi gelin bu süreci detaylıca inceleyelim.

## Jekyll’in Artıları ve Eksileri
**Jekyll**, özellikle **GitHub Pages** ile entegrasyonu sayesinde blog oluşturmayı çok kolaylaştırıyor. Markdown desteği ile yazı yazmak oldukça pratik. Ayrıca, temalar ve eklentilerle kişiselleştirme seçenekleri sınırsız. Ancak, Jekyll’in bazı sınırlamaları var. Statik dosyalar derlendiğinde, optimize edilmemiş HTML’lerin varlığı, sayfa performansını etkileyebilir. Bu yüzden sıkıştırma ve optimizasyon konularına dikkat etmek gerekiyor.

## Cloudflare Workers ile HTML Sıkıştırma
Cloudflare Workers, sunucu tarafında çalışarak HTTP isteklerine hızlı yanıt verebilen bir yapı sunuyor. HTML çıktılarımı sıkıştırmak için kullanacağım kod aşağıdaki gibi:

```javascript
export default {
    async fetch(request) {
        const res = await fetch(request);
        const contentType = res.headers.get("Content-Type");

        class HTMLCompressor {
            constructor() {
                this.codeBlocks = [];
                this.isInsideCodeBlock = false;
            }

            element(element) {
                // Kod blokları içinde sıkıştırma yapma
                if (element.tagName === 'code') {
                    this.isInsideCodeBlock = true;
                    element.onEndTag(() => {
                        this.isInsideCodeBlock = false;
                    });
                }
            }

            text(textChunk) {
                if (!this.isInsideCodeBlock) {
                    // Sıkıştırma işlemi: Fazla boşlukları, satır sonlarını kaldır
                    let compressedText = textChunk.text.replace(/\s{2,}/g, ' ')
                        .replace(/\n/g, '')
                        .replace(/\t/g, '')
                        .replace(/\>\s*\</g, '><');

                    // Metni güncelle
                    textChunk.replace(compressedText);
                }
            }

            comments(comment) {
                // Yorumları kaldır
                comment.remove();
            }
        }

        if (contentType && contentType.includes("text/html")) {
            return new HTMLRewriter()
                .on('code', new HTMLCompressor())  // Kod blokları için
                .onDocument(new HTMLCompressor())  // Diğer tüm HTML için
                .transform(res);
        }
        
        return res;
    },
};
```

Bu kod, HTML içeriğindeki gereksiz boşlukları, satır sonlarını ve yorumları temizlerken <code> bloklarının içeriğini korumak için tasarlanmıştır. Cloudflare Workers ile bu kodu çalıştırmak oldukça basit. İşte adımlar:

## Cloudflare Üzerinde Workers Kurulumu
- Cloudflare Hesabı Oluşturun: Eğer bir Cloudflare hesabınız yoksa, Cloudflare üzerinden ücretsiz bir hesap oluşturun.
- Workers Paneline Giriş Yapın: Hesabınızla giriş yaptıktan sonra, Cloudflare dashboard'unda "Workers" sekmesine gidin.
- Yeni Worker Oluşturun: "Create a Worker" butonuna tıklayın ve yukarıdaki kodu boş alana yapıştırın.
- Yayınlayın: Değişiklikleri kaydedin ve çalıştırın. Cloudflare, her gelen istek için HTML içeriğinizi sıkıştıracaktır.
- Domain Ayarları: Eğer kendi domaininizi kullanıyorsanız, yönlendirmeleri ve ayarları yapılandırmayı unutmayın.

## Performansı Arttırma Yöntemleri
Cloudflare’in sunduğu diğer özellikler sayesinde blogunuzu daha da hızlı hale getirebilirsiniz. Cloudflare Cache kullanarak sık sık talep edilen içeriği önbelleğe alabilir, böylece sunucuya gelen istekleri azaltabilirsiniz.

Ayrıca, Cloudflare’nin CDN (Content Delivery Network) hizmeti ile statik dosyalarınızı global ölçekte hızlandırabilir ve kullanıcılarınıza daha iyi bir deneyim sunabilirsiniz. Bu sistem, kullanıcıların dosyalara daha hızlı erişim sağlamasını ve sayfa yükleme sürelerini azaltmasını mümkün kılar.

Jekyll ile oluşturduğum blog deneyimimde, Cloudflare Workers sayesinde HTML içeriğimi optimize etmek oldukça kolay oldu. Statik dosyaların sıkıştırılması ve Cloudflare’nin diğer özelliklerini kullanarak performansı artırmak, blogumu daha hızlı ve kullanıcı dostu hale getirdi. Eğer siz de benzer bir deneyim yaşamak istiyorsanız, yukarıdaki adımları izleyerek başlayabilirsiniz. Unutmayın, performans her zaman bir adım önde olmak için önemli bir faktördür!

Bütün bunların avantajı: 0 (sıfır) maliyet! Doğru okudunuz. Cloudflare ücretsiz, github repo ücretsiz, jekyll ücretsiz. Peki neden şimdi wordpress veya benzeri bir sistemi, bir hosting üzerinde çalıştırayım? Üstelik asla bu kadar performanslı olmayacakken!