---
layout: post
title: "MySQL CONCAT String Fonksiyonu Kullanımı"
date: 2017-05-21 17:57
categories: ["Yazılım", "Database"]
tags: ["mysql", "concat", "database-engine"]
---

Bu gün **MySQL CONCAT String Fonksiyonu Kullanımı** hakkında ufak bir bilgi vermek istiyorum. 

## Nedir bu concat? 
Mysql concat hücreleri birleştirmeye olanak sağlar. Concat fonsiyonu select sorguları içerisinde birden fazla alanı tek bir alana birleştirmemizi sağlayan basit bir komuttur. MySQL’de CONCAT deyimi de tıpkı + veya & operatörü gibi string türündeki verileri toplama veya diğer deyişle string verileri birleştirme işlemleri için kullanılır. 

Bazen sunucu taraflı çözümlerde, yapılacak işlemleri uygulamalara mantıklı bir şekilde dağıtmak gerekir. MySQL CONCAT Fonksiyonu da bu noktada basitçe işimizi görür. Bazen Ad ve Soyad sütunlarını ayırırız ve çıktıyı kullanmak için + yada & operatörlerini IIS/Apache tarafında kullanırız. Bu da çok kod, çok zaman demektir. Unutmayalım, iyi bir yazılımcı az kod kullanan yazılımcıdır :) 

Kullanımı oldukça basit bir string fonksiyonudur kendisi. Şimdi bunu örnek ile destekleyelim, kendiniz de biraz test yaparak daha aşina olabilirsiniz.

Küçük bir dip bilgi verelim; MySQL Nedir diye soranlar olabilir çünkü blogumda sürekli farklı içerikler üretiyorum. Vikipedi maddesinde çok güzel anlatmışlar;

> MySQL, altı milyondan fazla sistemde yüklü bulunan çoklu iş parçacıklı (İng. multi-threaded), çok kullanıcılı (İng. multi-user), hızlı ve sağlam (İng. robust) bir veri tabanı yönetim sistemidir.

Güzel bir tanımla MySQL Concat kullanımına geçiyoruz.

## MySQL CONCAT String Fonksiyonu Kullanımı
```sql
CONCAT(str1,str2,str3...)
```

MySql CONCAT fonksiyonu iki stringi birleştirir ve tek bir string yapar.

Örnek tablo aşağıdaki gibi olsun.

| ID | AD |SOYAD |
| 1 |Ahmet |Kaya |
| 2 |Veli |Taş |
| 3 |Zeliha |Güzel |

adi ve soyadı kolonlarını birleştirmek isteyelim:

```sql
SELECT ID, CONCAT(ADI,' ',SOYAD) AS AD_SOYAD FROM tbl_calisanlar
```

Çıktımız ise şu şekilde olur:

| ID | AD_SOYAD |
| 1 | Ahmet Kaya |
| 2 | Veli Taş |
| 3 | Zeliha Güzel |