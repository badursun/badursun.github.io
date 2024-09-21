---
layout: post
title: "Plesk Backup Warning: Unable to get attributes of the file Access is denied -WINDOWS"
date: 2020-01-27 10:50
categories: ["Yazılım","Windows Server"]
tags: ["plesk", "plesk-backup", "hosting-manager", "windows-server", "windows-plesk"]
toc: true
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/plesk-windows-backup-failure-66eec40572338.webp"
  alt: "Plesk Backup Warning"
---

Plesk backup sisteminde bazen hatalar alırsınız. Bu hatalardan en sık karşılaşılanı olup, backup sürecini etkilemeyen fakat eksik bir şekilde backup alınmasına neden olan Access Denied hatasıdır.

> WARNING: (domain object 'example.com') [e4404190-b0b5-425e-a309-500e258cfe97]: Unable to get attributes of the file C:\Inetpub\vhosts\example.com\httpdocs\img\originais\photo.jpg: Access is denied. The file will not be archived. [:0]
{: .prompt-danger }

Çözüm yolu için sunucuya RDP ile bağlanmanız ve Power Shell altında aşağıda ki komutu uyarlayıp çalıştırmanız gerekmektedir. Hata bir klasör erişim yetki sorunudur. Bu genelde üçüncü parti komponentlerin resim, dosya yüklemesi esnasında izinlerin doğru aktarılamamasından kaynaklanır.

## Power Shell komutu:
```shell
plesk repair --repair-webspace-security -all-filesystem-objects -webspace-name example.com
```