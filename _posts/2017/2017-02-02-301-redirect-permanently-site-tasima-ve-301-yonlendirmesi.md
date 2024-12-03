---
layout: post
title: "301 Redirect Permanently - Site Taşıma ve 301 Yönlendirmesi"
date: 2017-02-02 10:29
categories: ["Yazilim", "Classic Asp"]
tags: ["classic asp", "asp", "301-redirect", "redirect"]
toc: true
---

Yine bir gün baktım projenin performansı alan adı tarafından engelleniyor, çok akıllıca bir alan adı almadığıma karar verdim. İşte o anda, indexlenmiş içerikleri kaybetmeden, mevut ziyaretçileri ürkütmeden projemi yeni alan adına nasıl taşırım diye düşündüm. 

404 yapısında tüm linkler ölmemeli, Google da ki potansiyel ziyaretçim kaybolmamalıydı. İşte bu noktada size yardımcı olacak bir kaç nokta var. İlki "301 Redirect Permanently" yani "301 Kalıcı Olarak Taşındı Yönlendirmesi", arama motorlarına ilgili sayfanın artık başka bir adrese taşındığını, ikametinin değiştiğini söylemenize yarayan header bilgisi. İkincisi ise Google bizim için çok önemli olduğundan, Webmaster Tools altında bulunan "site adres değişikliği" bölümü.

Sitenizi Google Webmaster Tools'da kayıt ettiyseniz, takibini yapıyorsunuz demektir. Yeni adresinizi ve eski adresinizi aynı şekilde kayıt edip, doğrulama işlemlerini yaptıysanız işte taşıma işlemine şimdi başlayabilirisiniz.

## Site Adresi Değiştirme/Taşıma
İlk önce Webmaster Tools da eski adresinizi seçin, giriş yapın, sağ üst çark ile **Adres Değişikliği** butonuna tıklayın.

Açılan sayfada 4 adımlık bir işlem listesi ile karşılaşacaksınız. Bu adımları dikkatlice tamamlayın, her adımda bir kontrol mekanizması mevcut. Böylece işlem sürecinin doğru ilerlediğini denetleyebiliyorsunuz.

2 Madde de işte 301 yönlendirmesi devreye giriyor. Ben bu noktada size Klasik ASP için hazırladığım bir örnek kodu vereceğim. Özellikle 404 yapısı kullanıyorsanız ve açılan sayfanızın tam olarak aynı adres uzantısı ile yeni web adresine yönlenmesini ve bunun dinamik yapıda otomatik olmasını istiyorsanız kod örneğim işinize yarayacaktır.

Bu kodu 2 sayfada konumlandırın.

- 404.asp
- default.asp

Böylece eski sitenizi ziyaret edenler farkına bile varmadan yeni adresinizde bulunan sayfaya ulaşacaktır. Ayrıca arama motorları bu yönlendirmeyi fark edeceği için hem arama motoru listelemelerinde ki sıranızı koruyacaksınız hemde kısa zamanda yeni alan adınız eskisi ile değişmiş olacaktır.

Bu süreci güçlendirmek adına webmaster tools altında ki tüm uygulamaları kullanın. Sitemap.xml gönderin, manuel url eklemeyi, Google gibi getir özelliğini mutlaka yapın.

## 404 Yönlendirmeli ASP Sitelerinde 301 Redirect Permanently Yönlendirme

```javascript
<%
Function RedirectPermanent()
    socket = "http://"
    If Request.ServerVariables("HTTPS") = "on" then 
        socket = "https://"
    End if 

    sTemp = Request.Querystring
    sTemp = replace(sTemp, "http://" & Request.ServerVariables("HTTP_HOST") & ":80/", "")
    sTemp = replace(sTemp, "http://" & Request.ServerVariables("HTTP_HOST") & "/", "")
    sTemp = replace(sTemp, "https://" & Request.ServerVariables("HTTP_HOST") & "/", "")
    sTemp = replace(sTemp, "https://" & Request.ServerVariables("HTTP_HOST") & ":443/", "")
    sTemp = socket & Request.ServerVariables("server_name")& "/" & sTemp

    sTemp = Replace(sTemp, "404;", "", 1, -1, 1)
    sTemp = Replace(sTemp, "mevcut-adres.com", "yeni-adres.com")
    getURL = sTemp
End Function

Response.Status="301 Moved Permanently"
Response.AddHeader "Location",""& RedirectPermanent() &""
%>
```