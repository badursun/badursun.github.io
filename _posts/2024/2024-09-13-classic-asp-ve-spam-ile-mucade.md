---
layout: post
title: "Classic ASP ve Spam ile Mücadele"
date: 2024-09-13 14:39
categories: ["Yazilim", "Classic Asp"]
tags: ["asp", "function", "spam"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/classic-asp-ve-spam-ile-mucade-66eea94a525e7.webp"
  alt: "Classic ASP ve Spam ile Mücadele"
---

Günümüzde spam bot'ları web sitelerini hedef alarak çeşitli saldırılar gerçekleştiriyor. Özellikle e-posta adreslerini ve diğer kişisel bilgileri toplamak için kullanılan register formlarında bu saldırılar daha sık görülüyor. Spam'cılar, bu formlara manipüle edilmiş veriler girerek, sistemleri kötüye kullanabiliyor ve hedef e-posta adreslerine spam e-postalar gönderebiliyor. Bu tür saldırılara karşı koruma sağlamak, özellikle güvenlik açıklarını kapatmak adına kritik önem taşıyor.

## Sorun ve Çözüm
Son birkaç gün içinde, sistemlerimde yaşadığım bir olay, bu tür saldırıların ne kadar yaygın olduğunu ve ne tür etkiler yaratabileceğini gösterdi. Sunucularıma ait veri merkezi (DC) firması, IP adresimden gelen yüksek bir şikayet oranı nedeniyle uyarı e-postası gönderdi. 

    Date: Fri, Sep 13, 2024 at 11:31 AM
    Subject: [*****umine #23***2] High SNDS complaint rate on your IP ***.**.****.102
    To: John Doe <info[a]my-company.mail>
    Cc: <ba****un[a]gmail.com>


    Subject: High SNDS complaint rate on your IP ***.**.****.102
     Hello!

    We have been notified by SNDS that your IP ***.**.****.102 has been sending out email traffic with a complaint rate of 11%, which is very frowned upon. A note on the report listing also mentions the following: "Abuse reported for: noreply[a]your-domain.com".  Please take action to stop this harmful activity.

    Lugupidamisega / Best regards / С уважением,
    A******* T******

Gönderilen e-postada, e-posta trafiğiyle ilgili bir şikayet oranının yüksek olduğu ve kötüye kullanım raporunun bulunduğu belirtiliyordu. Bu, spam saldırılarının etkili bir şekilde nasıl gizlenebileceğinin ve ne tür sonuçlar doğurabileceğinin bir örneğiydi.

Yapılan analizler sonucunda, sistemlerin normal hareketlerinin aslında spam saldırılarından etkilendiği ve bu hareketlerin normal gibi göründüğü ortaya çıktı. Bu durumu fark ettikten sonra, sistemlere yönelik bir yama hazırladım. Bu yamanın amacı, spam bot'larının kelime oyunları yaparak form verilerini manipüle etmelerini engellemektir.

## Classic ASP İçin Spam Koruma Fonksiyonu
Aşağıda, Classic ASP ile geliştirdiğim ve spam saldırılarını önlemeye yönelik bir fonksiyon örneğini bulabilirsiniz. Bu fonksiyon, olası spam saldırılarında kullanılan kelime oyunlarını tespit eder ve engeller. Sonuç olarak bir array döner, array'ın ilk parçası true ise, işlemi durdurabilirsiniz. Array'ın ikinci parçası, ihtiyaç halinde incelenen string'in hangi sebeple şüpheli olduğunu söyler.

Ben bu fonksiyonu register formlarında kullanıcı adı, soyadı gibi alanlar için kullandım. Parolalar zaten şifreleniyor ve e-posta için hem pattern hemde yasaklı provider listesi sunuyorum. 


```javascript
Function SuspectCheck(ByVal text)
    SuspectCheck = Array(False, "")

    Set htmlRegex = New RegExp
        htmlRegex.IgnoreCase    = True
        htmlRegex.Global        = True
        htmlRegex.Pattern       = "<[^>]+>" ' HTML tag pattern
        If htmlRegex.Test(text) Then 
            SuspectCheck = Array(True, "isHtml")
            Exit Function
        End If
    Set htmlRegex = Nothing

    Set urlRegex = New RegExp
        urlRegex.IgnoreCase     = True
        urlRegex.Global         = True
        ' urlRegex.Pattern        = "((mailto|tel|ftp[s]|http[s])?://[^\s]+)" ' Web URL pattern
        urlRegex.Pattern        = "(http[s]?|ftp|mailto|tel|www)?(:\/\/)?[a-zA-Z0-9\-]+\.[a-zA-Z]{2,}([\/\?\=].*)?"
        If urlRegex.Test(text) Then 
            SuspectCheck = Array(True, "isUrl")
            Exit Function
        End If
    Set urlRegex = Nothing

    Set emailRegex = New RegExp
        emailRegex.IgnoreCase   = True
        emailRegex.Global       = True
        emailRegex.Pattern      = "\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}\b" ' E-posta pattern (case insensitive)
        If emailRegex.Test(text) Then 
            SuspectCheck = Array(True, "isEmail")
            Exit Function
        End If
    Set emailRegex = Nothing

    Set webRegex = New RegExp 
        webRegex.Global = true
        webRegex.IgnoreCase = true
        webRegex.Pattern = "((((ht|f)tps?://)|www\.)\S+[^\.*](\s)?)"
        If webRegex.Test(text) Then 
            SuspectCheck = Array(True, "isWeb")
            Exit Function
        End If
    Set webRegex = Nothing

    Set nonSpecialRegex = New RegExp 
        nonSpecialRegex.Global = true
        nonSpecialRegex.IgnoreCase = true
        nonSpecialRegex.Pattern = "(--|<|>|'|(1=1)|/\*|\*/|//|%[0-9a-zA-Z]{2}|&#[0-9a-zA-Z]{2,3};?|(src|\(|\)|prompt|onerror|img|iframe)|((""+.*(script)+.*""+){1}))"
        If nonSpecialRegex.Test(text) Then 
            SuspectCheck = Array(True, "isSpecial")
            Exit Function
        End If
    Set nonSpecialRegex = Nothing
End Function
```

