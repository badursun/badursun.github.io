---
layout: post
title: "MacOS DNS cache Temizlemek"
date: 2017-09-28 18:15
categories: ["Yazılım", "MacOS"]
tags: ["macos", "dns", "dns-cache", "mdnsresponder"]
toc: true
---

Bir apple MacOS kullanıyorsanız ve DNS Cache sorunu yaşıyorsanız terminal ekranını çalıştırıp aşağıda sürümünüze uygun terminal komutunu girerek DNS Cache temizliği yapabilirsiniz. Bu işlemi tamamlamak için sizden mac parolanız istenecektir. Ekranda parolanızı yazarken hiç bir hareketlilik olmayacak, yani parolanızı doğru yazdınıza emin olun.

**macOS Sierra 10.12.0**
```bash
sudo killall -HUP mDNSResponder
```

**OSX 10.11.0**
```bash
sudo killall -HUP mDNSResponder
```

**OSX 10.10.4**
```bash
sudo killall -HUP mDNSResponder
```

**OSX 10.10.0 – 10.10.3**
```bash
sudo discoveryutil mdnsflushcache
```

**OSX 10.9 – 10.8 – 10.7**
```bash
sudo killall -HUP mDNSResponder
```

**OSX 10.5 – 10.6**
```bash
sudo dscacheutil -flushcache
```

**Windows**
```bash
ipconfig /flushdns
```

**Linux**
```bash
/etc/init.d/named restart
/etc/init.d/nscd restart
```