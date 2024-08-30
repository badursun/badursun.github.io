---
layout: post
title: "jQuery ile Parallax Arka Plan Yapımı"
date: 2017-02-02 09:57
categories: ["Yazılım", "jQuery"]
tags: ["jquery", "javascript", "parallax", "css", "html"]
---

Bir çok web sitesinde Parallax (hareketli arka plan) kullanıldığını görmüşsünüzdür. Hatta "hangi plugin bu acaba" dediğiniz de olmuştur. Veya kendinize uygun hazır bir kod bulamamış olabilirsiniz. Sorun yok, jQuery size yeter. Her zaman dediğim gibi, iyi bir programcı olmak; hazır kodları iyi kullanmayı bilmek değildir. Ortaya çıkan sonucun nasıl yapıldığını algoritmik olarak çözümleyebilmektir. Yani bir ATM makinası gördüğünüzde, o process'lerin nasıl yapıldığını kafanızda canlandırabiliyorsanız, o zaman gelişme var demektir.

Parallax arka planların mantığı çok belli değil mi?

## Nedir Parallax?
Parallax veya dilimizdeki karşılığı olan paralaks, gözlemcinin yerinin değişmesine bağlı olarak bir cismin gözlenen doğrultusunda meydana gelen değişime denilmektedir. Gerçek konum ile görülen konum arasında fark oluşması şeklinde de anlamlandırılır. Aslında bütün olay bir optik yanılsamada gizlidir.

Paralaks olayını kavramamız açısından bir örnek vermek gerekirse şekilde görüldüğü gibi uzaktaki bir cisme sağ gözümüzle sonrasında sol gözümüzle baktığımızda cismin ellerimizin arasında aynı doğrultuda olmadığı görürüz. Paralaks kaydırma tekniğiyle hızlı nesneleri önde, yavaş nesneleri de arka planda hareketlenmesi sonucu 3 boyutlu bir site tasarımı elde etmek mümkün olmaktadır. Bu hareketlendirme mouse’umuzda bulunan scroll‘un aşağı ve yukarı hareketlenmesi sonucunda farklı hızda nesnelerin kaymasından meydana gelmektedir. [1](https://webmaster.kitchen/paralaks-web-tasarimi-nedir/)

## Örnek: Basit Parallax Uygulaması
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>jQuery Parallax</title>
</head>
<body>
    <div id="parallax" style="width:100%; height:100px; background: url('asset/background-image.jpg') repeat;"></div>

    <script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
    <script>
    $(document).ready(function() {
        $('#parallax').mousemove(function(e) {
            $(this).css('background-position', `${e.pageX / 10}px ${e.pageY / 10}ğx`);
        });
    });
    </script>
</body>
</html>
```

jQuery mousemove fonksiyonu ihtiyacınız olan mouse kordinat bilgilerini size vermektedir. Bu fonksiyonu parallax kimlikli elemente tanımladığımızda, o elementin üzerinde parallax efektinin çalışmasını sağlayacaktır. X ve Y kordinatları alarak arka planın mevcut konumuna +-10px lik bir hareket vermiş oluyoruz. Bunu komple body elementine giydirirsek aynı etkiyi sitenin genel arka planı içinde alabiliriz. 

Fakat önemli olan bir nokta var, arada bulunan 10px fark, hat sonuna geldiğinizde boş arka plan çıkmasına sebep verecektir. Bu yüzden kullandığınız arka planın normalde görünen kısmından, her taraf için 10px daha büyük olmasına önem verin, böylece o taşmalar görünmez olacaktır. Bunu background-size ile de yapabilirsiniz.

Parallax tasarımın dezavantajları da vardır. Parallax tam olarak mobil uyumlu değildir ve responsive uyumluluk için büyük uğraş gerektirir. Gereksiz zaman kaybı yaratır. Geç yüklenmeye neden olabilir. Çok SEO dostu olduğu söylenemez. Ama yine de göze güzek görünen bir efekt, akıllıca tüketmekte fayda var.