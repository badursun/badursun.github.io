---
layout: post
title: "Optional Chaining Nedir? Kullanımı ve Örnekleri"
description: "Bu yazıda, Optional Chaining’in nasıl çalıştığını, hangi dillerde desteklendiğini ve nasıl kullanılabileceğini detaylıca inceleyeceğiz."
date: 2025-02-13 09:45
categories: ["Yazilim", "Javascript"]
tags: ["yazilim", "javascript", "optional-chaining", "typescript", "es2020"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/optional-chaining-nedir-kullanimi-ve-ornekleri-67aee9a3e2b54.webp"
  alt: "Optional Chaining Nedir? Kullanımı ve Örnekleri"
---

JavaScript ve diğer modern dillerde "Optional Chaining" (?.) operatörü, kod yazarken güvenliği artıran ve hata riskini azaltan güçlü bir özelliktir. Bu yazıda, Optional Chaining’in nasıl çalıştığını, hangi dillerde desteklendiğini ve nasıl kullanılabileceğini detaylıca inceleyeceğiz.

Optional Chaining, bir nesne ya da veri yapısının içindeki bir özelliğe veya metoda erişmeye çalışırken, o özelliğin tanımlı olup olmadığını kontrol eden bir operatördür. Eğer erişilmeye çalışılan özellik veya metod tanımlı değilse, `undefined` döndürerek hataların önüne geçer.

## 1. Neden Optional Chaining Kullanmalıyız?

Eskiden, iç içe geçmiş nesne veya array yapıları üzerinde erişim sağlarken manuel olarak `undefined` veya `null` olup olmadığını kontrol etmek gerekirdi. Ancak Optional Chaining, bu tür kontrolleri gereksiz hale getirir ve kodu daha okunabilir hale getirir.

**Önceden manuel kontrol:**

```javascript
const user = { profile: { name: "Burak" } };
console.log(user.profile && user.profile.name); // "Burak"
console.log(user.address && user.address.street); // undefined
```

**Optional Chaining ile:**

```javascript
console.log(user?.profile?.name); // "Burak"
console.log(user?.address?.street); // undefined, hata oluşmaz
```

## 2. Optional Chaining Hangi Dillerde Destekleniyor?

Optional Chaining yalnızca JavaScript’e özgü değildir. Aşağıdaki modern dillerde de benzer işlemler yapılabilir:

- **JavaScript (ES2020+)** – `?.`
- **TypeScript** – `?.`
- **Python (PEP 505 - önerildi ama resmi olarak kabul edilmedi)** – `?.` alternatifi `get()` metoduyla yapılabilir.
- **C# (?. operatörü)**
- **Swift (Optional Chaining ?.)**
- **Kotlin (Safe Call Operator ?.)**
- **Dart (?. operatörü)**
- **Ruby (dig methodu alternatifi)**

Her dilde tam olarak aynı sözdizimi olmasa da, aynı amaca hizmet eden operatörler bulunur.

## 3. JavaScript’te Optional Chaining Kullanımı

JavaScript, `?.` operatörünü ES2020 ile birlikte desteklemeye başladı. Bu operatör üç temel alanda kullanılabilir:

### 3.1. Nesne Özelliklerine Erişim

```javascript
const user = { profile: { name: "Burak" } };
console.log(user?.profile?.name); // "Burak"
console.log(user?.address?.street); // undefined, hata oluşmaz
```

Eğer `user.address` tanımlı değilse, `.street` özelliğine ulaşmaya çalışırken hata alınmaz, doğrudan `undefined` döndürülür.

### 3.2. Dizi Elemanlarına Erişim

```javascript
const users = [{ name: "Burak" }, { name: "Ali" }];
console.log(users?.[0]?.name); // "Burak"
console.log(users?.[5]?.name); // undefined
```

Eğer dizi elemanı yoksa, hata vermeden `undefined` döndürülür.

### 3.3. Fonksiyon Çağırımı

Optional Chaining, bir nesne içinde tanımlı olmayan metodları çağırırken de hata almamak için kullanılabilir:

```javascript
const person = {
  speak: () => "Merhaba!",
};
console.log(person.speak?.()); // "Merhaba!"
console.log(person.walk?.()); // undefined, hata yok
```

Burada `walk` metodu tanımlı olmadığından çağırma işlemi yapılmaz ve `undefined` döner.

## 4. Optional Chaining ve Nullish Coalescing (??) Kullanımı

Optional Chaining, `undefined` veya `null` döndürdüğünde, varsayılan bir değer atamak için **Nullish Coalescing (`??`)** operatörü ile birlikte kullanılabilir.

```javascript
const user = { name: "Burak" };
console.log(user?.address ?? "Adres bulunamadı"); // "Adres bulunamadı"
```

## 5. Hangi Durumlarda Kullanılmalı?

### Kullanılması Gereken Durumlar:
- API’den gelen verileri işlerken bilinmeyen yapıdaki nesnelere erişimde.
- İç içe geçmiş nesnelerde herhangi bir özelliğin olup olmadığını kontrol ederken.
- Opsiyonel olarak var olabilecek metodları çağırırken.
- Tanımsız olabilecek dizilere veya elemanlarına erişirken.

### Kullanılmaması Gereken Durumlar:
- Eğer nesne yapısı kesin olarak biliniyorsa ve belirli özelliklerin her zaman var olacağı garantileniyorsa.
- Performans açısından kritik noktalarda (Her ne kadar fark minimal olsa da, gereksiz `?.` kullanımı performansı az da olsa düşürebilir).

## 6. Alternatif Yöntemler

Optional Chaining desteği olmayan eski tarayıcılarda veya dillerde aşağıdaki yöntemler kullanılabilir:

### 6.1. Manuel Kontrollü Yaklaşım

```javascript
if (user && user.profile && user.profile.name) {
  console.log(user.profile.name);
} else {
  console.log("Bilinmeyen kullanıcı");
}
```

Bu yöntem çalışsa da, kodun okunabilirliğini düşürür.

### 6.2. Lodash get() Metodu

Lodash kütüphanesi ile aşağıdaki gibi güvenli bir erişim sağlanabilir:

```javascript
const _ = require("lodash");
const user = {};
console.log(_.get(user, "profile.name", "Bilinmeyen kullanıcı")); // "Bilinmeyen kullanıcı"
```

## 7. Sonuç

Optional Chaining (?.) operatörü, modern JavaScript’in okunabilirliğini ve güvenilirliğini artıran en önemli yeniliklerden biridir. API’lerden gelen verileri işlerken, opsiyonel metodları çağırırken veya derin nesne erişimleri yaparken büyük kolaylık sağlar.

Eğer projenizde eski tarayıcıları desteklemeniz gerekiyorsa Babel gibi transpiler’lar ile ES2020 özelliklerini eski tarayıcılarda çalışabilir hale getirebilirsiniz.

Optional Chaining modern yazılım geliştirme süreçlerinde kod hatalarını azaltmak ve okunabilirliği artırmak için oldukça faydalı bir özelliktir. Eğer hala kullanmıyorsanız, projelerinizde denemenizi öneririm!