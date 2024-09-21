---
layout: post
title: "Classic ASP ve SSL Kontrol, Yönlendirme"
date: 2018-03-22 10:17
categories: ["Yazılım", "Classic Asp"]
tags: ["classic asp", "asp", "ssl", "https", "secure-socket-layer"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/asp.jpg"
  alt: "Classic ASP ve Unix Time Stamp İşlemleri"
---

HTTPS protokolünün oluşturulma amacı güvensiz bir ağ ortamında güvenli bir iletişim yolu oluşturmaktır. Çünkü HTTP protokolü man-in-the-middle diye tabir edilen saldırılara açıktır. Bu tür saldırılarda ağı dinleyen saldırganlar, kişisel hesap bilgilerini ve parolaları kolay bir şekilde ele geçirebilirler.

Bu yüzden web sitelerimize SSL yükleriz ve bazen yazılımsal müdahaleler ile yönlendirmeler, SSL durumunun aktifliğini kontrol etmek gerekir. Bunun için Classic ASP de aşağıda vereceğim kodlar ile SSL kontrolünü yapabilir, gerekli durumda yönlendirme yaparak kullanıcıların tüm trafiği zorunlu olarak HTTPS protokolü üzerinden yapmasını sağlayabilirsiniz. ASP ve SSL kontrolü yapmak için vereceğim kodları kullanabilir, kendinize göre tekrar özelleştirebilirsiniz.

## SSL Yönlendirmesi Yapan Yeri Kontrol Edin
Biz kendimiz de yönlendirsek, olası ataklara ve kaçaklara karşı gerçekten kendimizin yönlendirdiğiniz kontrol edelim.

```javascript
<%
' #### Alan Adı Kontrol ve Yönlendirme
If InStr(Request.ServerVariables("HTTP_HOST"), "domain.com") = 0 Then
    response.redirect "https://" & Request.ServerVariables("HTTP_HOST")
    Response.End()
End If
%>
```

## Gelen Talebin Portuna Bakalım
HTTP protokolü genel olarak **80** veya **8080** portundan gerçekeleşir. HTTPS ise 443 numaralı portu kullanır. Bu yüzden gelen talebin 80 numaralı porttan olup olmadığını kontrol ediyoruz ve var olan tüm stringleri toplayıp, HTTPS protokolüne yönlendiriyoruz.

```javascript
<%
' #### SSL ve WWW KONTROLU
If request.servervariables("SERVER_PORT") = "80" Then
    Dim sQString
    sQString = Request.QueryString
    response.redirect "https://" & request.servervariables("SERVER_NAME") & request.servervariables("URL") & IIf(Len(sQString) > 0, "?" & sQString, "")
    Response.End()
End If
%>
```

## WWW Olmazsa Olmaz Diyorum
Takıntılıysanız WWW ekinin zorunlu kullanılmasını da sağlayabilirsiniz. Bunun için aşağıda ki kodu kullanmanız yeterli olacaktır. Gelen talebin başında WWW ön eki yoksa, ekleyip, mevcut adrese tekrar yönlendirecektir.

```javascript
<%
If InStr(1, request.servervariables("SERVER_NAME"), "www.") = 0 Then
    response.redirect "https://www." & request.servervariables("SERVER_NAME") & request.servervariables("URL") & "?" & sQString
    Response.End()
End If
%>
```