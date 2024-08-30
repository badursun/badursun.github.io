---
layout: post
title: "Javascript Fonksiyonu Tanımlamak İçin Farklı Yöntemler"
date: 2024-08-30 14:47
categories: ["Yazılım", "Javascript"]
tags: ["iife", "arrow-function", "function", "javascript", "es6", "ecma-script"]
toc: true
---

# Bir fonksiyonun anatomisi
JavaScript'te fonksiyonlar, kod tekrarını önlemek ve işlevselliği modüler hale getirmek için kullanılan temel yapılar arasındadır. Bu yazıda, fonksiyon tanımlamanın çeşitli yöntemlerine göz atacağız. Her yöntemin kendine has avantajları ve kullanım durumları bulunmaktadır.

Fonksiyon isim ve gövde olarak iki parçadan oluşur. İsim kısmından fonksiyona parametre geçişi sağlanabilir. Gövde üzerinde de çalıştırılacak olan kodlar yürütülmektedir. Bir fonksiyona parametre geçişi olabildiği gibi fonksiyon isimleri üzerinden değer dönüşü de yapılabilmektedir.

Bir javascript fonksiyonu çağırmak için fonksiyon adı ve fonksiyonunun aldığı parametreleri bilmeniz gerekir.

## Geleneksel Fonksiyon Bildirimi
Geleneksel fonksiyon bildirimi, JavaScript'in en yaygın kullanılan ve bilinen fonksiyon tanımlama yöntemidir. Bu yöntem, `function` anahtar kelimesiyle başlar ve oldukça esnektir.

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

console.log(greet("Burak")); // Output: Hello, Burak!
```

Bu yöntem, hoisting (değişkenlerin ve fonksiyonların bildirimlerinin üst sıralara taşınması) nedeniyle, fonksiyonun tanımlanmasından önce çağrılmasına olanak tanır.

## Arrow Fonksiyonlar
ES6 ile gelen arrow fonksiyonlar, daha kısa ve okunabilir bir sözdizimi sunar. Ancak bu fonksiyonlar, **this** bağlamını muhafaza etme gibi bazı farklılıklara sahiptir, bu da onları anonim fonksiyonlar için ideal hale getirir.

```javascript
const greet = (name) => {
    return `Hello, ${name}!`;
}

console.log(greet("Burak")); // Output: Hello, Burak!
```

Eğer fonksiyon sadece tek bir ifadeye sahipse, {} parantezlerini ve return anahtar kelimesini atlayabilirsiniz:

```javascript
const greet = name => `Hello, ${name}!`;

console.log(greet("Burak")); // Output: Hello, Burak!
```

## Fonksiyon İfadeleri
Fonksiyon ifadeleri, bir değişkene atanarak tanımlanan fonksiyonlardır. Bu, esneklik sağlar ve fonksiyonlarınızı kodun akışına daha uygun şekilde tanımlamanıza imkan verir.

```javascript
const greet = function(name) {
    return `Hello, ${name}!`;
}

console.log(greet("Burak")); // Output: Hello, Burak!
```

Fonksiyon ifadeleri, hoisting yapmaz, bu yüzden tanımlamadan önce kullanılamazlar.

## Kapsülleme (IIFE - Immediately Invoked Function Expression)
Hemen çağrılan fonksiyon ifadeleri (IIFE), tanımlandıkları anda yürütülen fonksiyonlardır. Genellikle değişkenlerin global kapsamdan izole edilmesi amacıyla kullanılır.

```javascript
(function() {
    console.log("Bu fonksiyon hemen çalışır!");
})();
```

Bu yapı, global kapsamı kirletmeden bir dizi işlemi gerçekleştirmek için idealdir.

## Class Yöntemleri
JavaScript sınıfları, metodlarıyla birlikte tanımlanan fonksiyonlara sahip olabilir. Bu metodlar, bir sınıfın örnekleri (instances) üzerinde işlemler gerçekleştirmek için kullanılır.

```javascript
class Badursun {
    constructor(name) {
        this.name = name;
    }

    Selamla() {
        return `Hello, ${this.name}!`;
    }
}

const Selamlayan = new Badursun("Burak");
console.log(Selamlayan.Selamla()); // Output: Hello, Burak!
```

Sınıf yöntemleri, objelerle çalışırken güçlü bir yapı sağlar ve nesne yönelimli programlama (OOP) konseptlerini uygulamanıza imkan tanır.

JavaScript'te fonksiyonlar, çeşitli şekillerde tanımlanabilir ve her bir yöntemin kendine özgü avantajları bulunmaktadır. Geleneksel fonksiyonlar, arrow fonksiyonlar, fonksiyon ifadeleri, IIFE'ler ve sınıf yöntemleri, her biri farklı senaryolar için uygun olabilir. Kendi projenizde hangi yöntemi kullanmanız gerektiğine karar verirken, fonksiyonun kapsamı, bağlamı ve esnekliği gibi faktörleri göz önünde bulundurmanız faydalı olacaktır.