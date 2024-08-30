---
layout: post
title: "macOS Sistemlerde HOSTS Dosyası Nasıl Değiştirilir?"
date: 2017-06-14 13:31
categories: ["Yazılım", "MacOS"]
tags: ["macos", "hosts", "ip-config", "dns"]
toc: true
---

Hosts dosyası bir işletim sisteminde bulunan, metin editörleri ile açılıp değiştirilebilen düz bir metin dosyasıdır. İnternet sitelerinin isimlerini ip adresleri ile eşleştirir. DNS yani Domain Name Server hizmetinde bir sorun, engel ya da yönlendirme varsa ve istediğiniz siteler açılmıyorsa, hosts dosyası üzerinde belirli alan adlarını ip adresleriyle birlikte değişiklik yaparak açılır hale getirebilirsiniz. Sanılanın aksine sadece Windows işletim sisteminde değil, eski yeni sayısız işletim sisteminde bulunur. Android ve iOS gibi mobil işletim sistemlerindeki hosts dosyası adreslerini de sizin için burada sunuyoruz. En çok merak edilen olduğu için Windows 7'yi önceden ve ayrıca verelim: **C:\Windows\System32\drivers\etc** adresinde hosts dosyasını bulabilirsiniz.

Hosts dosyası, YouTube engellendiği sıralar bayağı bir bilinen dosyaydı. O sayede tanıdı herkes bu hosts dosyasını. Fakat nedir bilmiyoruz bu hosts dosyası? Bir web sitesine giriş yapmak için normalde alan adı değil de bir numara girmemiz gerekir. Mesela facebook.com'un numarası 173.252.101.26'dır. Fakat bu numaraları aklımızda tutmak zor olduğundan dolayı onun yerine "facebook.com" gibi adresler ile bu numaraları kolayca bulmamızı sağladık. Yani siz aslında adres çubuğuna facebook.com yazdığınız zaman aslında 173.252.101.26 adresine gidiyorsunuz. Bu kısım bu kadar. Peki nedir bu hosts dosyası?

İşte bu dediğimiz hosts dosyası, şu işe yarar; tarayıcımızda facebook.com diye aradığımızda bilgisayarımız önce hosts dosyasına bakar. Eğer hosts dosyasında facebook.com ile ilgili bir kayıt var ise (# 173.252.101.26 facebook.com gibi) sistem direk onu açar. Eğer öyle bir kayıt bulunmuyorsa bilgisayarımız internette bulunan DNS sunucularına bağlanarak facebook.com’un adresini öğrenir ve bağlanır.

## MacOS da Host Dosyası Nasıl Değiştirilir?
Mac işletim sisteminde HOSTS dosyasını değiştirmek, windows da olduğundan biraz daha farklıdır. HOSTS dosyasını değiştirmek için Terminal uygulamasını açalım. $ sudo nano /private/etc/hosts yazabilirsiniz. Yada Ekranınızın üst kısmında bulunan mac tool bar aracılığı ile Go &gt; Go To Folder aracılığı ile aşağıda ki komut kutucuğuna ulaşabilirsiniz. Bu kutucuğa /private/etc/hosts yazmanız yeterlidir.

Terminal ile direkt dosyası değiştirme/edit için açarken, Go To Folder ile fiziksel klasöre erişirsiniz. Bu dosyayı açmak için bir aracı programa ihtiyaç duyacaksınız. Örneğin ben Sublime Text ile açıyorum. Fakat aşağıda standart Text Editor ile açılışı göreceksiniz.

Fiziksel klasörde hosts dosyasını bulup açtıktan sonra eğer windows kullandıysanız daha önce, aşina olduğunuz bir görüntü gelecek.

Bu noktada eklemek istediğiniz yönlendirme/ip/domain bilgilerini güncelleyerek devam edebilirsiniz.

Bu dosyayı kaydetmek istediğinizde size root şifrenizi soracak. Root şifrenizi girdikten sonra bu dosyayı kayıt edebileceksiniz

## Diğer İşletim Sistemlerinde HOSTS Dosyası Nerede Bulunur

- Unix, Unix-benzeri işletim sistemlerinde ve POSIX'te **/etc/hosts**
- Microsoft Windows 3.1'de **%Windir%\HOSTS**
- 95, 98/98SE, Me %WinDir%\hosts[3]
- NT, 2000, 32 bit XP, 2003, Vista ve Windows 7'de %SystemRoot%\system32\drivers\etc\hosts
- 64-bit işletim sistemlerinde %SystemRoot%\system32\drivers\etc\hosts altında yer alır ancak Microsoft destek sayfalarında %SystemRoot%\SysWOW64\drivers\etc\hosts olduğu yazılır, bu yanlış bir bilgidir
- Windows Mobile içerisinde Registry'de \HKEY_LOCAL_MACHINE\Comm\Tcpip\Hosts
- Apple Macintosh 9 ve öncesinde sistem klasöründe
- Mac OS X 10.0 – 10.1.5 NetInfo veya niload ile eklenir
- Mac OS X 10.2 ve sonrasında /private/etc/hosts yer alır
- Novell NetWare'de SYS:etc\hosts
- OS/2 &amp; eComStation'da "boot edilen sabit disk adı":\mptn\etc\
- Symbian OS 6.1–9.0'da C:\system\data\hosts
- Symbian OS 9.1 üzerinde C:\private\10000882\hosts
- MorphOS, NetStack ENVARC:sys/net/hosts
- Android'de /system/etc/hosts ya da /etc/hosts adresinde
- Jailbreak edilmiş iOS'ta iOS 2.0 ve üstü sürümlerde /private/etc/hosts ya da /etc/hosts adresinde
- TOPS-20'de &lt;SYSTEM&gt;HOSTS.TXT