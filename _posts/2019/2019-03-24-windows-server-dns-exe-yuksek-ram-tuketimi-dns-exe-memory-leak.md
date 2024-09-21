---
layout: post
title: "Windows Server DNS.EXE Yüksek Ram Tüketimi (dns.exe Memory Leak)"
date: 2019-03-24 13:34
categories: ["Yazılım","Windows Server"]
tags: ["windows", "dns", "iis", "ram-leak", "classic-asp"]
toc: true
---

Windows sunucularda IIS çalıştırıyorsunuz fark edeceğiniz bir şey var, taskmanager da mutlaka görürsünüz. **dns.exe** oldukça yüksek ram tüketimine sahiptir. Bunun için yapılacak ufak bir şey var.

dns.exe socket yapısında kontrollerini sağladığı için izin verilen maksimum soket bağlantı sayısını arttırmak gerekir.

- Klavyeden windows logosu ve R harfine basın, CMD (Komut işlemcisi) için yönetici desteği ile başlatın yada **runas /user:administrator@domain.local cmd** yazabilirsiniz. Burada aktif direktörde ki domain yada lokal servis adını yazmalısınız. Yada daha basitçesi, cmd.exe yi yönetici yetkileri ile başlatın.
- İstenirse administrator parolanızı girin.
- Komut satırına **dnscmd.exe /config /socketpoolsize 1000** yazın ve enter'a basın.
- Başarılı bir şekilde soket sayısını arttırdıysanız **net stop dns && net start dns** yazın ve enter'a basın. Yada daha basitçe, servisler bölümünden dns Server servisini durdurun, başlatın.

Sonuç olarak öncesi ve sonrası için etkiyi görebilirsiniz.