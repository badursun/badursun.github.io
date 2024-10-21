---
layout: post
title: "QNBPay ve NodeJS ile Hash Hesaplama"
date: 2024-10-21 12:28
categories: ["Yazılım", "NodeJS"]
tags: ["yazilim", "javascript", "nodejs", "qnbpay", "qnb", "sanal-pos"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/qnbpay-nodejs-hash-hesaplama.webp"
  alt: "QNBPay ve NodeJS ile Hash Hesaplama"
---

Bu yazıyı yazmamın üç temel nedeni var.

Birincisi, mesleğim boyunca karşılaştığım zorlukların çoğunu, yoğun çaba ve araştırmalar sonucu kendim çözmek zorunda kaldım. Ne yazık ki ülkemizde, özellikle teknik konularda yeterli yardım ve rehberlik bulmak oldukça zor. Bu yüzden, benim gibi zorluk çeken diğer yazılımcılara destek olabilecek bir kaynak ve arşiv oluşturma isteğiyle bu blogu tutuyorum.

İkincisi, büyük firmalarla çalışan kurumsal müşterilere entegrasyon gibi teknik destekler sağlarken, bu firmaların genellikle hatalı ve eksik dökümantasyonlar sunduğunu görüyorum. Ne yazık ki bu hataları onlara iletsek bile, dökümanlar güncellenmiyor ya da düzeltme yapmıyorlar. Çünkü kurumsal kültürde birçok kişi sadece "üstüne düşeni" yapıyor ve ötesine geçmiyor. Bu durumu "salla başı, al maaşı" olarak nitelendiriyorum. Amacım, bu yazılar aracılığıyla belki kurumlar hatalarını fark eder ve düzeltir, ya da bu hatalara sessiz kalanlara bir ilham olur.

Üçüncüsü, yazılımcı olmayı ve öğrendiklerimi paylaşmayı seviyorum. Paylaşımın yazılım dünyasında ne kadar önemli olduğunu bildiğim için, bilgimi ve deneyimlerimi başkalarıyla paylaşmayı görev ediniyorum.

Çok uzatmadan konuya girelim;

# Node.js ile Hash Key Üretimi: Güvenli Şifreleme Adımları
Hash key üretimi, verilerin güvenliğini sağlamak için kullanılan önemli bir tekniktir. Bu yazıda, banka gibi ödeme sistemleri ile entegre çalışırken, doğru ve güvenli bir şekilde hash key oluşturmayı ele alacağız ve QNBPay entegrasyonu esnasında eksik dökümanları yüzünden karşılaştığım sorunu aşmak için yazdığım kodu sizlerle paylaşacağım. 

Aşağıda adım adım, bir `generateHashKey` fonksiyonunun nasıl oluşturulacağını göreceksiniz.

## 1. Fonksiyonun Amacı Nedir?

`generateHashKey` fonksiyonu, belirli parametreler (toplam tutar, taksit, para birimi, fatura ID gibi) ile birlikte bir gizli anahtar (app_secret) kullanarak bir hash key oluşturur. Bu hash key, işlemlerin güvenliğini sağlamak için kullanılır. Fonksiyon, bu parametreleri şifreler ve güvenli bir şekilde dışa aktarır.

## 2. Parametrelerin Hazırlanması

Fonksiyonun giriş parametreleri şu şekilde tanımlanır:

- `total`: İşlemin toplam tutarı.
- `installment`: Taksit sayısı.
- `currency_code`: Kullanılan para birimi (örneğin "TRY").
- `merchant_key`: Satıcı anahtarı.
- `invoice_id`: Fatura ID'si.
- `app_secret`: Güvenli anahtar (sistemden gelen gizli bilgi).

Bu parametreleri kullanarak, şifrelemek istediğimiz veriyi oluşturuyoruz:

```javascript
const data = `${total}|${installment}|${currency_code}|${merchant_key}|${invoice_id}`;
```

Bu yapı, QNBPay için verilerin '|' ile ayrıldığı bir string haline getirir.

## 3. IV (Initialization Vector) Oluşturma

IV, şifreleme algoritmalarında kullanılan başlangıç değeridir. Bu fonksiyon, IV'yi SHA1 hash algoritması kullanarak oluşturur. İşte IV'nin oluşturulma adımı:

```javascript
const randNumIv = Math.floor(Math.random() * (99999999999999999 - 10000000000000000) + 10000000000000000).toString();
const hashIv = crypto.createHash('sha1').update(randNumIv).digest('hex');
const iv = hashIv.substring(0, 16);  // İlk 16 karakteri alıyoruz
```

Burada `Math.random()` ile rastgele bir sayı üretilir, ardından bu sayı SHA1 ile hashlenir ve ilk 16 karakteri IV olarak kullanılır.

## 4. Salt ve Parola Oluşturma

Salt, şifreleme işlemlerini daha güvenli hale getirmek için kullanılan rastgele bir değerdir. Parola (`app_secret`) SHA1 ile hashlenir ve salt ile birleştirilir. Salt oluşturma işlemi şu şekildedir:

```javascript
const passwordHash = crypto.createHash('sha1').update(app_secret).digest('hex');  // Gizli anahtarın hashlenmesi
const randNumSalt = Math.floor(Math.random() * (99999999999999999 - 10000000000000000) + 10000000000000000).toString();
const hashSalt = crypto.createHash('sha1').update(randNumSalt).digest('hex');
const salt = hashSalt.substring(0, 4);  // Salt olarak ilk 4 karakteri alıyoruz
```

Burada rastgele bir sayı hashlenir ve salt değeri oluşturulur. Salt ve hashlenmiş parola daha sonra birleştirilip SHA256 algoritmasıyla tekrar hashlenir:

```javascript
const saltWithPassword = crypto.createHash('sha256').update(passwordHash + salt).digest('hex').substring(0, 32);
```

Bu aşama, parola ve salt'ın bir araya getirilip daha güçlü bir hash ile işlenmesidir.

## 5. AES-256-CBC ile Şifreleme

Verinin şifrelenmesi için `aes-256-cbc` algoritması kullanılır. Burada şifreleme yapılır ve sonuç `base64` formatında kodlanır:

```javascript
const cipher = crypto.createCipheriv('aes-256-cbc', Buffer.from(saltWithPassword, 'utf-8'), Buffer.from(iv, 'utf-8'));
let encrypted = cipher.update(data, 'utf-8', 'base64');
    encrypted += cipher.final('base64');
```

Bu adımda, veri AES-256-CBC algoritması ile şifrelenir ve `base64` formatında döndürülür.

## 6. Sonucun Birleştirilmesi ve Dönüştürülmesi

Son olarak, IV, salt ve şifrelenmiş veri birleştirilip sonuç döndürülür:

```javascript
let msg_encrypted_bundle = `${iv}:${salt}:${encrypted}`;
    msg_encrypted_bundle = msg_encrypted_bundle.replace(/\//g, '__');
```
Burada şifrelenmiş veri `/` karakteri içeriyorsa, bu karakter `__` ile değiştirilir.

## 7. Sonuç
Bu adımları tamamladığınızda, fonksiyon size şifrelenmiş ve güvenli bir şekilde iletilebilecek bir hash key döndürecektir. İşte fonksiyonun tamamı:

```javascript
const crypto = require('crypto');

function generateHashKey(total, installment, currency_code, merchant_key, invoice_id, app_secret) {
    // Veriyi oluşturuyoruz
    const data = `${total}|${installment}|${currency_code}|${merchant_key}|${invoice_id}`;

    // IV oluşturma (SHA1 hash'in ilk 16 karakteri)
    const randNumIv = Math.floor(Math.random() * (99999999999999999 - 10000000000000000) + 10000000000000000).toString();
    const hashIv = crypto.createHash('sha1').update(randNumIv).digest('hex');
    const iv = hashIv.substring(0, 16);

    // app_secret SHA1 hash ile şifreleniyor
    const passwordHash = crypto.createHash('sha1').update(app_secret).digest('hex');

    // Salt oluşturma (SHA1 hash'in ilk 4 karakteri)
    const randNumSalt = Math.floor(Math.random() * (99999999999999999 - 10000000000000000) + 10000000000000000).toString();
    const hashSalt = crypto.createHash('sha1').update(randNumSalt).digest('hex');
    const salt = hashSalt.substring(0, 4);

    // Password ve salt birleştirilip SHA256 hash ile şifreleniyor
    const saltWithPassword = crypto.createHash('sha256').update(passwordHash + salt).digest('hex').substring(0, 32);

    // AES-256-CBC ile şifreleme
    const cipher = crypto.createCipheriv('aes-256-cbc', Buffer.from(saltWithPassword, 'utf-8'), Buffer.from(iv, 'utf-8'));
    let encrypted = cipher.update(data, 'utf-8', 'base64');
    encrypted += cipher.final('base64');

    // IV, salt ve şifrelenmiş veriyi birleştirip sonucu döndürüyoruz
    let msg_encrypted_bundle = `${iv}:${salt}:${encrypted}`;
    msg_encrypted_bundle = msg_encrypted_bundle.replace(/\//g, '__');

    return msg_encrypted_bundle;
}

// Örnek kullanım
const total = "100.00";
const installment = "1";
const currency_code = "TRY";
const merchant_key = "your_merchant_key";
const invoice_id = "your_invoice_id";
const app_secret = "your_app_secret";

console.log(generateHashKey(total, installment, currency_code, merchant_key, invoice_id, app_secret));
```

QNBPay ile hash oluşturma işlemlerinde sorun yaşıyorsanız ve https://apidocs.qnbpay.com.tr/#/paymentcontrolrefund/securepayment adersinde ki `Hash Key Oluşturma Kod Örnekleri` döküman halen güncellenmemişse, yukarıda verdiğim fonksiyonu NodeJS projenizde kullanabilirsiniz.