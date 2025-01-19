---
layout: post
title: "Classic ASP'de Method Chaining (Zincirleme Metot) Kullanımı"
description: "Method Chaining (Zincirleme Metot), nesne yönelimli programlamada sıkça kullanılan ve kodun okunabilirliğini artıran önemli bir tasarım desenidir."
date: 2025-01-04 09:45
categories: ["Yazilim", "Classic Asp"]
tags: ["asp-method-chaining", "asp-class", "vbscript", "classic-asp", "zincirleme-metot", "fluent-interface"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/classic-aspde-method-chaining-kullanimi-678d2a0582a7c.webp"
  alt: "Classic ASPde Method Chaining (Zincirleme Metot) Kullanımı"
---

Method Chaining (Zincirleme Metot), nesne yönelimli programlamada sıkça kullanılan ve kodun okunabilirliğini artıran önemli bir tasarım desenidir. Bu dokümanda, genellikle modern programlama dillerinde gördüğümüz bu tekniğin Classic ASP'de nasıl uygulanabileceğini inceleyeceğiz.

Yeni başlayanlar için yine bilgilendirme verelim: Classic ASP, Microsoft'un 1996'da tanıttığı ve hala birçok legacy sistemde kullanılan bir web teknolojisidir. Özellikle bankacılık sektöründe bolca kullanılır. Yeni nesil bir çok yazılım dilini parlatmak yerine ben hala Classic ASP'i bol miktarda kullanıyorum. Her kim, "Classic ASP öldü" desede, ben bir yazılım dilinin ölmeyeceğini düşünüyorum ve bunu blogumda bolca savundum, savunmaya devam ediyorum. Farklı diller de biliyorum ve kullanıyorum ama ilk gözağrım olan Classic ASP'nin sanırım ülkemizde hala önde gelen gizli temsilcilerinden birisiyim.

Modern yazılım dillerinde uygulanan yapıları, metodları Classic ASP ile de uygulamak genellikle mümkündür. Sadece uygulamak için core yapısında methodlar olmadığı için ya iyi bir kütüphane bulacaksınız yada oluşturacaksınız. Eğer blogumu zaten takip ediyorsanız, böyle bir kütüphaneyi rahatlıkla oluşturabilirsiniz. Gelelim günün konusuna, Method Chaining (Zincirleme Method) nedir, ne işe yarar, nasıl kullanılır?

## Method Chaining Nedir

Method Chaining, bir nesne üzerinde birden fazla metodun art arda çağrılmasına olanak sağlayan bir programlama tekniğidir. Bu teknik sayesinde, her metot çağrısı aynı nesne üzerinde zincirleme olarak devam eder. Üst kademe bir yazılımcı değilseniz bu method'a daha çok jQuery kütüphanesi kullanımında rastlarsınız.

```javascript
$("#element").css("color", "red").slideUp(2000).slideDown(2000);
```

Mesela en popüler dillerden birisi olan PHP’de "method chaining" dilin kendisinde bulunan bir özellik değildir. Ancak, sınıf metotlarınızda her metot dönüşünde `$this` döndürürseniz method chaining (fluent interface) yapısı elde edebilirsiniz. Yani doğrudan PHP’nin sağladığı bir sihirli özellik yoktur, kodunuzu bu mantığa göre yazmanız gerekir.

Method chaining, çoğu modern nesne yönelimli dilde (Java, JavaScript, Python, Ruby, Swift, Kotlin vb.) uygun şekilde tasarlanmış sınıflarla kullanılabilir. Genelde fluent interface tasarımını benimseyen sınıflar, kendi içinde `this` veya `self` döndüren metotlar yazarak method chaining’i mümkün kılarlar.

Malesef Classic ASP'de, PHP gibi bu dillerin arasında yer almaz. Ama bir Class ile modüler bir yapı kurarak programlama yaptığınızda, method chaining'i kullanabilecek şekilde programınızı dizayn edebilirsiniz.

Örneğin, normal bir class ile kod akışında şöyle yazacağımız bir kodu:

```javascript
Set Math1 = New Matematik
    Math1.Topla(5)
    Math1.Carp(2)
    Math1.Bol(3)
    Sonuc = Math1.Sonuc()
Set Math1 = Nothing
```

Method Chaining ile şu şekilde yazabiliriz:

```javascript
Set Math1 = New Matematik
    Sonuc = Math1.Topla(5).Carp(2).Bol(3).Sonuc()
Set Math1 = Nothing
```

## Classic ASP'de Method Chaining

VBScript tabanlı olan bu dilde de method chaining uygulamak için kullanacağımız temel teknikler:

1. Class yapısı oluşturma
2. Her metodun sınıf örneğini (instance) döndürmesi
3. `Me` anahtar kelimesinin kullanımı

## Me Anahtar Kelimesi

`Me` anahtar kelimesi, Classic ASP'de sınıfın mevcut örneğine (current instance) referans veren özel bir değişkendir. Java'daki `this` veya C#'taki `this` anahtar kelimelerine benzer şekilde çalışır.

Method Chaining'de `Me`'nin önemi:

- Her metot, zincirin devam edebilmesi için sınıf örneğini döndürmelidir
- `Set` anahtar kelimesi ile birlikte kullanılır
- Nesne referansını döndürmek için kullanılır

Örnek kullanım:

```javascript
Public Function Topla(sayi)
    m_sayi = m_sayi + sayi
    Set Topla = Me  ' Sınıf örneğini döndür
End Function
```

## Örnek Uygulama

Aşağıda, dört işlem yapabilen bir matematik sınıfı örneği bulunmaktadır:

```javascript
Class Matematik
    Private m_sayi 'İşlemlerin yapılacağı private sayı değişkeni

    Private Sub Class_Initialize()
        m_sayi = 0 'Sınıf oluşturulduğunda başlangıç değeri 0 olarak atanır
    End Sub

    'Toplama işlemi yapar ve sınıf örneğini (this) döndürür
    Public Function Topla(sayi)
        m_sayi = m_sayi + sayi
        Set Topla = Me 'Method chaining için sınıf örneğini döndür
    End Function

    'Çıkartma işlemi yapar ve sınıf örneğini (this) döndürür
    Public Function Cikart(sayi)
        m_sayi = m_sayi - sayi
        Set Cikart = Me
    End Function

    'Çarpma işlemi yapar ve sınıf örneğini (this) döndürür
    Public Function Carp(sayi)
        m_sayi = m_sayi * sayi
        Set Carp = Me
    End Function

    'Bölme işlemi yapar ve sınıf örneğini (this) döndürür
    Public Function Bol(sayi)
        If sayi <> 0 Then
            m_sayi = m_sayi / sayi
        End If
        Set Bol = Me
    End Function

    'Son işlem sonucunu döndürür
    Public Function Sonuc()
        Sonuc = m_sayi
    End Function
End Class
```

## Kod Açıklaması

1. **Sınıf Yapısı**:

   - `m_sayi`: Özel (private) bir değişken olarak tanımlanmış, işlemlerin sonucunu tutar
   - `Class_Initialize`: Sınıf oluşturulduğunda çalışan yapıcı metot
   - Her matematik işlemi için ayrı bir metot (Topla, Cikart, Carp, Bol)
   - `Sonuc` metodu final değeri döndürür

2. **Method Chaining Mekanizması**:

   - Her metot (Sonuc hariç) `Set MethodAdi = Me` ile kendisini döndürür
   - Bu sayede metodlar zincirleme çağrılabilir
   - Son metot olan `Sonuc()` sayısal değeri döndürür

3. **Kullanım Örneği**:

```vbscript
Dim mat
Set mat = New Matematik
sonuc = mat.Topla(10).Cikart(5).Carp(2).Bol(2).Sonuc()
Set mat = Nothing
```

Son olarak bir de şöyle güzel satır satır yazma örneği ekleyelim :) Sonra demesinler, tek satırda yazmak zorundasın diye..

