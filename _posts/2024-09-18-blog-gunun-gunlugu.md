---
layout: post
title: "Geçmişe Not: 18 Ağustos 2024"
date: 2024-09-18 20:00
categories: ["Gunluk", "Gecmise Not"]
tags: ["gunluk", "blog", "true-loss", "rabbitcms", "jquery", "tag-manager", "facebook-meta", "meta-pixel"]
toc: true
---

Bir kaç gündür günlük yazmadım. Dolayısıyla Güne Not değil, Geçmişe Not olarak ilk yazımı yazma fırsatım oldu.

Biraz yoğun bir süreçten geçtiğim için günlük planım tutmadı. Zaten bu olasılığı düşündüğüm için günlüğü 3 ayrı alt kategoride planlamıştım.

İşte Geçmişe Not'um:

# Neler oldu?
Geçen cuma gününe kadar True Loss projesi üzerinde çalışıyordum fakat biranda ciddi bir iş yoğunluğu fırlayınca hem mevcut müşteri işlerine yöneldim hemde işlerin akışı sebebiyle RabbitCMS üzerinde bazı geliştirmeler hazırladım. Bu süreçte aklımda olan True Loss projesi için de yine bazı geliştirme ve düzeltmeleri planladım. Ayrıca yeni bir projeye de kafamda başladım. Bu biraz daha amme hizmeti uygulaması olacak olan E-Posta İmza HTML üreticisi. Hazır şablonlar ile e-posta imza html dosyasını oluşturup, e-posta programınıza dahil edebileceğiniz basit bir arayüzü olacak olan uygulama fikri.

Daha öncede bir çok proje aklıma geldi, ciddi anlamda üzerine düşündüm fakat ben düşünürken birileri yaptı :) Bu sebeple artık düşünmüyorum, uyguluyorum. Bir ara yeni bir Geçmişe Not yazısında başlamadan başarısız olan projelerimden bahsedeceğim.

Şimdi gelelim 2 projenin durumuna;

## RabbitCMS
RabbitCMS de uzun zamandır planladığım fakat bir türlü geliştirmesini yapamadığım Google Tag Manager ve Facebook Meta Pixel ölçümleme entegrasyonlarını tamamladım. Bu yapıyı esnek kurmam gerekiyordu ve bunun için açıkcası boş bir kafayla düşünmem gerekiyordu. Bir türlü o boşluğa ulaşamadığım için sürekli parça parça müşteri sistemlerinde geliştirmeler ve yamalar yazmam gerekiyordu. Bunu çözmek için önce bir Class based plugin oluşturmayı düşündüm ama sonrasında bunun geliştirilecek bir yapı olmadığına karar verdiğim için vazgeçtim. Detaylı bir şekilde dev-docs okuduğumda fark ettim ki sadece belirli parametreleri mevcut yapıya taşıyarak tüm süreci bir jQuery trigger'ı ile çözebilirim.

Bunun için tüm sistemde çalışan bir jQuery trigger seti yazdım. Örnek olarak aşağıda sadece bir event için yazdığım scripti veriyorum. Bu şekilde event'ları sizde çoğaltabilirsiniz. Böylelikle asenkron süreçleri sadece bir trigger'ı aktive edip, datayı pass geçebilirsiniz. Gerisini script halledecektir. Ve bu yapıda da oldukça esnek bir alt yapı kurmuş olacaksınız. İlerleyen süreçte farklı entegrasyonlara da uygun hale gelmiş olacak. Mesela Yandex Metrica veya diğer ölçümleme araçlarını da bu yapıya dahil edebilirsiniz.

```javascript
$(document).ready(function() {
    /*************************************************************
        Complete Order click
        $(document).trigger("rabbitCms.InitiateCheckout", [dataToPass]);
    *************************************************************/
    $(document).on("rabbitCms.Purchase", function(e, purchaseData){
        console.log('TRIGGER: Purchase Complete', purchaseData);
        
        /* TagManager */
        if (typeof dataLayer == 'object') {
            dataLayer.push({ ecommerce: null });
            dataLayer.push({
                event       : "purchase",
                ecommerce   : {
                    transaction_id  : purchaseData.orderno,
                    value           : purchaseData.orderAmount,
                    // tax          : 0,
                    shipping        : purchaseData.shippingAmount,
                    currency        : purchaseData.orderCurrency,
                    coupon          : purchaseData.couponCode,
                    items           : purchaseData.items
                }
            });
        };

        /* Facebook Analytics */
        if(typeof fbq=='function'){
            const contents = purchaseData.items.map(result => ({
                id      : result.item_id,
                quantity: result.quantity
            }));

            fbq('track', 'Purchase', {
                value       : purchaseData.orderAmount,
                currency    : purchaseData.orderCurrency,
                contents    : contents,
                num_items   : contents.length,
                content_type: 'product',
                eventref    : ''
            });
        };
    });
});
```

