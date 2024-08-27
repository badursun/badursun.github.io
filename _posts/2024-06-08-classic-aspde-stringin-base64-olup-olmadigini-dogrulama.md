---
layout: post
title: "Classic ASP'de stringin Base64 olup olmadığını doğrulama (validate)"
date: 2024-06-08 12:42
categories: ["Yazılım", "Classic Asp"]
tags: ["base64", "classic-asp", "asp", "validate", "functions", "fonksiyonlar", "regex"]
---

Bazen Classic ASP dilinde basit işlemleri yapmak zorlaşabiliyor fakat yazılımcı birisi her zaman bir yol bulur. Bu noktada önemli olan yegane şey, en performanslı yolu bulmak.

İşte küçük veriler için oldukça performanslı bir doğrulama yöntemi. Kesinlikle %100 başarılı diyemeyiz çünkü basit bir regex ile pattern aramaktan öteye geçmiyor. Dolayısıyla, string'iniz bir base64 pattern'ine sahipse size true veya false dönecektir. Bu veri kümesinin gerçekten base64 ile encode edildiği anlamına gelmez.

Daha kesinlike bir doğrulama için server-side script ve error management kullanarak doğrulama yapmalısınız ama bu işlem daha fazla kaynak tüketeceği için, ilk adım olarak string'in gerçekten bir base64 pattern'i taşıyıp taşımadığını kontrol etmek olası riskleri azaltacaktır.

Aşağıdaki hazırladığım fonksiyon ile bu ön adımı gerçekleştirebilirsiniz.

```javascript
<%
Function IsBase64(byVal str)
    Dim regex, match, isValid

    ' Base64 regex pattern
    Set regex = New RegExp
    regex.Pattern = "^([A-Za-z0-9+/]{4})*([A-Za-z0-9+/]{3}=|[A-Za-z0-9+/]{2}==)?$"
    regex.IgnoreCase = True
    regex.Global = False

    ' Check if the string matches the base64 pattern
    Set match = regex.Execute(str)
    If match.Count > 0 Then
        isValid = True
    Else
        isValid = False
    End If

    ' Clean up
    Set match = Nothing
    Set regex = Nothing

    IsBase64 = isValid
End Function
%>
```