```vbscript
Set Math0 = New Matematik
    Response.Write Math0.Topla(10)_
        .Cikart(5)_
        .Carp(2)_
        .Bol(2)_
        .Sonuc()
Set Math0 = Nothing
```

## Avantajları ve Kullanım Alanları

1. **Kod Okunabilirliği**:

   - İşlemlerin akışı daha net görülür
   - Kod daha kompakt ve anlaşılır hale gelir

2. **Bakım Kolaylığı**:

   - Her metot bağımsız olarak güncellenebilir
   - Yeni metodlar kolayca eklenebilir

3. **Yaygın Kullanım Alanları**:

   - Veritabanı sorgu oluşturucular
   - Matematiksel işlemler
   - Form doğrulama işlemleri
   - HTML oluşturma işlemleri

4. **Performans**:
   - Her metot çağrısında yeni bir nesne oluşturulmaz
   - Aynı nesne üzerinde işlemler devam eder

Method Chaining, Classic ASP gibi eski bir teknolojide bile modern programlama tekniklerinin uygulanabileceğini göstermektedir. Bu teknik sayesinde daha okunabilir, bakımı kolay ve profesyonel kod yazabilirsiniz.

Zaten bugüne kadar yazdığım yazıları okuduysanız, çok eski bir yazılım dili olan Classic ASP'nin de neler yapabileceğini, bu dilin günümüzde halen kullanılabilir olduğunu sizlere göstermek istediğimi anlarsınız.

Classic ASP, ilk bakışta eskimiş ve Microsoft’un resmi desteğini yitirmiş gibi görünse de belli avantajlarıyla günümüzde hâlâ kullanılabilir. Öncelikle, hâlihazırda Classic ASP ile yazılmış ve sorunsuz işleyen çok sayıda proje var. Bu projeleri tamamen yeniden yazmak yerine bakımlarını sürdürmek, zamandan ve bütçeden tasarruf sağlar. Ayrıca, dil yapısının sadeliği ve sunucu tarafında çalışan temel script mantığı, hızlı geliştirme ve temel web ihtiyaçlarına kolayca çözüm üretme imkânı tanır.

Modern dillerin her ne kadar geniş ekosistemleri ve güncel topluluk desteği olsa da, Classic ASP ile ortaya çıkan güvenlik açıklarının çoğu, iyi bir bakım ve düzenli güncellemelerle kontrol altında tutulabilir. Sunucu maliyetleri de ASP.NET gibi çözümlere göre nispeten düşük kalabilir. Eğer proje gereksinimleri büyük bir dönüşümü mecbur kılmıyorsa ve sistem kararlı şekilde çalışıyorsa, Classic ASP’yi devam ettirmek hâlâ makul bir tercih olabilir.

Özetle, eğer eldeki uygulama ölçek olarak büyük bir refaktör ihtiyacı doğurmuyorsa, Classic ASP’nin mevcut kod tabanını korumak, ekibe ek yük getirmeden hızlıca güncellemek ve belirli bir amaca yönelik sade projelerde kullanmak hâlâ geçerli bir seçenektir. Bu tür projelerde Classic ASP, beklentileri karşılayan bir çözüm olarak yaşam döngüsünü sürdürmeye devam edebilir.
