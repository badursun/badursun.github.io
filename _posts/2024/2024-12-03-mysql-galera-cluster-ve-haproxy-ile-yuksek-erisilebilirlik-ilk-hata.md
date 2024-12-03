---
layout: post
title: "MySQL Galera Cluster ve HAProxy ile Yüksek Erişilebilirlik: İlk Hata!"
description: "Galera Cluster, MySQL veya MariaDB veritabanları için gerçek zamanlı, senkronize ve dağıtılmış bir veritabanı çözümüdür."
date: 2024-12-03 08:28
categories: ["Yazilim"]
tags: ["yazilim", "mysql", "galer-cluster", "haproxy"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/galera-cluster-mysql-haproxy-674eba8e717ca.webp"
  alt: "MySQL Galera Cluster ve HAProxy ile Yüksek Erişilebilirlik"
---

# MySQL Galera Cluster ve HAProxy ile Yüksek Erişilebilirlik: İlk Hata!

MySQL Galera Cluster ve HAProxy, yüksek erişilebilirlik (HA) ve yük dengeleme sağlamak için sıkça kullanılan iki güçlü araçtır. Ancak, bu tür sistemlerde bazen GTID (Global Transaction ID) hataları gibi sorunlarla karşılaşılabilir. Bu yazıda, Galera Cluster ve HAProxy hakkında temel bilgiler verdikten sonra, "An attempt was made to binlog GTID..." hatasının nedenlerini ve çözüm yollarını inceleyeceğiz. Bu hata beni benden alan bir kriz yarattı ilk başta. Tabi bu, böyle bir yapıyı ilk defa kullandığım için kriz yarattı :)

## MySQL Galera Cluster Nedir?

Galera Cluster, MySQL veya MariaDB veritabanları için gerçek zamanlı, senkronize ve dağıtılmış bir veritabanı çözümüdür.

### Özellikleri

- **Senkron Replikasyon**: Tüm düğümler veriyi aynı anda günceller, veri kaybı riskini minimize eder.
- **Multi-Master Desteği**: Her düğüm üzerinde okuma ve yazma yapılabilir.
- **Otomatik Üye Yönetimi**: Bir düğüm kapandığında küme otomatik olarak yönetilir.

### Galera Cluster Neden Kullanılır?

- **Yüksek Erişilebilirlik**: Düğüm çökse bile küme çalışmaya devam eder.
- **Performans**: Yatay ölçeklenebilirlik sağlar.
- **Veri Tutarlılığı**: Tüm düğümler aynı anda senkronize kalır.

## HAProxy Nedir?

HAProxy, ağ trafiğini verimli bir şekilde yönlendiren bir yük dengeleme ve proxy çözümüdür.

### Özellikleri

- **Yük Dengeleme**: Gelen veritabanı sorgularını Galera Cluster düğümleri arasında dengeler.
- **Sağlık Kontrolleri**: Düğümlerin çalışır durumda olduğunu kontrol eder ve sorunlu düğümleri trafiğe kapatır.
- **Esneklik**: HTTP, TCP ve diğer protokollerle çalışabilir.

### Neden HAProxy Kullanılır?

- **Performans Artışı**: Trafiği dengeli dağıtarak aşırı yüklenmeyi önler.
- **Kesintisiz Çalışma**: Düğüm hatalarını algılar ve kesintisiz bir deneyim sunar.

# İlk Hata: "An attempt was made to binlog GTID..."

> Error Code: 1950
> An attempt was made to binlog GTID 1000-1000-396571 which would create an out-of-order sequence number with existing GTID 1000-1000-396571, and gtid strict mode is enabled

Bu hata, yapı kurulduktan sonra karşılaştığım ilk hataydı. Herşey bitmiş, artık yapıyı yapıyıp yeni yerinde ayağa kaldıracağım için veritabanını aktarmaya çalışıyorken karşılaştık kendisiyle. MySQL Galera Cluster üzerinde GTID modu etkinken ve strict mode açık olduğunda oluşabilen bir hata. Bu hata, MySQL'de GTID (Global Transaction ID) sistemi etkin olduğunda ve GTID Strict Mode açıkken, binlog GTID sıralamasında bir tutarsızlık oluştuğunda meydana gelebiliyor. MySQL, aynı GTID'nin iki kez yazılmasına izin vermez çünkü bu durum, veri tutarlılığı ve replikasyon güvenilirliği açısından sorun yaratabilir. Galer Cluster yapısında, bir veri herhangi bir veritabanına yazıldığında, diğer slave gibi olan kümeler de aslında master gibi kullanılır. Bunu da eşitliği takip ederek yapar. Siz bir veri yazmaya başladığınızda, özellikle multiple transaction gerçekleştirdiğinizde (sql dump yazmak gibi) bu hata görülebiliyor.

## Hatanın Sebepleri

- **Replikasyon Sorunları**: Master ve Slave arasında bir tutarsızlık olabilir. Aynı GTID, replikasyon sırasında birden fazla kez işlenmeye çalışmış olabilir.
- **Aynı Transaction ID'nin Tekrarlanması**: Galera Cluster'da birden fazla düğüm aynı GTID'yi kaydetmeye çalışmış olabilir.
- **Manual Binlog Manipülasyonu**: Binlog dosyalarına manuel bir müdahale yapılmış olabilir.
- **Binlog Uyuşmazlığı**: Binlog dosyaları arasında uyumsuzluk.
- **Yedekten Geri Yükleme**: Yedekten geri yüklenen bir işlem sırasında GTID sıralaması karışmış olabilir.
- **GTID Strict Mode**: gtid_mode ve enforce_gtid_consistency parametreleri etkin olduğunda, GTID sıralama kurallarına uyulmaması bu hatayı tetikleyebilir.
- **GTID Tutarsızlığı**: Yedekten geri yükleme sırasında gtid_purged parametresinin yanlış kullanılması.
- **HAProxy Yönlendirme Sorunları**: HAProxy, aynı yazma işlemini birden fazla düğüme yönlendirmiş olabilir.

