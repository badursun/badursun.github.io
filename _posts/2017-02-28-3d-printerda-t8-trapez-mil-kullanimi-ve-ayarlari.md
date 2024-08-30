---
layout: post
title: "3D Printer'da T8 Trapez Mil Kullanımı ve Ayarları"
date: 2017-02-28 10:21
categories: ["Teknoloji", "3D-Printer"]
tags: []
toc: true
---

3D Printer'da kullanım yaygınlaştıkça, açık kaynaklı özelliğinden dolayı hep bir adım daha fazla kaliteyi arttırma çabaları görünüyor. Herkesin derdi daha kaliteli bir 3D Printer sahibi olmak, kusursuz baskılar almak. Hızlı olmasını sağlamak falan filan... Öncelikle şunu söylemek istiyorum, Kaliteli olsun, Hızlı Bassın diyorsanız bu ikisi birbirine ters paralel isteklerdir :) Hız arttıkça baskı kaliteniz düşecektir. Bunu eşit şekilde yükseltmek için öncelikle çin malzemelerinden uzak duracaksınız. 2 TL lik lineer rulman ile, 5 TL lik mil, 6 TL lik kayış ve 25 TL lik motor ile alacağınız performans ancak bu kadar olur. Daha kaliteli bir 3D Printer yapmak istiyorsanız, gerçekten kullanacağınız malzemeler de kaliteli olmalı ki bu da doğal olarak cihaz maliyetinizi arttıran bir şey. Şöyle düşünün; 15.000 TL lik şahin mi yoksa 150.000 TL lik audi mi :)

Bugün sizlere kalite arttırma yöntemlerinden birisi olan T8 Trapez mili 3D Printer'inizda kullanıma geçirmeyi anlatacağım. Bildiğiniz gibi Z eksenin de genel olarak metrik 10 (yada metrik 5, ben 10 üzerinden anlatacağım) dişli saplama dediğimiz mil kullanılmakta. Bu mil ve T8 mil arasında ki farkı önce görelim, sonra detayını anlatayım.

## T8 Trapez Mil
![T8 Trapez Mil](assets/img/417kcALH26L._AC_UF894,1000_QL80_.jpg)

Yukarıda ki resimde göreceğiniz gibi T8 mil de 4 Start denilen 4 kanaldan oluşmakta. Dolayısıyla M10 saplama dişliye göre daha stabil, sallantısız ve kısa dönüşde geniş hareket sağlamakta. M10 saplama dişli ise genelde çok pürüzlü ve titreşim sebebi olan pürüzlere sahip. Ayrıca yüksek hareket için daha fazla dönüşe ihtiyaç duyuyor ki bu da hem daha fazla motor dönüş süresi demek hemde baskı süresini de uzatmak demek. Ayrıca önemli bir nokta, T8 vidası ile M10 için kullanılan somunun da arasında oldukça büyük kalite farkı var. Bu farkı aşağıda ki kısa bir video ile örnekleyelim

<p style="text-align: center;"><iframe src="https://www.youtube.com/embed/tWQL4_V37e8?rel=0&amp;showinfo=0" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe></p>

Gördüğünüz gibi T8 mil üzerinde ki somun yağ gibi kaymaktadır. Bu sürtünme sebepli kuvvetin azaltıdığı anlamına gelir ki, pürüssüz (smooth) bir akış sağlar.

Printerinizde T8 mi kullanmına geçmek için bazı ufak modifikasyonlar yapmanız gerekiyor. Bunlardan en önemlisi, Z ekseni taşıyı parçalarının bu mile uygun alanlara ihtiyaç duyması. Dolayısıyla benim gibi Sigma 3D kullanıyorsanız öncelikle Z ekseninizde ki parçaları tekrar basmanız gerekiyor. **[Thingiverse](https://www.thingiverse.com/thing:1765539)** üzerinden bu parçayı temin edebilirsiniz.

Montaj esnasında ihtiyaç duyacağınız bir diğer değişim ise kaplin (couplin) olacak. Çünkü NEMA17 bağlantı noktası 5MM olan kaplinin T8 bağlantısı için 8MM lik bir karşıt noktaya ihtiyacı var. Böylece bağlantıyı yapabilirsiniz. Ama elinizden iş geliyorsa ve tornanız yada tornacı tanıdığınız varsa, daha sağlıklı bir bağlantı için yine 5x5 kaplin kullanıp, T8 milin kapline girecek yerlerini tornada 5MM ye indirmenizi tavsiye ederim. Böylece kaplin içinde vidalarken herhangi bir dişli boşluğundan sebep az da olsa yamuk girmesini engellemiş olursunuz.

## Marlin / Firmware Ayarları
İlk kurulumu tamamladıktan sonra yaşayacağınız en büyük sıkıntı T8 mil için step ayarlarını hesaplamak :) Joseph Prusa bunun için bir hesaplama sistemi yapmış. Sizi çok yormadan gerekli değerleri size veriyor. Tabii test yapıp, kalibrasyona ince ayar vermek sizin işiniz ama yinede fazlasıyla işinizi görece **[bu hesap makinesini](https://www.prusaprinters.org/calculator/#MotorStuffSPML)** kullanın.

Aşağıda ki ekranda kendinize uygun değerleri girin. Leadscrew pitch değeri T8 trapez mil için 8 olacak. DRiver microstepping değerini mutlaka kendi değerlerinize göre girin. Eğer bu konu hakkında bilginiz yoksa kısaca özetliyorum, çünkü bu başlı başına ayrı bir konu. Kalite arttırma konusunda bununla ilgili de size detaylı bilgi vereceğim. Driver microstepping genel olarak piyasadan alacağınız ucuz sürücüler için 1/16 dır. Bu değişim, sürücünün ramps üzerinde girdiği alanın altında bulunan jumper lar ile ayarlanıyor. DRV8825 modeli haricinde bir çok sürücünün altında jumperların üçü de takılı ise 1/16 adımda kullanıyorsunuz demektir. vAyarlarınızı buna göre aşağıdan örnek alarak yapılandırabilirsiniz. Sürücüler için aşağıda ekstra bir görsel daha ekliyorum. Sürücü türünüzden ve jumper ayarlarınızdan emin olabilirsiniz.

Bazı step driverları kabaca grupladım.

DRV8825 haricinde diğer step sürücüler aşağı yukarı birbirine benzer. Bunlar genel olarak maksimum 1/16 adım destekler. Ramps üzerinde taktığınız yerin altında eğer 3 jumper takılı ise büyük ihtimalle 1/16 adımda çalışıyorsunuz demektir. Fakat aynı jumper ayarında DRV8825 sürücü sahibi iseniz ve bu sürücüyü kullanıyorsanız 1/32 adım da kullanıyorsunuz demektir. Yukarıda ki hesap makinesinde buna göre ayarlamalarınızı yapın ve çıkan değeri firmware de ilgili alana yazın. Benim için değer 800 çıktı. Sigma 3D kullanıyorsanız ve DRV8825 ile Trapez T8 mile geçecekseniz, sizinde değeriniz 800 olacaktır. Ama şunu unutmayın; printerların hepsi birbirine benzese de asla birebir aynısı değildir. Benim değerim 800 ise, sizin 810 olabilir. Ve bu minik değer farkları kalitenizi fazlasıyla değiştirebilir.

Umarım T8 mile geçerken sorunsuz bir geçiş yaşamışsınızdır. Bir sonra ki yazımda Step sürücülere değineceğim. Daha kaliteli bir sürüş keyfi için size sürücü tavsiyesi vermeyi planlıyorum.