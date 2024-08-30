---
layout: post
title: "3D Printer Eksen Kayma Sorunları"
date: 2017-02-26 23:08
categories: ["Teknoloji", "3D-Printer"]
tags: ["3d-printer", "axis", "diy", "embedded-system"]
toc: true
---

### 3D Yazıcıların Açık Döngülü Kontrol Mekanizması
Çoğu 3D yazıcı, açık döngülü kontrol mekanizması kullanır, yani makine, nozzle'ın hedef konumuyla gerçek konumunu karşılaştırarak doğrulama yapamaz. Geri bildirim mekanizmasının olmaması, yazıcının nozzle'ı belirli bir noktaya taşırken, o noktaya gerçekten ulaşıp ulaşmadığını bilememesine yol açar. Yazıcı, nozzle'ı hareket ettirir ve doğru noktaya ulaşacağını varsayar. Genellikle bu sistem sorunsuz çalışır çünkü step motorlar nozzle'ı yeterince güçlü bir şekilde iter ve aşırı yüklenme olmadığı sürece sorun yaşanmaz. Ancak, herhangi bir aksilik durumunda yazıcı bu durumu fark edemez. Örneğin, yazım sırasında yazıcıya çarpmanız veya yazıcı kafasında bir kayma olması, geri bildirim eksikliği nedeniyle tespit edilemez. Yazıcı, sanki hiçbir şey olmamış gibi baskıya devam eder. Eksen veya katman kayması gibi sorunlar genellikle bu tür durumlar nedeniyle ortaya çıkar. Ne yazık ki, bu tür bir sorun meydana geldiğinde yazıcının bunu algılayıp düzeltmesi mümkün değildir, bu yüzden üretimi yeniden başlatmak gerekebilir.

#### Motorların Çok Hızlı Hareket Etmesi
Eğer yazıcı çok hızlı çalışıyorsa, motorlar bu hızı sürdüremeyebilir. Motorlar aşırı yüklendiğinde, genellikle "klik" sesleri duyulmaya başlar. Bu durum, yazıcı kafasının daha önce üretilen katman üzerine doğru malzemeyi düzgün şekilde yerleştirememesine yol açar. Yazıcınızın çok hızlı çalıştığını düşünüyorsanız, hızı %50 oranında azaltmayı deneyebilirsiniz. Hızı yarıya indirmenize rağmen sorun devam ederse ve ileri düzey ayarlarla rahat hissediyorsanız, motorların ivmelenme değerini de düşürmeyi deneyebilirsiniz.

#### Extruder’ın Çok Hızlı Hareket Etmesi
Extruder'ın hareket hızının maksimum çalışma hızını aşması, katman kaymasına neden olabilir. Bu durumda, extruder hızını varsayılan ayarlara geri getirmek sorunu çözebilir.

#### Mekanik ve Elektriksel Sorunlar
Eğer eksen kayması sorunu devam ederse, problem yazıcının mekanik veya elektriksel bileşenlerinden kaynaklanıyor olabilir. Çoğu yazıcıda, yazıcı kafasının hareketini sağlamak için kayışlar kullanılır. Bu kayışlar zamanla gevşeyebilir. Kayışı gerdirip yeniden üretime geçmeyi deneyebilirsiniz. Ancak, kayışın gerginliği, motorun dönmesini engelleyecek kadar sıkı olmamalı, fakat eksen kaymasına neden olacak kadar gevşek de olmamalıdır. Uygun bir gerginlik ayarı yapılmalıdır.

Bir diğer mekanik sorun, motor mili ile kasnağı birbirine bağlayan setiskurun gevşemesi olabilir. Bu durumda, motor mili döner fakat kasnak hareket etmez, bu da dönen gücün kasnağa aktarılmamasına ve eksen kaymasına neden olur. Bu sorunu gidermek için setiskuru sıkmak gerekir.

Ayrıca, eksen kaymasına neden olabilecek çeşitli elektriksel sorunlar da vardır. Örneğin, motorlara yeterli akım gitmemesi motorların adım kaçırmasına yol açabilir. Başka bir elektriksel sorun ise motor sürücülerinin aşırı ısınmasıdır. Aşırı ısınan motor sürücüleri, soğuyana kadar çalışmaz ve bu da adım kayıplarına neden olur. Bu sorunu önlemek için, kartların bulunduğu bölgeye bir fan yerleştirilmesi önerilir.
