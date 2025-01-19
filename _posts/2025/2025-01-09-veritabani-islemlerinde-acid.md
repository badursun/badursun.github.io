---
layout: post
title: "Veritabanı İşlemlerinde ACID ve Row-Level Locking"
description: "Veritabanı İşlemlerinde ACID ve Row-Level Locking ne demek? Neden bu terimleri bilmeniz lazım?"
date: 2025-01-09 09:45
categories: ["Yazilim", "Database"]
tags: ["mysql", "myisam", "innodb", "database-engine", "acid", "row-level-locking", "rollback", "foreign-key-constraint", "atomicity", "consistency", "isolation", "durability"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/veritabani-islemlerinde-acid-ve-row-level-locking-678d2b74139c6.webp"
  alt: "Veritabanı İşlemlerinde ACID ve Row-Level Locking ne demek? Neden bu terimleri bilmeniz lazım?"
---

Veritabanı işlemleri, yazılım geliştirme sürecinde güvenilirlik ve tutarlılık sağlamak için kritik bir role sahiptir. Veriler üzerinde yapılan işlemlerin doğru şekilde işlenmesi, aynı anda çalışan birçok kullanıcıya cevap verebilmesi ve hata durumlarında veri kaybını önlemesi gerekir. İşte burada ACID özellikleri, Row-Level Locking, Rollback ve Foreign Key Constraint gibi kavramlar devreye girer. Bu yazıda, modern veritabanı sistemlerinde bu kavramların neden önemli olduğunu, nasıl çalıştığını ve pratik kullanım örneklerini ele alacağız.

ACID, bir veritabanı sisteminin özellikle **transaction (işlem)** işlemlerinde güvenilir, tutarlı ve dayanıklı olmasını sağlayan dört temel özelliği ifade eder. **InnoDB** gibi bir depolama motoru bu özellikleri sağlar. İşte ACID'in açıklaması:

## Atomicity (Atomiklik)
- **Tanım**: Bir işlem (transaction) ya **tamamen başarıyla tamamlanır** ya da **hiç başlamamış gibi iptal edilir**. Kısmi bir işlem gerçekleşmez.
- **Örnek**: Bir bankadan başka bir hesaba para transferi düşünün:
  1. Hesap A'dan 100 TL düşülmeli.
  2. Hesap B'ye 100 TL eklenmeli.
  - Eğer işlem sırasında bir hata olursa (örneğin, elektrik kesintisi), her iki adım da geri alınır ve veritabanı eski haline döner.
- **InnoDB**'de nasıl sağlanır:
  - `BEGIN` ve `COMMIT` arasında işlem yapılır.
  - Hata durumunda `ROLLBACK` ile tüm değişiklikler geri alınır.

## Consistency (Tutarlılık)
- **Tanım**: Bir işlem, veritabanını **geçerli bir durumdan başka bir geçerli duruma** taşımalıdır. İşlem sırasında veya sonunda veritabanında kuralları ihlal eden (örneğin, bir kısıtlama, yabancı anahtar) bir durum oluşmamalıdır.
- **Örnek**:
  - Hesapların toplam bakiyesi (A + B) bir işlemden önce ve sonra aynı kalmalıdır. Bir işlem başarısız olursa, veritabanı eski tutarlı durumuna döner.
- **InnoDB**'de nasıl sağlanır:
  - Yabancı anahtarlar (`FOREIGN KEY`), unique kısıtlamalar (`UNIQUE`), indeksler (`INDEX`) ve trigger'lar tutarlılığı korur.

## Isolation (Yalıtım)
- **Tanım**: Bir işlem diğer işlemlerden **tamamen izole edilir**. Aynı anda çalışan birden fazla işlem birbiriyle çakışmaz.
- **Örnek**:
  - Hesap A'dan para transferi yapılırken başka bir işlem Hesap A'nın bakiyesini görmeye çalışırsa, işlem ya bekletilir ya da önceki geçerli bakiyeyi görür.
- **InnoDB**'de nasıl sağlanır:
  - Farklı **izolasyon seviyeleri** ile kontrol edilir:
    1. **Read Uncommitted**: Diğer işlemler tarafından henüz tamamlanmamış (commit edilmemiş) değişiklikler görülebilir.
    2. **Read Committed**: Sadece commit edilmiş değişiklikler görülebilir.
    3. **Repeatable Read**: Aynı sorgu, bir işlem sırasında aynı sonuçları döndürür (standart InnoDB varsayılanı).
    4. **Serializable**: Tam izolasyon sağlar, işlemler birer birer çalışır.

## Durability (Dayanıklılık)
- **Tanım**: Bir işlem başarıyla tamamlandıktan sonra (commit edildikten sonra), bu işlem **herhangi bir sistem çökmesi durumunda bile kaybolmaz**.
- **Örnek**:
  - Bir banka işlemi başarıyla tamamlandıktan sonra (örneğin, bir transfer), elektrik kesintisi olsa bile işlem kalıcı olarak kaydedilir.
- **InnoDB**'de nasıl sağlanır:
  - **Write-Ahead Logging (WAL)** gibi mekanizmalar ile veriler önce log dosyasına yazılır, ardından disk üzerine işlenir.
  - `innodb_flush_log_at_trx_commit` parametresi, bu dayanıklılığı nasıl sağlayacağını kontrol eder:
    - `1`: Her commit sırasında log dosyasını diske yazar.
    - `0` veya `2`: Performansı artırabilir ama veri kaybı riski yaratabilir.

## Peki ACID neden kullanılmalıdır?
ACID özellikleri, özellikle finansal veya kritik uygulamalarda güvenilirliği sağlamak için önemlidir:

- **Atomicity**: "Ya hep ya hiç."
- **Consistency**: "Veritabanı her zaman kurallara uygun olmalı."
- **Isolation**: "Bir işlem diğerine karışmaz."
- **Durability**: "Tamamlanmış işlemler kaybolmaz."

**InnoDB**, bu özellikleri sağlayarak yüksek güvenilirlik ve veri bütünlüğü sunar.

---

Bir veritabanının güvenilirliği, kullanıcıların beklenmedik durumlarda bile doğru sonuçlar almasını sağlar. Örneğin, bir bankacılık uygulamasında aynı anda yüzlerce para transferi işlemi gerçekleşirken, işlemlerin birbirini etkilemeden gerçekleşmesi, işlem sırasında bir hata oluşursa yapılan değişikliklerin geri alınabilmesi ve her transferin eksiksiz kaydedilmesi gereklidir. ACID özellikleri, bu gibi durumlarda işlemleri güvence altına alır.

Buna ek olarak, Row-Level Locking gibi mekanizmalar, aynı anda birçok işlemin aynı tablo üzerinde paralel çalışmasını sağlayarak performansı optimize eder. Rollback ise hatalı veya istenmeyen durumlarda yapılan değişikliklerin geri alınması için hayati önem taşır. Foreign Key Constraint ise veri bütünlüğünü sağlamak ve tablolar arasındaki ilişkileri yönetmek için kullanılır.

## Row-Level Locking (Satır Seviyesi Kilitleme)

**Tanım**:  
Row-level locking, bir veritabanındaki yalnızca belirli bir satırı kilitleme işlemidir. Bu mekanizma, aynı tablo üzerindeki diğer satırların başka işlemler tarafından güncellenmesine veya okunmasına olanak tanırken, bir işlem sırasında yalnızca hedeflenen satırı kilitler.

**Avantajları**:
- **Daha Yüksek Paralellik**: Bir işlem yalnızca hedeflenen satırı kilitler, bu sayede diğer işlemler aynı tabloyu (başka satırları) etkileyebilir.
- **Özellikle Büyük Tablolarda Verimli**: Bir işlem, büyük bir tablodaki tek bir satırı etkilerken, tablo seviyesinde kilitleme yapmak yerine yalnızca ilgili satırı kilitlemek daha az kaynak tüketir.

**Örnek**:
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- Bu işlem sırasında sadece "id = 1" olan satır kilitlenir.
COMMIT;
```

**InnoDB'de Nasıl Sağlanır**:
- InnoDB, varsayılan olarak row-level locking kullanır. Ancak, bir sorgunun doğru çalışması için **index**'lerin uygun şekilde yapılandırılması gerekir. Aksi takdirde **table-level locking** devreye girebilir.

---

## Rollback (Geri Alma)

**Tanım**:  
Rollback, bir veritabanı işleminde yapılan değişiklikleri iptal ederek veritabanını işlem öncesindeki durumuna geri döndürme işlemidir. Bu, bir işlem sırasında bir hata oluştuğunda veya bir işlem isteyerek durdurulduğunda kullanılır.

**Avantajları**:
- **Veri Bütünlüğü Sağlama**: Yanlış veya eksik işlemlerin etkisi geri alınır.
- **Hata Toleransı**: Sistem, hata durumlarında veri kaybını önler.

**Örnek**:
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- Hata oluştuğunu düşünelim.
ROLLBACK;
-- Veritabanı değişiklikleri iptal edilir.
```

**InnoDB'de Nasıl Sağlanır**:
- InnoDB, rollback işlemini gerçekleştirmek için bir **undo log** kullanır. Her yapılan değişiklik, diskteki asıl verilere yazılmadan önce undo log'da saklanır. Eğer `ROLLBACK` çağrılırsa, undo log kullanılarak önceki durum geri yüklenir.

---

## Foreign Key Constraint (Yabancı Anahtar Kısıtlaması)

**Tanım**:  
Foreign key, bir tablodaki bir sütunun (veya sütun grubunun), başka bir tablodaki bir sütuna (veya sütun grubuna) referans verdiği bir kısıtlamadır. Bu, iki tablo arasındaki ilişkiyi belirlemek ve veri bütünlüğünü sağlamak için kullanılır.

**Amaçları**:
- **Veri Bütünlüğü Sağlama**: Bir tablodaki değerlerin yalnızca referans verilen tabloya uygun olması sağlanır.
- **Cascade Operasyonları**: İlgili verilerdeki değişikliklerin otomatik olarak yansıtılması (örneğin, silme veya güncelleme).

**Örnek**:
```sql
CREATE TABLE parent_table (
    id INT PRIMARY KEY
);

CREATE TABLE child_table (
    id INT PRIMARY KEY,
    parent_id INT,
    FOREIGN KEY (parent_id) REFERENCES parent_table(id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);
```

**Kullanım Açıklaması**:
- `ON DELETE CASCADE`: Eğer bir `parent_table` satırı silinirse, ona bağlı olan `child_table` satırları da otomatik olarak silinir.
- `ON UPDATE CASCADE`: Eğer `parent_table`'daki bir anahtar güncellenirse, bağlı `child_table`'daki değerler de güncellenir.

**InnoDB'de Nasıl Sağlanır**:
- InnoDB, **foreign key** desteği sunar ve yukarıdaki gibi kısıtlamalar uygulanabilir.
- Foreign key kısıtlaması, **referential integrity (ilişkisel bütünlük)** sağlar.
- Silme veya güncelleme sırasında hataları önlemek için `ON DELETE RESTRICT` veya `ON UPDATE RESTRICT` kullanılabilir.

## Row-Level Locking, Rollback ve Foreign Key Constraint'in InnoDB'de Kullanımı

- **Row-Level Locking**: Sadece ilgili satırı kilitleyerek paralel işlemleri optimize eder.
- **Rollback**: Bir işlemi geri alarak veri bütünlüğünü ve hata toleransını sağlar.
- **Foreign Key Constraint**: Tablolar arasındaki ilişkileri tanımlar ve veri bütünlüğünü zorunlu kılar. `CASCADE`, `RESTRICT` gibi seçeneklerle ilişkisel operasyonlar yönetilir.

Bu özellikler, **InnoDB** gibi modern depolama motorlarının güçlü yönlerindendir ve veritabanlarında veri bütünlüğü ile yüksek performansı bir arada sağlar.

InnoDB gibi modern veritabanı motorları, bu özelliklerin tümünü destekleyerek hem yüksek performans hem de veri güvenilirliği sunar.