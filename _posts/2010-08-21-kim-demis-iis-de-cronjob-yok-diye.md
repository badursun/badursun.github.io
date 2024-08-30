---
layout: post
title: "Kim demiş IIS de Cronjob Yok Diye?"
date: 2010-08-21 12:02
categories: ["Yazılım", "Windows Server"]
tags: ["windows", "linux", "cronjob", "crontab", "james-crooke"]
---

Birçok Linux tabanlı yazılımcının "1-0 öndeyiz" dediği cronjob (crontab) yani zamanlanmış görev özelliği, artık bir yama ile Windows'ta da kullanılabilir hale geliyor. Eğer çok yoğun bir web siteniz yoksa, genellikle tarih tutarak `Server.Execute()` metodu ile bu işlevi indexinize gömerek veya web tabanlı ücretli/ücretsiz cron servislerini kullanarak çözüm bulabilirsiniz. Ancak artık cronjob, eskisi gibi külfetli değil.

## Peki nasıl oluyor?
Basit: [James Crooke](https://www.jamescrooke.co.uk/) geçen sene [blogunda](https://www.jamescrooke.co.uk/blog/10/cron-for-windows-iis/) bunu detaylı olarak anlattı. `WGet` adlı bir bileşeni yükleyerek, Ms-Dos komut satırı yardımıyla siz de cronjob özelliğini kullanabilirsiniz. Ancak üzülerek belirtmeliyim ki, kendi VPS, VDS ya da sunucunuz yoksa, yer sağlayıcınız bu tür bir yazılımı kurmanıza veya kullanmanıza izin vermeyebilir.

## CronJob Nedir?
CronJob, Türkçe karşılığıyla zamanlanmış görev demektir. "Ne için zamanlanmış görev kullanmalıyım?" dediğinizi duyar gibiyim. Diyelim ki bir oyun scripti ya da istatistik scripti yazdınız ve bu sistemin belirli aralıklarla çalıştırılması gereken bir kod öbeği var. Eğer bunu sürekli kendiniz yapacak olursanız, bilgisayar başında esir olursunuz. Bu işlemi bir sayfaya gömerek çalıştırmak isterseniz, yoğun bir sitede aynı anda birden fazla kez çalışması veya sunucu tarafında kilitlenmeye neden olma riski vardır. İşte cronjob, belirttiğiniz bir exe, asp ya da benzeri çalıştırılabilir programı veya sayfayı, yine belirlediğiniz tarih/tarihler ve saatlerde çalıştırmaya yarar.

CronJob, Linux/Unix sistemlerde host panellerinde mevcuttur, fakat Windows bu konuda biraz geri kalmıştır. Neyse ki, bu eksiklik WGet ile bir nebze giderilmiş durumda. Ancak herkes bu çözümden haberdar değil. WGet, oldukça küçük ve kullanımı biraz karışık gibi görünen ama işlevsel bir uygulama.

### Kurulum ve Kullanım

Öncelikle, WGet uygulamasını dosyalar bölümünden ya da yukarıdaki linkten indirip yükleyin. Sonra, Ms-Dos komut satırına girerek şu komutu yazın ve Enter'a basın:

```bash
schtasks /create /tn "Test Cron" /tr "C:\wget.exe https://www.site.com/cron.asp" /sc minute /mo 5 /ru "System"
```

Yukarıdaki örnekte, "Test Cron" adına sahip site.com/cron.aspyi 5 dakikada bir çalıştıracak cron oluşturmuş olduk.

### Birkaç Örnek

#### Haftalık
```bash
schtasks /create /tn "Test Cron" /tr "C:\wget.exe https://www.site.com/cron.asp" /sc weekly /ru "System"
```

#### Her Hafta Cuma
```bash
schtasks /create /tn "Test Cron" /tr "C:\wget.exe https://www.site.com/cron.asp" /sc weekly /d FRI /ru "System"
```

#### Her Hafta Cuma Saat 11:00'da
```bash
schtasks /create /tn "Test Cron" /tr "C:\wget.exe https://www.site.com/cron.asp" /sc weekly /st 11:00:00 /d FRI /ru "System"
```

#### Her gün saat 17:00'de
```bash
schtasks /create /tn "Test Cron" /tr "C:\wget.exe https://www.site.com/cron.asp" /sc daily /st 17:00:00 /ru "System"
```

#### Her Saat başı
```bash
schtasks /create /tn "Test Cron" /tr "C:\wget.exe https://www.site.com/cron.asp" /sc hourly /st 00:00:00 /ru "System"
```

Yazılımı Dosyalar bölümünden, ASP Kategorisi altında yükleyebilirsiniz.

Yok, ben makalenin orijinalini okumak istiyorum derseniz, [buraya tıklayın](https://www.jamescrooke.co.uk/blog/10/cron-for-windows-iis/).

CPanel CronJob hakkında detaylı bilgi için [buraya](https://www.bilgiportal.com/v1/idx/52/2888/Hosting/makale/cPanel-Cron-Jobs-Nedir-Nasl-Kullanlr-.html) tıklayın.