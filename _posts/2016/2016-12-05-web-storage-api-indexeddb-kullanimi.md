---
layout: post
title: "Web Storage Api - IndexedDB Kullanımı"
date: 2016-12-05 14:35
categories: ["Yazılım", "Javascript"]
tags: ["web-api", "javascript", "database", "local-storage", "web-storage"]
toc: true
---

Kullanımına farklı bir açıdan yaklaştığımız Web Storage API konusunu daha detaylıca örneklendirmeye karar verdim. Örneklendirmemi kod bazlı tutacağım. Yani nasıl varlığı kontrol edilir, nasıl eklenir, nasıl okunur, nasıl düzeltilir, nasıl silinir.

Web storage API 2 tip tanımlar. Bunlar "**local storage**" ve "**session storage**".

Local storage verileri browser’ın önbelleği silinene kadar saklar. Yani dışardan manuel olarak browser’dan silmemiz gerekir.

Session storage verileri browserdaki sekme veya pencere kapananan kadar saklar.

Local storage verileri browser tarafından paylaşırken , örnek olarak chrome’ da saklanan veri başka bir chrome ‘dan okunabilir ve üzerine tekrar veri yazılabilir. Yani local storage ile saklanan veriler bir browser tipi için global bir değerdir.

### Nasıl Kontrol Edilir
```javascript
function localStorageSupported() {
  try {
    return "localStorage" in window && window["localStorage"] !== null;
  } catch (e) {
    return false;
  }
}
```

Bu fonksiyon localStorage veya sessionStorage varlığını kontrol edecek ve size true yada false olarak dönecektir.

### Nasıl Eklenir, Nasıl Güncellenir

```javascript
localStorage.setItem("anahtar", "deger");
sessionStorage.setItem("dolarkuru", 3.14);
localStorage.setItem("bar", true);
sessionStorage.setItem("baz", JSON.stringify(object));
```

Veri ekleme/kaydetme setItem("anahtar","deger") metoduyla gerçekleştirilir. 

Eğer key var olan storage da yoksa bu anahtar/deger pair storage’ a eklenir. 

Eğer varsa , bu anahtara ait değer yeni değerle güncellenir.

### Hataları Kontrol Etmek
```javascript
try {
  localStorage.setItem("key", "value");
} catch (e) {
  alert("Limit Aşıldı");
}
```
Tabii her güzel şeyin sonu gibi, bu API nin de bir sonu, limiti var. Eğer yüklemeye çalıştığınız veri boyutu fazla ise hata alırsınız. Bu hatayı yakalamak için örnek kodu mutlaka kullanmalısınız.

### Veri Alma - Okuma
```javascript
var string = localStorage.getItem("key");
var number = sessionStorage.getItem("foo");
var boolean = localStorage.getItem("bar");
var object = JSON.parse(sessionStorage.getItem("baz"));
```

Kayıt ettiğiniz verileri bu şekilde alıp, okuyabilirsiniz.

### Veri Silme
```javascript
sessionStorage.removeItem("foo");
localStorage.removeItem("bar");
sessionStorage.removeItem("baz");
```
    