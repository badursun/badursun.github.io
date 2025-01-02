---
layout: post
title: "Classic ASP ile Performans Optimizasyonu 1# Döngüler ve String Birleştirme"
description: "Döngülerde yapılan string birleştirme, farkında olmadan projelerinizi yavaşlatıyor olabilir. Bu yazıda, verimli yöntemlerle nasıl performans kazancı sağlanacağını tartışıyoruz."
date: 2025-01-02 09:45
categories: ["Classic ASP ile Performans Optimizasyonu"]
tags: ["performans", "optimizasyon", "vbscript", "classic-asp"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/classic-asp-ile-performans-optimizasyonu-6771b14e212fa.webp"
  alt: "Classic ASP ile Performans Optimizasyonu: Döngüler ve String Birleştirme"
---

Performans optimizasyonu, yazılım projelerinin maliyetini, ölçeklenebilirliğini ve kullanıcı deneyimini doğrudan etkiler. Özellikle Classic ASP gibi eski teknolojilerde doğru stratejiler uygulanmadığında, performans sorunları yazılımın genel verimliliğini ciddi şekilde düşürebilir.

20 yılı aşkın süredir Classic ASP ile çalışan bir yazılımcı olarak, bu platformda karşılaştığım ve çözdüğüm performans sorunlarını bir yazı serisi halinde paylaşmaya karar verdim. Bu serinin amacı, hala Classic ASP kullanan geliştiricilere pratik optimizasyon teknikleri sunmak ve yaygın hataları önlemektir. İlk yazımızda, en temel ve sık karşılaşılan konulardan biri olan "string birleştirme" yöntemlerini ele alacağız.

Bir yazılımın maliyetini belirleyen en büyük etkenlerden biri, veritabanı sorgularının, CPU ve bellek tüketiminin, disk I/O işlemlerinin etkili yönetimidir. Kötü optimize edilmiş sorgular, gereksiz string birleştirmeler veya bellek tüketimi gibi faktörler, sadece uygulamanın hızını değil, aynı zamanda barındırma maliyetlerini de artırır. Bu seride, bu tür sorunları nasıl önleyebileceğinize dair pratik çözümleri adım adım paylaşacağım.

İlk yazımızda, "Döngülerde String Birleştirme" konusuna odaklanıyoruz. Hadi başlayalım!

Classic ASP, günümüzde modern framework'lerin gölgesinde kalsa da, hala birçok projede aktif olarak kullanılıyor. Ancak, performans açısından doğru adımlar atmazsanız, özellikle büyük veri setlerinde veya yoğun döngülerde ciddi sıkıntılarla karşılaşabilirsiniz. "String birleştirme" gibi basit görünen ancak yanlış kullanıldığında performansı altüst eden bir konuya odaklanacağım. "Classic ASP ile Performans Optimizasyonu" yazı serisi için oluşturacağım kategoriden tüm yazıları okuyabilirsiniz.

Hadi o zaman başlıyoruz!

## Döngülerde String Birleştirme Problemi

Classic ASP'de string birleştirme, döngülerde sıkça yaptığımız bir işlem. Örneğin, bir HTML tablo oluştururken her satırı string olarak birleştirmek yaygın bir yöntemdir. Fakat performans'a ne kadar etkisi olduğunu düşünmeden fütursuzca kullandığımız, küçük projeler için veli-nimet olup aslında ciddi bir maliyet etkisi olan yöntemdir:

```javascript
Dim s
s = "<table>"
For Each fld in rs.Fields
    s = s & "<th>" & fld.Name & "</th>"
Next

While Not rs.EOF
    s = s & "<tr>"
    For Each fld in rs.Fields
        s = s & "<td>" & fld.Value & "</td>"
    Next
    s = s & "</tr>"
    rs.MoveNext
Wend
s = s & "</table>"
Response.Write s
```

Bu kod, işlevsel olarak doğru çalışır. Ancak, performans açısından bir felakettir. String birleştirme, her adımda bellekte yeni bir alan tahsis edilmesini gerektirir. Bu da veri miktarı ve tekrarı büyüdükçe işlem süresinin katlanarak artmasına neden olur. Üstelik asla cost calculation yapamazsınız. Özellikle büyük veri setlerinde bu yöntem, sayfanın yanıt süresini ciddi şekilde uzatabilir.

## Performans Açısından Doğru Yöntemler

Döngülerde string birleştirme yerine daha verimli yöntemlere odaklanarak performansı artırabilirsiniz. Aşağıda, farklı yöntemleri performans açısından karşılaştırarak ele alıyorum:

### 1. Tek Satırda Çoklu Birleştirme (`&` Operatörü)

```javascript
sqlQuery = "SELECT u.ID, u.Name, u.Email, o.OrderDate, o.Total " & _
           "FROM Users u " & _
           "INNER JOIN Orders o ON u.ID = o.UserID " & _
           "WHERE o.OrderDate >= '2023-01-01' AND u.Status = 'Active' " & _
           "ORDER BY o.OrderDate DESC"
```

Bu yöntem, performans açısından en hızlı olanıdır çünkü string birleştirme yalnızca bir kez yapılır. Ayrıca, kod daha okunaklıdır.

**Avantajları:**
- Kod okunabilirliği yüksek
- Hata ayıklama kolay
- IDE'lerde düzgün görüntülenir
- String yapısı korunur
- Performans açısından optimum

**Kullanım Alanları:**
- SQL sorguları
- HTML şablonları
- Uzun string birleştirmeleri
- Çok satırlı metinler

### 2. Satır Satır Birleştirme (`&` Operatörü)

```javascript
sqlQuery = "SELECT u.ID, u.Name, u.Email, o.OrderDate, o.Total "
sqlQuery = sqlQuery & "FROM Users u "
sqlQuery = sqlQuery & "INNER JOIN Orders o ON u.ID = o.UserID "
sqlQuery = sqlQuery & "WHERE o.OrderDate >= '2023-01-01' AND u.Status = 'Active' "
sqlQuery = sqlQuery & "ORDER BY o.OrderDate DESC"
```

Bu yöntem okunabilirliği artırabilir ancak her satırda birleştirme işlemi nedeniyle performansı düşürür. Bellekte gereksiz yeniden tahsis işlemleri yapılır.

**Dezavantajları:**
- Her birleştirmede yeni string nesnesi oluşur
- Bellek kullanımı yüksek
- Döngülerde performans problemi
- Uzun stringleri yönetmek zor

**Ne Zaman Kullanılmalı:**
- Kısa string birleştirmeleri
- Az sayıda değişken
- Performans kritik değilse

### 3. Satır Satır Birleştirme (`+` Operatörü)

```javascript
sqlQuery = "SELECT u.ID, u.Name, u.Email, o.OrderDate, o.Total "
sqlQuery = sqlQuery + "FROM Users u "
sqlQuery = sqlQuery + "INNER JOIN Orders o ON u.ID = o.UserID "
sqlQuery = sqlQuery + "WHERE o.OrderDate >= '2023-01-01' AND u.Status = 'Active' "
sqlQuery = sqlQuery + "ORDER BY o.OrderDate DESC"
```

Bu yöntem her ne kadar string birleştirme için 'çalışıyor' olsa da, Classic ASP ve VBScript'te kullanımı önerilmez çünkü `+` operatörü matematiksel toplama için de kullanılır. Hatalara ve yanlış sonuçlara yol açabilir. Performans açısından da `&` operatörüne göre daha yavaştır. Kullanımı hatalı ve risklidir.

**Neden Kaçınmalısınız:**
- `+` operatörü öncelikle matematiksel işlem için kullanılır
- String ve sayı karışımında beklenmeyen sonuçlar
- Tip dönüşümü hataları
- Kod okunabilirliği düşük
- Maintenance zorluğu

**Potansiyel Hatalar:**
```vbscript
' ÖRNEK 1: Beklenmeyen Sonuç
result = "5" + "3"     ' Sonuç: 8 (sayısal toplama)
result = "5" & "3"     ' Sonuç: "53" (string birleştirme)

' ÖRNEK 2: Hata Riski
userInput = Request.Form("number")  ' Kullanıcı "abc" girerse
result = userInput + 5             ' Hata: Type mismatch

## Daha Verimli Alternatifler

Döngülerde string birleştirme yerine, aşağıdaki yöntemlerle performansı artırabilirsiniz, ama burada kullanım amacınıza uygun yöntem seçmeniz gerekir:

### 1. **`Response.Write`** ile Doğrudan Çıktı

Döngü içinde birleştirme yapmak yerine, her bir satırı direkt olarak response buffer'a yazdırabilirsiniz:

```javascript
Response.Write "<table>"

For Each fld in rs.Fields
    Response.Write "<th>" & fld.Name & "</th>"
Next

While Not rs.EOF
    Response.Write "<tr>"
    For Each fld in rs.Fields
        Response.Write "<td>" & fld.Value & "</td>"
    Next
    Response.Write "</tr>"
    rs.MoveNext
Wend

Response.Write "</table>"
```

**Ne Zaman Kullanılmalı?**

- Küçük veya orta ölçekli veri setlerinde.
- Dinamik HTML çıktısı oluştururken basit ve hızlı bir çözüm gerektiğinde.

### 2. **`GetRows`** Kullanarak Diziye Dönüştürme

Recordset'i bir diziye çevirerek veriyle işlem yapmak, hem performansı artırır hem de kodun daha okunabilir olmasını sağlar:

```javascript
Dim arrData, i, j
arrData = rs.GetRows()

Response.Write "<table>"

' Başlıkları yaz
For i = 0 To UBound(arrData, 1)
    Response.Write "<th>" & rs.Fields(i).Name & "</th>"
Next

' Satırları yaz
For j = 0 To UBound(arrData, 2)
    Response.Write "<tr>"
    For i = 0 To UBound(arrData, 1)
        Response.Write "<td>" & arrData(i, j) & "</td>"
    Next
    Response.Write "</tr>"
Next

Response.Write "</table>"
```

**Ne Zaman Kullanılmalı?**

- Büyük veri setleri üzerinde işlem yaparken.
- Recordset ile sık sık veri erişimi gerektiğinde performans avantajı sağlar.

## Best Practices ve Öneriler

1. **String Birleştirme Kuralları:**
   - Uzun stringler için her zaman `& _` kullanın
   - Kısa birleştirmelerde `&` operatörünü tercih edin
   - `+` operatöründen kaçının

2. **Performans İpuçları:**
   - Döngülerde string birleştirmeden kaçının
   - Uzun SQL sorgularını satırlara bölün
   - String uzunluğunu önceden tahmin edin

3. **Kod Okunabilirliği:**
   - Her satırı aynı hizaya getirin
   - Mantıksal grupları boşluk ile ayırın
   - Tutarlı girinti (indentation) kullanın

## Microsoft'un Önerileri

Microsoft, string birleştirme konusunda şu tavsiyelerde bulunuyor:

- Döngülerde string birleştirme yapmaktan kaçının. String birleştirme, her işlemde bellekte yeniden tahsis gerektirir ve bu işlem zaman alır.
- Büyük veri setleri için `Response.Write` veya `GetRows` yöntemlerini tercih edin.
- SQL sorguları için stored procedure kullanmayı düşünün.
- VBScript'te string birleştirme için `+` yerine `&` operatörünü kullanın.

Performans optimizasyonu, genellikle "küçük detaylar" üzerinde yoğunlaşır. Bu küçük detaylar, proje büyüdükçe kocaman ihmallere sebep verebilir. Yazılımcı olmak demek sadece yazılımı çalışan bir yazılımcı olmak demek değildir. Bu işin hesaplaması oldukça ince bir iş ve önemli bir detaydır.

Daha verimli alternatifler kullanarak, uygulamanızın yanıt süresini ve ölçeklenebilirliğini önemli ölçüde artırabilirsiniz. Bu yazı, Classic ASP ile performans optimizasyonu serimizin ilk yazısıydı. Serinin devamında başka kritik konuları ele alacağız...
