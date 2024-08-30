---
layout: post
title: "Windows Sunucuda Wordpress ve web.config Dosyası"
date: 2018-04-19 16:08
categories: ["Yazılım","Windows Server"]
tags: ["windows", "wordpress", "htaaccess", "webconfig"]
toc: true
---

## Windows Sunucuda Wordpress Çalıştırmak
Windows Sunucularda Wordpress ve **web.config** dosyası nasıl ayarlanır, en büyük sorunlardan birisi olabiliyor bazen. Window sunuculara aşina değilseniz, web.config dosyası hazırlarken kalp krizi geçirebilirsiniz.

Aşağıda direkt kullanabileceğiniz örnek web.config dosyası yer almaktadır. Windows sunucu / hosting'e kurup bu dosyayı httpdocs ana dizine bırkmanız yeterlidir. Dosyayı web.config adı ile kaydetmeniz gerekmektedir.

## web.config
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <httpErrors errorMode="Detailed">
            <remove statusCode="404" subStatusCode="-1" />
        </httpErrors>
        <httpCompression directory="%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files">
            <scheme name="gzip" dll="%Windir%\system32\inetsrv\gzip.dll" />
            <dynamicTypes>
                <add mimeType="text/*" enabled="true" />
                <add mimeType="message/*" enabled="true" />
                <add mimeType="application/javascript" enabled="true" />
                <add mimeType="*/*" enabled="false" />
            </dynamicTypes>
            <staticTypes>
                <add mimeType="text/*" enabled="true" />
                <add mimeType="message/*" enabled="true" />
                <add mimeType="application/javascript" enabled="true" />
                <add mimeType="*/*" enabled="false" />
            </staticTypes>
        </httpCompression>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="8.00:00:00" />
        </staticContent>
        <caching>
            <profiles>
                <add extension=".jpg" policy="CacheUntilChange" kernelCachePolicy="CacheUntilChange" />
                <add extension=".jpeg" policy="CacheUntilChange" kernelCachePolicy="CacheUntilChange" />
                <add extension=".png" policy="CacheUntilChange" kernelCachePolicy="CacheUntilChange" />
                <add extension=".js" policy="CacheUntilChange" kernelCachePolicy="CacheUntilChange" />
                <add extension=".css" policy="CacheUntilChange" kernelCachePolicy="CacheUntilChange" />
            </profiles>
        </caching>
        <urlCompression doDynamicCompression="true" />
        <rewrite>
            <rules>
                <rule name="WordPress: http://www.domain.com" patternSyntax="Wildcard">
                    <match url="*" />
                    <conditions>
                        <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
                        <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
                    </conditions>
                    <action type="Rewrite" url="index.php" />
                </rule>
            </rules>
        </rewrite>
    </system.webServer>
</configuration>
```