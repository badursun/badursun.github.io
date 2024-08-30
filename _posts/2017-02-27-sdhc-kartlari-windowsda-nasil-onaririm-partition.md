---
layout: post
title: "SDHC Kartları Windows'da Nasıl Onarırım (Partition)"
date: 2017-02-27 11:59
categories: ["Teknoloji"]
tags: ["sdhc", "sd-card", "diskpart", "msdos", "partition", "disk-partition"]
toc: true
---

SD Kartlar bazen durduk yere bozulur. Genelde boyutu olduğundan küçük gözükür. Bu gibi durumlarda SD kartı tekrar formatlarsınız fakat düzelmez. Bu partition ile ilgili bir hatadır. Bazen boot atılan kartlarda da gözüken klasik bir sorundur. Bunu çözmek için genelde disk manager ve benzeri programlar kullanılır yada üçün parti internetten indirilen partition tool programları kullanılır. Fakat bunlara da güven olmadığı gibi çoğu ücretli programlardır.

Klasik windows formatlama aracı da buna çözüm olmadığında Disk Yönetimi üzerinden komple bölümleri silmeyi denersiniz ve yine hata alırsınız. Lakin bölümü silip, tekrar oluşturduğunuzda da yine kartınız da aynı hatayı almaya devam edersiniz. İşte bu noktada daha güçlü bir erişime ihtiyaç vardır ve windows bu güçlü erişime sahip programı içerisinde barındırıyor. Fakat bunun bir arayüzü yok.

Partition Manager'da ise bazen var olan bölümü silmenize izin vermez. Halbu ki silebilseniz, ne güzel olurdu :) Bütün dertler biter, kart kurtulurdu fakat malesef Disk Yönetimi altında ki Partition Manager bunu yapamıyor.

Hazırsanız size boyutlandırması bozulan SD kartlarınızı tekrardan eski boyutuna nasıl kavuşturacağınızı adım adım anlatıyorum.

## Diskpart
Başlat menüsüne tıklayın ve **Diskpart** yazın, çıkan programa sağ tıklayıp **Yönetici İzinleri İle Aç** seçeneği üzerinden programı çalıştırın. Karşınıza MS-DOS ekranı gelecek, linux ve mac'ciler terminal den aşinadır.
<h4>1. Bölüm Silme</h4>
İlk önce diskleri görüntülememiz gerekir. Bunun için terminal ekranına   **LIST DISK**  yazıyoruz.

Mevcut bütün diskleriniz birer numara ile dökülecek. İşlem yapacağımız diski seçmemiz gerekiyor. Eğer SD Kartınızın numarası **DISK 1** ise seçmek için bu komutu yazın **SELECT DISK 1** arından entera basın.

Doğru diski seçtiğinize emin olmak için isterseniz tekrar **LIST DISK** yazabilirsiniz. Doğru diski seçtiysek bu sefer **LIST PARTITION** yazarak, seçtiğimiz diskin bölümlerini görüntüleyelim. Yine aynı şekilde bölümler numaralı olarak çıkacaktır. Birden fazla varsa ve hepsini silip, tek parça yapmak istiyorsak tek tek bölümleri silmemiz gerek.  Ama önce bölümü seçmemiz gerekiyor, şu komutu giriyoruz **SELECT PARTITION 1** ve 1. bölümü seçtik. Şimdi seçtiğimiz diskte, seçtiğimiz bölümde silme işlemi yapacağımız komut girelim **DELETE PARTITION** ile seçtiğimiz bölümü sildik.

## 2. Bölüm Oluşturma
Yeni bölüm oluşturmak için **CREATE PARTITION PRIMARY** yazıp entera basıyoruz. Bu size içi belirsiz boşlukta bir alan oluşturacaktır. Dilerseniz direkt olarak alanı belirleyip bölüm oluşturmak isterseniz **CREATE PARTITION PRIMARY SIZE=NNN** komutunu giriyoruz. Burada ki NNN yerine MB cinsinden bölüm boyutunu girmemiz gerekiyor. Örnek verecek olursak  **CREATE PARTITION PRIMARY SIZE=4096 **komutu ile 4 GB lık bir alan oluşturabiliriz.

Gönlüm rahat olsun derseniz terminalden çıkıp kartınıza temiz bir format atabilirsiniz. Terminalden çıkmak için **EXIT** yazıp enterlamanız yeterlidir.

Protected EFI disk partition olarak geçen korumalı disk, bellek ve kartları da bu yöntem ile silebilirsiniz. Fakat unutmayın, bu tür silme işlemleri kartınızın yada belleğinizin ve diskinizin içinde gömülü olan recovery (kurtarma) ve yardımcı yazılımlar ile güvenlik araçlarını da silecektir. Ve bunun geri dönüşü olmadığı gibi, kurtarma şansınız da yoktur.