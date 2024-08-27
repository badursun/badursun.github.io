---
layout: post
title: "IIS web.config HTTP - HTTPS Force Redirect Sample"
date: 2020-01-25 17:14
categories: ["Yazılım","Windows Server"]
tags: ["windows", "asp", "https", "webconfig", "classic-asp"]
---

## IIS web.config HTTP - HTTPS Force Redirect Sample
**web.config** dosyası web sitesinin ortak konfigürasyon ayarlarının tutulduğu XML tabanlı bir dosyadır.Bu dosya sistenin kök dizininde olabileceği gibi belli dizinler altındada olabilir. Sitenin kök dizininde olduğunda dosyadaki bilgiler tüm siteyi etkiler, eğer bir dizin altına yerleştirmişsek dosyadaki bilgiler sadece o dizin altındaki sayfaları etkileyecektir.

Buna örnek olarak üyelik sayfalarının içinde barındığı bir dizini verebiliriz. **web.config** dosyasında üyelik sayfalarının içinde bulunduğu dizin için kullanıcı girişi yapılmasını istediğimizde bu dizindeki sayfalara erişebilmek için yine **web.config** dosyasında belirttiğimiz kullanıcı adı/parola çiftine sahip olan kullanıcılar istenilen sayfalara erişebilmektedir.

İhtiyacınız olan ziyaretçinizi otomatik oalrak http den https protokolüne aktarmak ise, **web.config** dosyasına ekleyeceğiniz bir rewrite rule ile çözülebilir.

Hemen kodumuza geçelim;

### web.config
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <rewrite>
            <rules>
                <rule name="Force HTTPS" enabled="true">
                    <match url="(.*)" ignoreCase="false" />
                    <conditions>
                        <add input="{HTTPS}" pattern="off" />
                    </conditions>
                    <action type="Redirect" url="https://{HTTP_HOST}{REQUEST_URI}" appendQueryString="true" redirectType="Permanent" />
                </rule>
            </rules>
        </rewrite>
    </system.webServer>
</configuration>
```