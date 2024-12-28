---
layout: post
title: "Classic ASP ile Ortam Değişkenleri: Performans ve Kullanım Kılavuzu"
description: "Classic ASP ile sistem ve kullanıcı ortam değişkenlerini nasıl kullanacağınızı öğrenin. Ortam değişkenlerinin performans etkileri, Application nesnesi ile kıyaslaması, avantaj ve dezavantajları..."
date: 2024-12-11 09:30
categories: ["Yazilim", "Classic Asp"]
tags: ["asp", "application", "environment", "shell", "wscript", "asp-session", "asp-application", "asp-environment", "performance"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/classic-asp-ile-ortam-degiskenleri-performans-metrikleri-675c103c136dc.webp"
  alt: "Classic ASP ile Ortam Değişkenleri: Performans ve Kullanım Kılavuzu"
---

Bu yazıda, Classic ASP ortamında `Environment`, `Application` ve `Session` nesnelerinin nasıl kullanılacağını, bu yapıların avantajlarını, dezavantajlarını ve performans etkilerini detaylı bir şekilde ele alacağım.

Classic ASP, web geliştirme teknolojileri arasında köklü bir yere sahiptir. Geliştiriciler genellikle uygulama verilerini saklamak ve paylaşmak için ağırlıklı olarak `Session`, `Application` nesnesi veya çok nadir olarak `Environment` değişkenlerini tercih ederler.

# Session Nesnesi

**Session** nesnesi, kullanıcı bazlı veri saklamak için kullanılır. Her kullanıcı için ayrı bir alanda çalışır ve oturum sona erdiğinde veriler silinir.

## Session Nesnesi Kullanma

### Değişken Oluşturma

```javascript
<%
Session("KullaniciAdi") = "Burak"
Response.Write "Hoşgeldiniz, " & Session("KullaniciAdi")
%>
```

### Oturum Süresini Ayarlama

```javascript
<%
Session.Timeout = 30
%>
```

## Avantajları ve Dezavantajları

| Avantajlar | Dezavantajlar |
| ------------------ | ------------------ |
| Kullanıcıya özgü veri saklama imkanı sağlar. | Kullanıcı başına bellek tüketimi artar. |
| Veriler dinamik olarak güncellenebilir. | Oturum sona erdiğinde veriler kaybolur.|

# Application Nesnesi

**Application** nesnesi, uygulama düzeyinde veri saklamak ve paylaşmak için kullanılır. Tüm kullanıcılar için ortak bir alanda çalışır ve veriler sunucu kapanana kadar saklanır.

## Application Nesnesi Kullanma

#### Veri Ekleme ve Okuma

```javascript
<%
Application("SiteAdı") = "Örnek Site"
Response.Write "Site Adı: " & Application("SiteAdı")
%>
```

#### Veri Güncelleme

```javascript
<%
Application.Lock
Application("SiteAdı") = "Yeni Site Adı"
Application.Unlock
%>
```

> **Not:** `Application.Lock` ve `Application.Unlock` kullanımı, veri güncelleme işlemleri sırasında çakışmayı önler.

### Avantajları ve Dezavantajları

| Avantajlar | Dezavantajlar |
| ------------------ | ------------------ |
| Hızlı ve bellek tabanlıdır. | Veriler sunucu yeniden başlatıldığında kaybolur.|
| Tüm uygulama kullanıcıları için ortak bir veri alanı sunar. | `Lock`/`Unlock` kullanımı yönetim zorluğu yaratabilir.|


# Ortam Değişkenleri (Environment)

Ortam değişkenleri, işletim sisteminin belirli bilgileri uygulamalara aktarması için kullanılan global değişkenlerdir. Üç temel türü vardır:

- **Sistem Ortam Değişkenleri (SYSTEM):** Tüm kullanıcılar ve uygulamalar için geçerlidir. Örnek: `PATH`, `COMPUTERNAME`.
- **Kullanıcı Ortam Değişkenleri (USER):** Belirli bir kullanıcıya özeldir. Örnek: `TEMP`, `USERPROFILE`.
- **Süreç Ortam Değişkenleri (PROCESS):** Mevcut süreç (örneğin, bir ASP oturumu) için geçerlidir.

## Ortam Değişkenlerini Kullanma

### Okuma

Ortam değişkenlerini `WScript.Shell` nesnesi ile okuyabilirsiniz. İşte bir örnek:

```javascript
<%
Function GetEnvironmentVariable(envType, envVar)
    On Error Resume Next
    Dim shell, env
    Set shell = CreateObject("WScript.Shell")
    Set env = shell.Environment(envType)
    GetEnvironmentVariable = env(envVar)
    If Err.Number <> 0 Then
        GetEnvironmentVariable = "Hata: " & Err.Description
    End If
    On Error GoTo 0
    Set env = Nothing
    Set shell = Nothing
End Function

Response.Write "Bilgisayar Adı: " & GetEnvironmentVariable("SYSTEM", "COMPUTERNAME") & "<br>"
%>
```

### Yazma

Sistem veya kullanıcı ortam değişkenlerine veri eklemek için aşağıdaki gibi bir yöntem kullanabilirsiniz:

```javascript
<%
Function SetEnvironmentVariable(envType, envVar, envValue)
    On Error Resume Next
    Dim shell, env
    Set shell = CreateObject("WScript.Shell")
    Set env = shell.Environment(envType)
    env(envVar) = envValue
    If Err.Number <> 0 Then
        Response.Write "Hata: " & Err.Description
    Else
        Response.Write envVar & " başarıyla ayarlandı.<br>"
    End If
    On Error GoTo 0
    Set env = Nothing
    Set shell = Nothing
End Function

Call SetEnvironmentVariable("SYSTEM", "MyCustomVariable", "CustomValue")
%>
```

### Geçici Ortam Değişkenleri

Geçici (process-level) ortam değişkenleri yalnızca uygulama çalışırken geçerlidir:

```javascript
<%
Dim shell
Set shell = CreateObject("WScript.Shell")
shell.Environment("PROCESS")("TempVariable") = "TemporaryValue"
Response.Write "Geçici Değişken: " & shell.Environment("PROCESS")("TempVariable")
Set shell = Nothing
%>
```

## Performans Karşılaştırması

| Özellik            | Ortam Değişkenleri (Environment)       | Application Nesnesi               | Session Nesnesi                |
| ------------------ | -------------------------------------- | --------------------------------- | ------------------------------ |
| Performans         | Yavaş                                 | Hızlı                             | Orta                           |
| Kapsam             | Tüm sistem veya süreç düzeyinde       | Uygulama genelinde                | Kullanıcı bazlı                |
| Kalıcılık          | Sistem kapanana kadar                 | Sunucu yeniden başlatılana kadar  | Oturum süresi boyunca          |
| Kullanım Kolaylığı | Dış bağımlılıklar gerekebilir         | ASP'ye entegre                    | ASP'ye entegre                 |

- Birden fazla kullanıcıya ortak veri sunmak için **Application** nesnesini tercih edin.
- Kullanıcı bazlı geçici veriler için **Session** nesnesi daha uygundur.
- Performans ve sistem değişkenlerine erişim gerektiğinde **Environment** kullanılabilir.

Classic ASP uygulamalarında veri yönetimi için Environment, Application ve Session nesneleri farklı ihtiyaçlara hitap eden güçlü araçlardır. Her biri, kullanım alanlarına göre avantajlar ve dezavantajlar sunar. Bu üç yapı arasında doğru seçimi yapmak, uygulamanızın performansını, sürdürülebilirliğini ve kullanıcı deneyimini doğrudan etkiler. İhtiyacınıza uygun olan yapıyı seçerken bu faktörleri göz önünde bulundurun. Ayrıca projenizin yatay veya dikey olarak ölçeklendirilmesi aşamasında **benim önerim bu üçünüde kullanmamanız**. Bu da başka bir yazının konusu olacak.

> Bu yöntemlerin her biri, farklı senaryolarda avantaj sağlar. İhtiyacınıza uygun olanı seçerek uygulamanızın performansını ve verimliliğini artırabilirsiniz!
{: .prompt-danger }