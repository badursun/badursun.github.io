---
layout: post
title: "MacOS ve Paloalto Globalprotect VPN Uninstall"
date: 2017-10-09 22:50
categories: ["Yazılım", "MacOS"]
tags: ["macos", "paloalto", "vpn", "vpn-uninstall"]
---

Önce isterseniz [ekşi](https://eksisozluk.com/globalprotect--4680006)'den bir güfte ile GlobalProtect VPN nedir okuyalım: 

> "palo alto networks tarafından geliştirilen, hafiften arsız bir kurumsal vpn yazılımıdır. wan bağlantısı kuramadığı her durumda zbank diye ekrana fırlayan bir durum penceresi, ve bağlantı kesilmek istendiğinde kullanıcıya "neden?" diye hesap soran bir kaynana modülü vardır. görünüşe göre gezenti (bkz: roaming) personelin sürekli kullanımı için tasarlanmış olup, uzaktan destek için arada sırada kullanıma uygun değildir. kullanılmadığı dönemlerde kaldırılması (bkz: uninstall) vaciptir."

Eğer Paloalto'nun GlobalProtect VPN programını yüklediyseniz ve uninstall edemiyorsanız korkmayın :) Bu gün size globalprotect vpn uninstall nasıl yapılır anlatacağım. Çünkü benimde gerçekten mac kullanmaya başladığımdan beri en karın ağrılı sürecimi yaşamama sebep oldu. İlk önce versiyon sorunları, sonrasında müşterimin vpn değişikliği sebebiyle çok uğraştırdı beni. En son kaldırmak istediğimde ise çaresizce arka planda çalışmasına izin vermek zorunda kaldım. Daha sonra bir gün oturup araştırmaya karar verdim ve çözümü buldum. Fakat türkçe kaynak olmadığı için globalprotect vpn uninstall nasıl kaldırılır sorusuna bir türkçe kaynak yaratmak istedim.

Paloalto'nun saçma politikası sebebiyle programın orjinal sürümünü indirmek veya uninstall yapmak için dahi sisteme login olmanız gerekiyor. Lakin, üyeliğiniz bittiyse ve giriş yapamıyorsanız program sümük gibi yapışıyor. Bu arada saçma dediğime bakmayın belki şirket için mantıklı bir açıklaması vardır ama biz kullanıcılar için bazen "basit" olmak gerekiyor. Basitlik iyidir, sadelik can'dır :)

Gelelim globalprotect vpn uninstall nasıl yapılır. Yapmanız gereken spotlight açarak terminal ekranına erişmeniz ve aşağıdaki kodu yapıştırdıktan sonra kullanıcı parolanızı girmeniz. Böylece VPN programını hızlı bir şekilde sonsuzluğa uğurlamış olacaksınız.

```bash
sudo /Applications/GlobalProtect.app/Contents/Resources/uninstall_gp.sh
```