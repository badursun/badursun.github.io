---
layout: post
title: Windows Server Classic ASP CORS Policy Problemi Çözümü
date: 2021-10-22 12:04:00
categories: [yazilim]
tags: [window,asp,cors,webconfig]
---

![Windows Server Classic ASP CORS Policy Problemi Çözümü](/assets/img/2021-10-22-windows-server-classic-asp-cors-policy-problemi-cozumu.png)

Yine hızlı bir çözüm yazısı ile beraberiz. CORS Policy sorunu yüzünden şu yazıyı hepiniz görmüşsünüzdür.

Access to XMLHttpRequest at ‘https://www. xxxxxx’ from origin ‘http://www. xxxxx’ has been blocked by CORS policy: Response to preflight request doesn’t pass access control check: No ‘Access-Control-Allow-Origin’ header is present on the requested resource.

İşte bu sorunun windows server’larda Classic ASP ile çözümü oldukça basit. Tabii konu derinlemesine güvenlik içeriyor fakat özetle bu soruna kısadan bir çözüm yolunu web.config dosyası ile nasıl yaparsınız; işte hazır bir web.config dosyası

## web.config Dosyası Güncellemesi
   ```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <httpProtocol>
            <customHeaders>
                <add name="Access-Control-Allow-Origin" value="*" />
                <add name="Access-Control-Request-Method" value="POST, GET, OPTIONS, PUT, DELETE" />
                <add name="Access-Control-Allow-Headers" value="Authorization" />
            </customHeaders>
        </httpProtocol>
    </system.webServer>
</configuration>
   ```

Eğer bir servisinizde CORS hatası ile karşılaşıyorsanız, servisinizi sunduğunuz temel web.config ayarı yukarıda ki gibi olmalıdır. Bu ayarlar ile POST, GET, OPTIONS, PUT ve DELETE methodlarına izin verirsiniz. Burada önemli olan bir ayrıntı OPTIONS methodur. Bu method güncel bir çok tarayıcı tarafından PREFILGHT yani CORS kontrolünde ilk adım olarak gerçekleşir, dolayısıyla mutlaka OPTIONS eklemenizde fayda var. Ayrıca servisinizde bir Authorization header ekli ise yine izin verilen header parametresine bunu eklemenizde fayda olacaktır.