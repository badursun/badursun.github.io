---
layout: post
title: "Red Alert 2 Windows 7 ve Windows 8 de oynamak"
date: 2014-02-23 21:52
categories: ["Teknoloji"]
tags: ["red-alert-2", "windows-7", "windows-8", "winsocks32", "ipx", "spx"]
---

Eski oyunlardan vazgeçemeyenler için yeni işletim sistemlerinin çıkması her zaman bir korku olmuştur. Ne yazık ki Windows Vista’da (ben SP1 üzerinde denedim) Red Alert 2 oyununda ve Yuri’s Revenge eklenti paketinde Network seçeneğini seçtiğinizde oyun ana menüye dönüyor ve çok oyunculu oynayamıyoruz.

En azından Google'da ufak bir arama yapana kadar oynayamıyordum.

Şu kaynakta bulduğum bilgiye göre ufak bir dosya eklentisi yaparak Red Alert 2′nin IPX yerine UDP protokolünü kullanmasını sağlayarak Windows Vista’da çoklu oyuncu desteıine kavuşmasını sağlayabiliyoruz. IPX çok eski bir protokol olduğundan Vista IPX desteklemiyor.

## Adım 1
Oyunu kurduğunuz klasöre gidin

## Adım 2
**game.exe**, **ra2.exe**, **gamemd.exe**, **ra2md.exe** dosyalarına tek tek sağ tıklayıp, özellikler, uyumluluk modu ayarlarına gelin, windows 98'i seçin, alt kutusuda ki Yönetici Olarak Aç seçeneğini işaretleyin ve uygula, tamam tuşuna basın. Oyun açılışında hata mesajı alıyorsanız artık bu mesajı almayacaksınız.

## Adım 3
Ağ üzerinden oynamak için ise [buraya](https://dl.dropboxusercontent.com/u/79400900/wsock32.zip) tıklayın ve dosyayı indirin. Zip içinden çıkan **wsocsk32.dll** dosyasını oyunun kurulu olduğu (game.exe vb..) klasörün içine atın. Oyunu tekrardan başlatın.

Artık Red Alert 2 oyununu windows vista, windows 7, windows 8 gibi tüm platformlarda sorunsuzca oynayabilirsiniz.

Bizzat denedim ve çalıştırdım.

Yazının esas kaynağı için **[https://www.ubenzer.com/red-alert-2yi-vistada-multiplayer-oynamak/](https://www.ubenzer.com/red-alert-2yi-vistada-multiplayer-oynamak/)** tıklayabilirsiniz. Yazı üzerinde bir kaç küçük noktayı editledim ve güncelledim. Kendi platformum windows 8'dir. Yazıyı yazan arkadaş Win 7 ve platformlarında test etmiştir.