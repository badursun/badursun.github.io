---
layout: post
title: "reCAPTCHA ve Classic ASP Entegrasyonu (Doğrulama)"
date: 2018-10-15 12:55
categories: ["Yazılım", "Classic Asp"]
tags: ["google", "classic-asp", "asp", "recaptcha", "functions", "fonksiyonlar"]
toc: true
---

Classic ASP ile yazılmış web sitelerinize reCAPTCHA ve Classic ASP Entegrasyonu nasıl yapacaksınız? En hızlısından çözüme gidiyoruz. Mevcut sistmeinize entegre edecekseniz yapınızda ki değişiklikleri uyarlamanız gerekecek fakat endişe edecek birşey yok, çünkü her ne kadar "klasik asp öldü" deselerde, doğru kod yapısı ile herşeyi entegre edebilir, kullanabilirisiz. 

Özenli bir kod yazıyorsanız, performans endişeniz de olmaz. Hadi o zaman entegrasyona başlayalım.

> Bu yazı en basit kullanım şekliyle anlatılmıştır. Daha profesyonel bir şekilde geliştirmek için basit mantığı anlamanıza yardımcı olması için yazılmıştır.

Önce reCAPTCHA için kayıt olmanız ve kullanacağınız web sitesini tanıtmanız gerekiyor. Bunun için [https://www.google.com/recaptcha/](https://www.google.com/recaptcha/) adresini ziyaret ediyoruz.

Bu noktada bilmeniz gereken 2 önemli anahtar var.

- **Site-Key**: Bu herkes tarafından görülebilen bir koddur. Paylaşmanızda sakınca yoktur. Zaten kaynak kodunuzdan erişilebilir oluyor.
- **Secret-Key**: Bu ise verifikasyon yaparken kullanacağınız gizli bir anahtardır. Sadece yazılımda gömülü olmalıdır. Aşağıda fonksiyonun üstünde belirttiğim alana yazıp, gömülü bırakacaksınız.

Bu iki veriyi, yukarıda belirttiğim adresten kayıt işlemlerinizi tamamladıktan sonra ulaşabileceksiniz. Devam etmeden önce bu bilgileri almanız süreci kolaylaştırır.

Önce formumuza ve html kodlarımıza eklememiz gereken bazı veriler var.

## reCAPTCHA ve Classic ASP Entegrasyonu
Önce **<head> ve </head>** arasına

```html
<script src="https://www.google.com/recaptcha/api.js"></script>
```

scriptini ekliyoruz. Bu ilgili div içine reCaptcha sistemini çağıracak böylece reCAPTCHA ve Classic ASP Entegrasyonu için ilk adımları atmış olacaksınız.

Daha sonra **<form> ve </form>** arasında uygun bir yere

```html
<div class="g-recaptcha" data-sitekey="API-SITE-KEY"></div>
```

div elementini ekliyoruz. Bu element, eklediğimiz script tarafından doldurulacak. Eğer SITE-KEY kısmında hata alırsanız burada gösterilecek.

### Doğrulama Fonksiyonu

```javascript
<%
ChaptaSecret = "API-SEVRET-KEY"

Function recaptchaVerify(TheCode)
    If Request.ServerVariables("REQUEST_METHOD") = "POST" Then
        If Len(ChaptaSecret) < 10 Then
            Response.Write "recaptchaVerify() Hatalı Secret Key" 
            recaptchaVerify = False
            Exit Function
        End If

        If Len(TheCode) < 50 Then
            Response.Write "recaptchaVerify() Doğrulanacak Kod İletilmedi"
            recaptchaVerify = False
            Exit Function
        End If

        dataParam = "remoteip="& Request.ServerVariables("REMOTE_ADDR") &"&secret="& ChaptaSecret &"&response="& TheCode
        Set httpGoogle = Server.CreateObject("Msxml2.ServerXMLHTTP.6.0")
            httpGoogle.open "GET", "https://www.google.com/recaptcha/api/siteverify?"& dataParam &"", false
            httpGoogle.setOption(2) = SXH_SERVER_CERT_IGNORE_ALL_SERVER_ERRORS
            httpGoogle.setRequestHeader "Content-Length", Len(dataParam)
            httpGoogle.setRequestHeader "Content-type", "text/json; charset=utf-8"
            httpGoogle.send
        strStatus = httpGoogle.Status
        strRetval = httpGoogle.responseText
        Set httpGoogle = nothing
        
        Set recaptchaJson = JSON.parse(join(array( strRetval )))
            If Err <> 0 Then
                Response.Write "recaptchaVerify() Google ile İletişim Kurulamadı "
                recaptchaVerify = False
                Exit Function
            End If
        If recaptchaJson.success = True Then
            recaptchaVerify = True
        Else
            Response.Write "recaptchaVerify() Doğrulanamadı !"
            recaptchaVerify = False
            Exit Function
        End If
    End If
End Function
%>
```

### Fonksiyonun Kullanımı

```javascript
<%
If recaptchaVerify( Request.Form("g-recaptcha-response") ) = False Then 
    Response.Write "Güvenlik Kodu Hatalı. Please Validate You Are Human <a href='#'>Geri Dön / Go Back</a>"
    Response.End 
End If
%>
```

Kullanmanız gereken bir kütüphane daha var o da ASP2Json kütüphanesi. İlgili kütüphaneye [buraya]({{ site.baseurl }}{% post_url 2017/2017-11-06-www-asptojson-com-wayback-machine %}) tıklayıp ulaşabilirsiniz. Google'a gönderdiğiniz kodu verify etmek için dönen cevap json formatında olduğu için, ilgili cevaptan verileri okumanız gerekiyor. Eğer bu kütüphaneyi eklemezseniz yazılım size hata döndürecektir. Kodu kendi yapınıza göre geliştirebilirsiniz.

Eğer bir de jQuery ile ön validasyon yapacağım diyorsanız yukarıda eklediğimiz div içerisinde bir parametre daha olmalı

```html
<div class="g-recaptcha" data-callback="recaptchaCallback" data-sitekey="BURAYA SITE-KEY GELECEK"></div></pre>
```

ve tabii bir de javascript kodumuz var;
```javascript
function recaptchaCallback() {
  // Ön doğrulama tamamlandığında yapılacak işlemler
  $("#GonderButonu").prop("disabled", false);
};
```

Örnek olarak bir fonksiyon verdim, bu fonksiyon ile captcha ön doğrulama yapıldığında tetikleniyor, bir #GonderButonu ID sine sahip butonum var, disabled pozisyondayken bunu tekrar kullanılabilir hale getiriyorum. Ve submit işlemine izin veriyorum. Varyasyonları istediğiniz gibi geliştirebilirsiniz.