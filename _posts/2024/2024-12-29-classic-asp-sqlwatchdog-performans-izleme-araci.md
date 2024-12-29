---
layout: post
title: "Classic ASP SQLWatchdog Performans İzleme Aracı"
description: "Classic ASP, günümüzde modern teknolojilerin gölgesinde kalmış olsa da, hala belirli projelerde kullanılmaktadır. "
date: 2024-12-29 09:45
categories: ["Yazilim", "Classic Asp"]
tags: ["performans", "optimizasyon", "vbscript", "classic-asp", "debugging", "sqlwatchdog", "sql", "asp", "slowlog", "slowquery", "github"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/classic-asp-sqlwatchdog-tool.webp"
  alt: "Classic ASP SQLWatchdog Performans İzleme Aracı"
---

Classic ASP uygulamalarında SQL sorgularının performansını izlemek ve optimize etmek her zaman kritik bir görev olmuştur. Bu ihtiyacı karşılamak amacıyla, **Classic ASP SQLWatchdog** adını verdiğim hafif ve etkili bir araç geliştirdim.

## **Classic ASP SQLWatchdog Nedir?**

Classic ASP SQLWatchdog, Classic ASP tabanlı uygulamalar için tasarlanmış bir SQL sorgu performans izleme ve profil oluşturma aracıdır. Bu araç, yavaş sorguları tespit eder, parametreli SQL ifadelerini destekler ve sorgu yürütme sürelerini kaydederek performans analizi yapmanıza olanak tanır. Özellikle eski ASP uygulamalarını sürdürmek ve optimize etmek için idealdir. Kurulumu oldukça basit olup, tüm projenizde ekstra kod değişikliği yapmadan entegre edebilir, ihtiyaçlarınıza göre geliştirebilirsiniz.

## **Özellikler**

- **Yavaş Sorgu Tespiti:** Belirlediğiniz eşik değeri aşan sorguları otomatik olarak tespit eder.
- **Parametreli SQL Desteği:** SQL enjeksiyon riskini azaltmak için parametreli sorguları destekler.
- **Sorgu Süresi Kaydı:** Tüm SQL sorgularının çalışma sürelerini kaydederek performans analizi yapmanızı sağlar.
- **Kolay Entegrasyon:** Mevcut kod tabanınıza minimum değişiklikle entegre edilebilir.

## **Neden Classic ASP SQLWatchdog?**

Classic ASP, modern web geliştirme teknolojilerinin gerisinde kalmış olabilir; ancak birçok işletme hala bu platformu kullanmaktadır. Bu projeyle, eski uygulamalarınızın performansını izlemek ve optimize etmek için modern bir çözüm sunmayı hedefledim. SQLWatchdog, uygulamalarınızın veritabanı işlemlerini daha verimli hale getirmenize yardımcı olacaktır.

## **Kurulum ve Kullanım**

1. **SQLWatchdog'u Projenize Dahil Edin:** `sqlwatchdog.asp` dosyasını projenize ekleyin.
2. **Bağlantınızı Ayarlayın:** Mevcut veritabanı bağlantınızı SQLWatchdog ile sarın.
3. **Sorgularınızı Çalıştırın:** Sorgularınızı her zamanki gibi çalıştırın; SQLWatchdog sorgu sürelerini otomatik olarak izleyecektir.
4. **Raporları İnceleyin:** Yavaş sorguları ve performans verilerini analiz edin.

Daha detaylı bilgi ve örnek kodlar için GitHub deposunu ziyaret edebilirsiniz: [Classic ASP SQLWatchdog](https://github.com/badursun/Classic-ASP-SQLWatchdog)

Classic ASP SQLWatchdog, eski uygulamalarınızın veritabanı performansını izlemek ve iyileştirmek için güçlü bir araçtır. Bu projeyi geliştirerek, Classic ASP topluluğuna katkıda bulunmayı ve geliştiricilerin işini kolaylaştırmayı amaçladım. Umarım siz de bu aracı faydalı bulur ve projelerinizde kullanırsınız. Yıldız atmayı unutmayın :)

Herhangi bir geri bildirim veya katkı için lütfen GitHub üzerinden iletişime geçmekten çekinmeyin. Zaman içerisinde projeyi güncellemeye devam edeceğim.