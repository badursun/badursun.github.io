---
layout: post
title: "Classic ASP ve Unix Time Stamp İşlemleri"
date: 2018-02-18 17:57
categories: ["Yazilim", "Classic Asp"]
tags: ["classic asp", "asp", "unixtimestamp", "functions"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/asp-66eea92b39262.webp"
  alt: "Classic ASP ve Unix Time Stamp İşlemleri"
---

Unix timestamp, 1 Ocak 1970 UTC tarihinden itibaren geçen saniye sayısını ifade eden bir zaman formatıdır. Bu tarih, "Unix epoch" olarak adlandırılır. Timestamp, birçok sistemde tarih ve zamanı depolamak veya karşılaştırmak için kullanılır çünkü hesaplanması ve işlenmesi kolaydır.

Daha çok evrenselleştirilmiş makine saat birimidir. İçerisinde gün, ay, yıl, saat, dakika, saniye, salise, saat fark bilgisi, lokasyon gibi kodlanmış bilgileri barındırır. Classic asp ile bu birimleri çevirmek, düzenlemek zor olabilir. 

Bunun için yardımcı bir fonksiyona ihtiyaç vardır. İşte ihtiyacınız olan ve classic asp ile oluşturacağınız Kod Kütüphanenizde bulunması gereken önemli bir fonksiyon.

```javacscript
Function ConvertToUnixTimeStamp(ByVal input_datetime)
    If IsDate(input_datetime) = false Then 
        ConvertToUnixTimeStamp = 0
        Exit Function
    End If

    Dim d : d = CDate(input_datetime)
    ConvertToUnixTimeStamp = CStr(DateDiff("s", "01/01/1970 00:00:00", d))
End Function

Function ConvertUnixTimeStampToDateTime(ByVal input_unix_timestamp)
    Dim DateUnix
        DateUnix = "" & input_unix_timestamp

    If IsNumericalAndNotZero(DateUnix) = False Then
        ConvertUnixTimeStampToDateTime = "Null"
        Exit Function
    End If

    If Len(DateUnix) > 10 Then 
        DateUnix = Left(DateUnix, 10)
    End If

    Dim NewDate 
        NewData = DateAdd("s", Csng(DateUnix), CDate("01/01/1970 00:00:00"))

    ConvertUnixTimeStampToDateTime = NewData
End Function

Function UTC03(ByVal date)
    UTC03 = DateAdd("h", 3, CDate(date))
End Function
```
> Son Güncelleme: 21 Eylül 2024