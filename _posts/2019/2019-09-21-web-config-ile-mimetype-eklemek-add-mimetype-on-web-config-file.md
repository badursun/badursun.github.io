---
layout: post
title: "web.config ile MimeType Eklemek"
date: 2019-09-21 13:46
categories: ["Yazilim","Windows Server"]
tags: ["windows", "asp", "mimetype", "webconfig", "classic-asp"]
toc: true
---

Bazen sunucuya tam erişim hakkınız olmaz ve mimetype eklemeniz gerekir. Bu durumda yapmanız gereken şey **web.config** dosyanız üzerinden mimetype tanımlamaktır.

Bunu yapmak için aşağıdaki kodu girmeniz gerekir.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <staticContent>       
            <mimeMap fileExtension=".oga" mimeType="audio/ogg" />
            <mimeMap fileExtension=".mp4" mimeType="video/mp4" />
            <mimeMap fileExtension=".webm" mimeType="video/webm" />
            <mimeMap fileExtension=".ogv" mimeType="video/ogv" />
            <mimeMap fileExtension=".ogg" mimeType="video/ogg" />
            <mimeMap fileExtension=".m4a" mimeType="video/mp4" />
            <mimeMap fileExtension=".webmanifest" mimeType="application/manifest+json" />
        </staticContent>
    </system.webServer>
</configuration>
```

Bu şekilde **web.config** ile mimetype eklemesi yapabilirsiniz. Fakat bazen bu da hata verir. Mevcutta IIS de tanımlı bir mimetype tanımlarsanız, bir çok dosyanın çalışmaz hale geldiğini göreceksiniz.

Bunu çözmek için önce eklediğiniz mimetype'ları kaldırarak eklemeniz gerekir. Bu da yine **web.config** üzerinden basit bir şekilde yapılabilir.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <staticContent>       
            <remove fileExtension=".oga" />
            <mimeMap fileExtension=".oga" mimeType="audio/ogg" />

            <remove fileExtension=".mp4" />
            <mimeMap fileExtension=".mp4" mimeType="video/mp4" />

            <remove fileExtension=".webm" />
            <mimeMap fileExtension=".webm" mimeType="video/webm" />

            <remove fileExtension=".ogv" />
            <mimeMap fileExtension=".ogv" mimeType="video/ogv" />

            <remove fileExtension=".ogg" />
            <mimeMap fileExtension=".ogg" mimeType="video/ogg" />

            <remove fileExtension=".m4a" />
            <mimeMap fileExtension=".m4a" mimeType="video/mp4" />

            <remove fileExtension=".webmanifest" />
            <mimeMap fileExtension=".webmanifest" mimeType="application/manifest+json" />
        </staticContent>
    </system.webServer>
</configuration>
```