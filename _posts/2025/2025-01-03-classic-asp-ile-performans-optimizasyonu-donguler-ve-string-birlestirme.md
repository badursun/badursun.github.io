---
layout: post
title: "Classic ASP ile Performans Optimizasyonu 1# Döngüler ve String Birleştirme"
description: "Döngülerde yapılan string birleştirme, farkında olmadan projelerinizi yavaşlatıyor olabilir. Bu yazıda, verimli yöntemlerle nasıl performans kazancı sağlanacağını tartışıyoruz."
date: 2025-01-03 09:45
categories: ["Classic ASP ile Performans Optimizasyonu"]
tags: ["performans", "optimizasyon", "vbscript", "classic-asp"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/classic-asp-ile-performans-optimizasyonu-6771b14e212fa.webp"
  alt: "Classic ASP ile Performans Optimizasyonu: Döngüler ve String Birleştirme"
---

Performans optimizasyonu, yazılım projelerinin maliyetini, ölçeklenebilirliğini ve kullanıcı deneyimini doğrudan etkiler. Özellikle Classic ASP gibi eski teknolojilerde doğru stratejiler uygulanmadığında, performans sorunları yazılımın genel verimliliğini ciddi şekilde düşürebilir. Bu yazı serisinde, Classic ASP ile yazılım geliştirirken yapılan yaygın hataları ve performansı artırmanın yollarını ele alacağız.

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

### 2. Satır Satır Birleştirme (`&` Operatörü)

```javascript
sqlQuery = "SELECT u.ID, u.Name, u.Email, o.OrderDate, o.Total "
sqlQuery = sqlQuery & "FROM Users u "
sqlQuery = sqlQuery & "INNER JOIN Orders o ON u.ID = o.UserID "
sqlQuery = sqlQuery & "WHERE o.OrderDate >= '2023-01-01' AND u.Status = 'Active' "
sqlQuery = sqlQuery & "ORDER BY o.OrderDate DESC"
```

Bu yöntem okunabilirliği artırabilir ancak her satırda birleştirme işlemi nedeniyle performansı düşürür. Bellekte gereksiz yeniden tahsis işlemleri yapılır.

### 3. Satır Satır Birleştirme (`+` Operatörü)

```javascript
sqlQuery = "SELECT u.ID, u.Name, u.Email, o.OrderDate, o.Total "
sqlQuery = sqlQuery + "FROM Users u "
sqlQuery = sqlQuery + "INNER JOIN Orders o ON u.ID = o.UserID "
sqlQuery = sqlQuery + "WHERE o.OrderDate >= '2023-01-01' AND u.Status = 'Active' "
sqlQuery = sqlQuery + "ORDER BY o.OrderDate DESC"
```

Bu yöntem her ne kadar string birleştirme için 'çalışıyor' olsa da, Classic ASP ve VBScript'te kullanımı önerilmez çünkü `+` operatörü matematiksel toplama için de kullanılır. Hatalara ve yanlış sonuçlara yol açabilir. Performans açısından da `&` operatörüne göre daha yavaştır.

## Daha Verimli Alternatifler

Döngülerde string birleştirme yerine, aşağıdaki yöntemlerle performansı artırabilirsiniz, ama burada kullanım amacınıza uygun yöntem seçmeniz gerekir:

### 1. **`Response.Write`**\*\* ile Doğrudan Çıktı\*\*

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

### 2. **`GetRows`**\*\* Kullanarak Diziye Dönüştürme\*\*

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

### 3. **Stored Procedure ile İşlem Yapma**

SQL sorgularını uygulama kodundan bağımsız hale getirerek performansı artırabilirsiniz. Stored procedure kullanımı, hem sorgu optimizasyonu sağlar hem de kodun daha temiz olmasını sağlar:

```javascript
Dim cmd, rs
Set cmd = Server.CreateObject("ADODB.Command")
Set cmd.ActiveConnection = conn
cmd.CommandText = "GetUserOrders"
cmd.CommandType = adCmdStoredProc

Set rs = cmd.Execute()

' Burada döngü ile response işlemleri yapılabilir.
```

**Avantajlar:**

- Sorgu optimizasyonu veritabanı seviyesinde yapılır.
- Tekrar eden işlemler için performans avantajı sağlar.

**Dikkat Edilmesi Gerekenler:**

- Stored procedure'ler, özellikle yoğun trafikli sistemlerde, veritabanı kaynaklarının tüketimini artırabilir. Ancak, doğru şekilde optimize edilirse genellikle bu maliyet düşüktür ve daha büyük performans kazancı sağlar.

## Microsoft'un Önerileri

Microsoft, string birleştirme konusunda şu tavsiyelerde bulunuyor:

- Döngülerde string birleştirme yapmaktan kaçının. String birleştirme, her işlemde bellekte yeniden tahsis gerektirir ve bu işlem zaman alır.
- Büyük veri setleri için `Response.Write` veya `GetRows` yöntemlerini tercih edin.
- SQL sorguları için stored procedure kullanmayı düşünün.
- VBScript'te string birleştirme için `+` yerine `&` operatörünü kullanın.

Performans optimizasyonu, genellikle "küçük detaylar" üzerinde yoğunlaşır. Bu küçük detaylar, proje büyüdükçe kocaman ihmallere sebep verebilir. Yazılımcı olmak demek sadece yazılımı çalışan bir yazılımcı olmak demek değildir. Bu işin hesaplaması oldukça ince bir iş ve önemli bir detaydır.

Daha verimli alternatifler kullanarak, uygulamanızın yanıt süresini ve ölçeklenebilirliğini önemli ölçüde artırabilirsiniz. Bu yazı, Classic ASP ile performans optimizasyonu serimizin ilk yazısıydı. Serinin devamında başka kritik konuları ele alacağız...

