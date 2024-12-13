---
layout: post
title: "JavaScript`te const ile Tanımlanan Fonksiyonların Hoisting Problemi"
description: "JavaScript'te hoisting, değişken ve fonksiyon bildirimlerinin kodun çalıştırılma anında kapsamın başına taşınmasıdır. Ancak bu taşınma işlemi yalnızca bildirimi kapsar, atamalar taşınmaz."
date: 2024-12-13 09:30
categories: ["Yazilim", "Javascript"]
tags: ["yazilim", "javascript", "hoisting", "functions"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/javascriptte-const-ile-tanimlanan-fonksiyonlarin-hoisting-problemi-675c04a3d410e.webp"
  alt: "JavaScript`te const ile Tanımlanan Fonksiyonların Hoisting Problemi"
---

# JavaScript'te const ile Tanımlanan Fonksiyonların Hoisting Problemi

JavaScript'in güçlü yapısı, bazen geliştiriciler için karmaşık durumlara yol açabilir. Bu durumların başında "hoisting" kavramı gelir. Hoisting, özellikle `const` ile tanımlanan fonksiyonlar söz konusu olduğunda beklenmeyen hatalara neden olabilir. Bu yazıda, hoisting'in nasıl çalıştığını ve `const` ile tanımlanan fonksiyonlarda neden sorun yarattığını, bu sorunları aşmak için hangi yöntemlerin kullanılabileceğini ele alacağız.

## Hoisting ve Temporal Dead Zone

JavaScript'te hoisting, değişken ve fonksiyon bildirimlerinin kodun çalıştırılma anında kapsamın başına taşınmasıdır. Ancak bu taşınma işlemi yalnızca bildirimi kapsar, atamalar taşınmaz. Örneğin:

```javascript
console.log(deger); // undefined
var deger = 5;
```

Bu kod JavaScript motoru tarafından şu şekilde işlenir:

```javascript
var deger;
console.log(deger); // undefined
deger = 5;
```

`let` ve `const` değişkenleri ise farklı bir yaklaşım sergiler. Tanımlanmadıkları süre boyunca Temporal Dead Zone (TDZ) adı verilen bir alanda bulunurlar ve bu süre içinde onlara erişim girişimi `ReferenceError` ile sonuçlanır:

```javascript
console.log(sayi); // ReferenceError
let sayi = 10;
```

Bu mekanizma, `const` ile tanımlanan fonksiyonlar için de geçerlidir.

## Fonksiyonların Tanımlanma Yöntemleri

JavaScript'te fonksiyonlar, genellikle iki yöntemle tanımlanır: **Function Declaration** ve **Function Expression**. Her iki yöntemin davranışı ve hoisting'e etkisi birbirinden farklıdır.

### Function Declaration

Function declaration, fonksiyonun klasik bir yöntemle tanımlanmasıdır. Bu yöntem hoisting özelliği taşır; yani fonksiyonun tanımı, kodun herhangi bir yerinden çağrılabilir:

```javascript
merhabaDeyin(); // Çıktı: Merhaba, dünya!

function merhabaDeyin() {
    console.log("Merhaba, dünya!");
}
```

Burada `merhabaDeyin` fonksiyonu tanım sırasından bağımsız olarak çağrılabilir. Bu, hoisting sayesinde mümkündür.

### Function Expression

Function expression, bir fonksiyonun değişkene atanarak tanımlandığı yöntemdir. Eğer değişken `const` veya `let` ile tanımlanırsa, hoisting gerçekleşmez. Bu da tanımlama sırasına dikkat edilmesi gerektiği anlamına gelir:

```javascript
merhabaDeyin(); // ReferenceError: Cannot access 'merhabaDeyin' before initialization

const merhabaDeyin = function() {
    console.log("Merhaba, dünya!");
};
```

Function expression yönteminde, Temporal Dead Zone nedeniyle fonksiyona tanımlanmadan önce erişilemez.

## const ile Tanımlanan Fonksiyonların Sorunları

`const` ile tanımlanan fonksiyonlarda, tanımlamadan önce yapılan çağrılar `ReferenceError` ile sonuçlanır. Bu durum, özellikle birbirine bağlı fonksiyonların olduğu projelerde sorun yaratabilir. Örneğin:

```javascript
function selamla() {
    hosgeldin(); // ReferenceError
}

const hosgeldin = function() {
    console.log("Hoş geldiniz!");
};

selamla();
```

Burada `hosgeldin` fonksiyonu henüz tanımlanmamış olduğundan `selamla` fonksiyonu hata verecektir.

## Çözüm Yöntemleri

### Tanımlama Sırasına Dikkat Edin

Fonksiyonların önce tanımlanıp sonra çağrılmasını sağlamak, bu tür hataları önlemenin en etkili yoludur:

```javascript
const hosgeldin = function() {
    console.log("Hoş geldiniz!");
};

selamla();

function selamla() {
    hosgeldin();
}
```

### Function Declaration Kullanımı

Eğer fonksiyonlarınızın hoisted edilmesini istiyorsanız, function declaration yöntemini tercih edebilirsiniz:

```javascript
function hosgeldin() {
    console.log("Hoş geldiniz!");
}

function selamla() {
    hosgeldin();
}

selamla();
```

Bu yöntem, tanımlama sırasından bağımsız olarak fonksiyonların çağrılabilmesini sağlar.

### Modüler Yaklaşım Kullanın

Fonksiyonlarınızı modüler bir yapıya taşımak, kodun daha okunabilir ve yönetilebilir olmasını sağlar. ES6 modülleri bu konuda oldukça etkilidir:

**hosgeldin.js**
```javascript
export const hosgeldin = () => {
    console.log("Hoş geldiniz!");
};
```

**selamla.js**
```javascript
import { hosgeldin } from "./hosgeldin.js";

export const selamla = () => {
    hosgeldin();
};
```

Bu yapı, hem hata riskini azaltır hem de projeyi daha modüler hale getirir.

### Linter Araçlarından Yararlanın

Linting araçları, kodunuzu analiz ederek olası hataları önceden tespit edebilir. Eslint gibi araçlar, tanımlanmadan önce kullanılan değişkenleri veya fonksiyonları tespit ederek sizi uyarır:

```javascript
"rules": {
    "no-use-before-define": "error"
}
```

Linting araçlarını projelerinize dahil ederek hem kod kalitesini artırabilir hem de hataları minimize edebilirsiniz.

JavaScript'te `const` ile tanımlanan fonksiyonlar, hoisting mekanizması nedeniyle bazı sınırlamalar taşır. Bu sınırların farkında olarak kod yazmak, daha güvenilir ve sürdürülebilir projeler geliştirmenize olanak tanır.

