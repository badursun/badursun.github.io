---
layout: post
title: "Güne Not: 10 Ağustos 2024"
date: 2024-09-10 20:00
categories: ["Gunluk", "Gune Not"]
tags: ["gunluk", "blog", "true-loss", "nodejs", "railway"]
toc: true
---


Bugünde tamamen True Loss platformu'nu geliştirmeye adadığım bir gün oldu. IDE ve API üzerinde çalıştım ağırlıkla.

IDE arayüzünü iyice toparladım ve başlangıç dilini ingilizce olarak belirledim. Şöyle bir arayüz oldu:

![True Loss IDE UI](/assets/img/true-loss-ide-ui-1.jpg)

API üzerinde ise temel ihtiyaçları ve bazı düzenlemeleri yaptım.

## Dosya yüklemek için AWS S3 bucket entegrasyonu
Railway ve MongoDB Atlas üzerinde koştuğumuz için API response süreleri oldukça iyi. Kayıtlı oyunları listelememiz 80-150ms içerisinde gerçekleşiyor. Veritabanı olarak obje yönetimli bir sistem tercihi çok doğru bir seçim oldu diyebilirim. Railway ise hem sürüm kontrolü hem deploy süreçleri için git entegrasyonu ile müthiş kolaylık aslında. Tabii bu süreçlerin içinde de dosya yönetimi için bir geliştirme yapmam gerekiyordu. Biraz araştırma ve geçmiş deneyimlerim ile AWS S3 bucket kullanmak çok mantıklı geldi fakat şöyle bir sorun var: Railway'e dosya yüklemek, işlemek ve dağıtmak oldukça maliyetli, performans düşürücü ve problemli olacak. Peki, dosyaları nasıl yükleyecektim?

Bu noktada AWS S3'ün [Presigned URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html) özelliğini, SDK kit'i ile kullanmaya karar verdim. S3'ün de maliyetleri var fakat API sistemini tamamiyle Auth ve Control merkezi olarak kullanmak istediğim için, API ile signin olan kullanıcıya bir presigned url oluşturup, dosyasını o anda S3'e yüklüyorum. Böylece API sistemini gereksiz meşgul etmemiş oluyorum. Tabii bu süreç sadece IDE ve development süreçleri için böyle olacak. Publish edilmiş bir oyun mutlaka Pull-CDN üzerinden dağıtacak dosyaları.

## Veri Doğrulama ve Oyun Şablon Yapısı
Oyun şablonu yapısında halen geliştirmelerim devam ediyor. Özellikle oyun sürecinin olabilecek en interaktif şekilde gerçekleşmesi için fazlasıyla özen gösteriyorum. Sahne geçişleri, sahne efektleri, ses efektleri gibi bir çok detay ekledim. Böylece bir seçim, sizi olmaması gereken bir sahneye götürürse, sahneyi sallayabilir, ışık patlaması veya ani ses yüklemeleri kullanılabilecek.

Böyle bir oyun verisini yönetmek oldukça zor çünkü temel şablon içinde sahneler, sahneler içinde bir çok ayar ve seçimler, her seçimin ayrı efekt ve hedefleri gibi dallanan bir obje yapısı var.

Mevcut obje yapısı şöyle;

```json
{
    "title"         : "True Loss Game Title",
    "version"       : "1.0.0",
    "create_date"    : null,
    "update_date"    : null,
    "meta_data"     : {
        "template_version"  : "1.0.0",
        "author_name"       : "Anthony Burak Dursun",
        "author_email"      : "badursun@gmail.com",
        "author_website"    : "badursun@gmail.com"
    },
    "game"  : {
        "scenes"    : [
            {
                "id"            : 1,
                "text"          : "You can write the first introduction to the game here.",
                "sceneSettings" : {
                    "isFinal"       : false,
                    "isTrueEnd"     : false,
                    "sceneAction"   : "none",
                    "sceneMusic"    : "none"
                },
                "files"         : {
                    "images"    : [],
                    "videos"    : [],
                    "audio"     : []
                },
                "options"   : [
                    {
                        "text"          : "Take Me to the Second Stage.",
                        "targetSceneId" : 2
                    },
                    {
                        "text"          : "Take Me to the Third Stage.",
                        "targetSceneId" : 3
                    }
                ]
            }
        ]
    }
}
```

Böyle bir yapıyı geliştirme sürecinde dallandırmak, IDE üzerine sunmak, kayıt için geri çekerken obje bütünlüğünü kıyaslamak güvenlik bakımından oldukça zorlayıcı. Evet, halen güvenlik ve performans takıntım üst düzeyde.

## API Servisi Hakkında
API servisinde ilk defa geleneksel yazılım dillerinden birini tercih etmek yerine, daha modüler ve performanslı bir tercih olan NodeJS dilinde kodlamaya karar verdim çünkü mümkün olan en kısa sürede, en güvenli ve performanslı bir sonuca varmak istiyordum. Ayrıca, henüz bir dil tercihi yapmamışken, oyun verisini zaten bir obje tarzında tutmayı planlamıştım. Böyle bir yapıyıda ne php, ne dot-net ne de classic asp ile yönetmek mümkün değildi.

Çok uzun zamandır javascript, typescript kodlama yaptığım için nodejs konusunda hiç yabancılık çekmedim. Yeni SDK kitlerini kulanırken de GPT sağolsun, çok iyi yol gösterdi diyebilirim. Bakın buraya yazıyorum, ayda 20$'a bu kapasitede hintli bile çalıştıramazsınız...

API servislerini hem local hemde remote olarak test ediyorum ve bir postman collection hazırlıyorum. Dökümante edecek zamanım olmadı ama postman üzerinden muhtemelen bir swagger hazırlarım. Neden dökümantasyon derseniz, True Loss bir komunite projesi olacak. Sadece bir oyun merkezi değil, IF tarzında oyun seven herkesin, kodlama bilmeden oyun üretebileceği bir portal olacak.

İleride de ufakta olsa bir komunite oluşturabilirse, monetize konusunda ayrıca çalışacağım.

## Peki; True Loss tam olarak nedir?
**True Loss**, 90'ların benzersiz oyun tadını günümüz teknolojisiyle buluşturan bir **mikro interactive fiction** (MIF) deneyimidir. Temelde **gerilim dolu** ve **gizemli** bir metin tabanlı oyun sistemi sunan True Loss, sizi hikayenin içine çekerken, her kararın oyunun akışını ve sonucunu nasıl etkilediğine tanık olmanızı sağlar. Etkileşim için tüm teknolojik tetikleyicileri kullanır.

- True Loss: Editor
- True Loss: Game Engine
- True Loss: Gaming Platforms

olarak 3 alt kırılımda, True Loss platformunu tamamlamayı planlıyorum.

### Hangi teknolojiler?
Şimdilik aşağıdaki teknolojileri entegre etmiş bulunuyorum:

- NodeJS: API Servisleri
- Github: Sürüm Kontrolü
- Railway: Deploy Sistemi
- MongoDB: Veritabanı
- AWS S3: Dinamik Dosya Yönetimi
- Web Host: Statik Asset Yönetimi
- BunnyCDN: Global İçerik Dağıtımı

Aslında bu kadar değil ama bugünlük bu kadar. Bu yorgunluğun üzerine "Günlük" sözümü ancak böyle tutabilirim bir süre.