## Çözüm Yöntemleri

### 1. GTID Strict Mode'u Geçici Olarak Devre Dışı Bırakmak

Eğer GTID sırasındaki tutarsızlığı düzeltmeniz gerekiyorsa, geçici olarak strict mode'u devre dışı bırakabilirsiniz.

```sql
SET GLOBAL gtid_strict_mode = OFF;
```

Daha sonra işlemi tekrar deneyin. İşlemler bittikten sonra strict mode'u tekrar etkinleştirin:

```sql
SET GLOBAL gtid_strict_mode = ON;
```

> Not: Bu yöntem, veri tutarlılığı açısından risklidir. Mutlaka dikkatli kullanın.

### 2. Tekrarlanan GTID’yi Atlamak

Eğer bir replikasyon hatası nedeniyle aynı GTID tekrar edilmeye çalışıyorsa, replikasyonu ilerletmek için hatalı GTID'yi atlayabilirsiniz.

Replikasyonu durdurun
```sql
STOP SLAVE;
```

Hatalı GTID'yi SET GLOBAL sql_slave_skip_counter ile atlayın
```sql
SET GLOBAL sql_slave_skip_counter = 1;
```

Replikasyonu yeniden başlatın
```sql
START SLAVE;
```

### 3. GTID'nin Binlog’da Zaten Olduğunu Kontrol Edin

MySQL'de mevcut GTID'leri kontrol edin ve çakışmayı tespit edin:

```sql
SHOW MASTER STATUS;
SHOW BINLOG EVENTS IN 'binlog_name';
```

GTID çakışması varsa, gerekli işlemleri yaparak tutarsızlığı giderin.

### 4. Backup Geri Yükleme Durumunu Kontrol Etmek

Eğer yedekten geri yükleme sırasında bu hata oluştuysa, aşağıdaki gibi bir yol izleyin: Yedeği alırken SET `@@GLOBAL.gtid_purged` komutuyla **GTID_PURGED** değerini sıfırlayarak temiz bir geri yükleme yapabilirsiniz.

```sql
SET @@GLOBAL.gtid_purged = '';
```

> Uyarı: Bu işlem sırasında veri kaybı olabileceği için dikkatli olun.

### 5. En Son Çözüm: Replikasyonu Baştan Kurun

Eğer replikasyon sorunları çözülemiyorsa:

- Mevcut replikasyonu durdurun.
- Slave sunucusunu temizleyin ve master sunucudan yeni bir yedek alarak replikasyonu yeniden başlatın.

## Gelecekteki Sorunları Önlemek İçin

- **GTID Yedekleme ve Yönetiminde Dikkatli Olun**: Yedek alırken ve geri yüklerken GTID_PURGED değerlerini doğru ayarlayın.
- **Replikasyon Kontrollerini Otomatize Edin**: Replikasyon hatalarını erken tespit etmek için düzenli izleme araçları kullanın.
- **Binlog’u Manipüle Etmekten Kaçının**: Manuel düzenlemeler veri tutarlılığını bozabilir.

Hatanın kaynağını doğru bir şekilde anlamak için detaylı log incelemesi yapmanız önemlidir. Eğer ek bir sorun veya açıklamaya ihtiyaç duyarsanız, log detaylarını paylaşabilirsiniz.

### HAProxy Konfigürasyonunu Kontrol Edin

HAProxy’nin `mode` parametresini ve düğümlere gönderdiği trafiği kontrol edin. Yanlış yapılandırma, aynı yazma işlemini birden fazla düğüme gönderebilir. Yazma işlemlerini yalnızca bir düğüme yönlendirmek için bir düğümü "primary" olarak belirleyin.

MySQL Galera Cluster ve HAProxy, yüksek erişilebilirlik ve yük dengeleme sağlayan güçlü bir kombinasyondur. Ancak, GTID hatası gibi sorunlarla karşılaşıldığında sistemin yapılandırmasını dikkatle incelemek ve doğru adımları atmak önemlidir.

# Önemli Hatırlatma!
Bu yazıda paylaşılan bilgiler, Galera Cluster ve HAProxy yapılandırma ve sorun giderme süreçlerine dair kendi deneyimlerim ve araştırmalarım doğrultusunda hazırlanmıştır. Bu rehberdeki çözümleri uygulamadan önce, sisteminize özgü yapılandırmayı detaylıca değerlendirmenizi öneririm.

Yukarıdaki işlemler, özellikle canlı sistemlerde uygulanırken dikkatli olunmalıdır. Yanlış bir adım, veri kaybına veya sistemde ciddi performans sorunlarına yol açabilir. Ayrıca, yedekleme yapılmadan herhangi bir sistemde değişiklik yapmanızı kesinlikle önermem.

Yazıda sunulan bilgiler, bir garanti veya profesyonel destek niteliği taşımaz. Bu işlemler sırasında oluşabilecek herhangi bir veri kaybı, hizmet kesintisi veya başka sorunlardan sorumlu tutulamayacağımı belirtmek isterim. Kritik sistemler için profesyonel bir uzman desteği almanız her zaman daha güvenli bir yaklaşımdır.

Unutmayın, her sistemin kendine özgü yapısı ve dinamikleri vardır. Uygulamadan önce bir test ortamında değişiklikleri denemeyi ihmal etmeyin.