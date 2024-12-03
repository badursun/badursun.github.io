---
layout: post
title: "MySQL tabloları için MyISAM yada InnoDB Kıyaslama"
date: 2019-03-23 16:33
categories: ["Yazilim", "Database"]
tags: ["mysql", "myisam", "innodb", "database-engine"]
toc: true
---

# MyISAM ve InnoDB Arasındaki Farklar
Veritabanı tabloları için MySQL'de en çok kullanılan iki depolama motoru MyISAM ve InnoDB'dir. Bu iki motor arasında önemli farklar bulunur. **InnoDB**, **MyISAM**'a göre daha kompleks bir yapıya sahiptir. MyISAM, basit işlemler için tercih edilirken, InnoDB genellikle çok verinin olduğu karmaşık işlerde kullanılır.

### MyISAM'in İyi Yanları
- **SELECT** işlemleri için hızlıdır.
- Sunucu kaynaklarını daha az tüketir.
- Kısıtlayıcılığı az olduğu için başlangıç seviyesi projeler için uygundur.

### MyISAM'in Kötü Yanları
- **INSERT** ve **UPDATE** işlemleri için yavaştır.
- 4 GB data sınırı vardır.
- Text tabanlı bir depolama sistemi olduğu için sunucu tarafında bozulma ihtimali daha yüksektir.
- **Foreign key** desteklemez. Bu yüzden tablolar arasında bütünlük sağlamak, duruma göre, veritabanı ve SQL dışına çıkarak programa/scripte bırakılır.
- Transaction desteği yoktur, yani tabloların yedeğini tutmaz ve bir kaza olursa veriler geri alınamaz.
- **Table-level lock** yapısındadır. Yani işlem emri geldiğinde tüm tabloyu kilitler ve işlem tamamlandıktan sonra diğer işlemlere geçer.

### InnoDB'nin İyi Yanları
- **Foreign key** desteği sunar ve veri bütünlüğünü veritabanı seviyesinde de korur.
- **INSERT** ve **UPDATE** işlemleri hızlıdır.
- Transaction desteği vardır, tabloların yedeğini tutar ve veri kayıplarını geri alma imkanı sunar.
- **Row-level lock** yapısındadır. İşlem yapmak için tüm tabloyu kilitlemez.

### InnoDB'nin Kötü Yanları
- Veritabanı **foreign key** tasarımı daha fazla zaman alır ve programda/scriptte ilişkiler konusunda esnekliğe izin vermez.
- Transaction işlemleri nedeniyle daha yavaştır.
- Daha fazla sistem kaynağı tüketir.

> **NOT**: MyISAM; tip tabloları için kullanılabilir. Statik durumlarda InnoDB'ye gerek yoktur. MyISAM ile daha hızlı sonuç alınabilir.