## True Loss
True Loss projesinde ise son geldiğim nokta IDE ile API arasında performans iyileştirmeleri ile kaldı. Aslında IDE kısmını bitirdim denebilir fakat oyun objesinde bazı değişiklikler ve eklemeler yapmam gerekiyordu. Çünkü daha önce anlattığım gibi, görsel ve diğer dosyaları depolamak için AWS S3 kullanmaya karar verdim fakat Mongodb Atlas'ın obje depolama özelliği sayesinde tüm veriye çok hızlı erişsem de, bu noktada sorun objelerin içinde ki dosyaların erişimiydi. Hız konusu için CDN kullanacağım fakat S3 lokasyonumun birden fazla olabilmesi olasılığı bana yapıyı daha ince hazırlamam gerektiği konusunda bazı fikirler uyandırdı. 

Öyle ki, ben obje içinde bir görüntü, ses veya farklı dosya için direkt full url kullanacakken, olası bir durumda storage taşıma/değiştirme gibi bir gereksinim doğarsa, tüm kayıtlı oyunlarda arama-değiştirme işlemi uygulamam gerekecekti. Bende bu yapıyı ayrı bir dosya tablosu oluşturarak, oyunlara dosyaları bir obje id si ile yerleştirmeye karar verdim. Böylece belirli bir dosyayı, S3 bucket adına göre ayırıp, dosya taşıma-güncelleme işlemini sadece dosya tablosundan yapabilecektim. Ayrıca bir oyunda kullanılan dosyanın sadece o oyun ve o kullanıcı için ayrıştırılabilir olmasını sağlayabileceğim.

Yine IDE üzerinde bir dosya kırılımı sunarak, Görsel-Video-Ses-Diğer şeklinde 4 farklı kategori sunabilecektim.

![True Loss File Manager](/assets/img/true-loss-filemanager.jpg)

Nitekim yapıyı bu şekilde kurdum.  NodeJS tarafında da `listFiles` kodumu aşağıdaki gibi dinamikleştirdim.

> NOT: Parametre ve request'ler ile ilgili herhangi bir sqlinjection koruması yok gibi gelmesin, bu işi yapan sıkı bir middleware kullanarak tüm giriş parametrelerini niteliklerine kadar kontrol ediyorum. O kodum da bana kalsın :)


```javascript
exports.listFiles = async (req, res) => {
    const user = req.user;

    try {
        if (user) {
            const { page = 1, limit = 10, fileType, gameFiles } = req.query;
            const skip = (page - 1) * limit;

            try {
                // Dinamik sorgu oluştur
                const query = { userId: user._id };
                // Eğer fileType varsa category filtresi eklenir
                if (fileType) {query.category = fileType}
                if (gameFiles) {query.gameId = gameFiles}

                // Dosyaları dinamik sorguya göre al
                const files = await File.find(query)
                    .skip(skip)
                    .limit(parseInt(limit))
                    .sort({ addedDate: -1 }); // En yeni dosyalar önce gelir

                // Toplam dosya sayısı
                const totalFiles = await File.countDocuments(query);

                res.status(200).json({
                    status      : 200,
                    totalFiles  : totalFiles,
                    filters     : [
                        fileType ? fileType : 'All Types',
                        gameFiles ? gameFiles : 'All Games',
                    ],
                    files       : files,
                    totalPages  : Math.ceil(totalFiles / limit),
                    currentPage : parseInt(page)
                });
            } catch (error) {
                console.log('server_error:listFiles', error, error.message);
                res.status(500).json({
                    status: 500,
                    message: 'Server error listFiles'
                });
            }
        } else {
            res.status(404).json({
                status: 404,
                message: 'user_not_found'
            });
        }
    } catch (error) {
        console.log('server_error:saveFile', error, error.message);
        res.status(500).json({
            status: 500,
            message: 'Server error: ' + error.message
        });
    }
};
```


Son bir kaç gün için geçmişe notlarımız bu kadar. Sonraki yazıda görüşmek üzere, hoşçakalın!