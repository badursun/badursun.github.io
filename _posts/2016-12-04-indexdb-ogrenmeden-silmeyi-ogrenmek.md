---
layout: post
title: "IndexDB Öğrenmeden, Silmeyi Öğrenmek"
date: 2016-12-04 11:29
categories: ["Yazılım", "Javascript"]
tags: ["web-api", "javascript", "database", "local-storage", "web-storage", "indexdb"]
---

Herşeyi baştan anlatmayı çok sevmiyorum, kötü bir huy fakat ihtiyacınız olduğunda aradığınız tek örneğe denk gelmek güzeldir diye düşündüğüm için bu şekilde bir çanta olması için Kod Bankası bölümünü oluşturdum. IndexDB nedir, ne işe yarar, nasıl çalışır kısaca anlatalım ve ilk örneğimizi verelim; IndexDB den kayıt silmek.

IndexDB (web storage) türkçe söylersek, web veritabanı. Yani bilgileri kullanıcıların tarayıcısında saklamak da diyebiliriz.

Web uygulamaların da client tarafta veri saklamak veya uygulamayı offline şekilde çalıştırmak için HTML5 bize bu konuda yeni apiler sunmakta. Bunlar Web Storage , Web SQL , IndexedDB , File System api’ leridir. Biz IndexedDB ile başlıyoruz ve hızlıca bitiriyoruz.

Hızlı kullanım için **dbTemizle** adlı bir fonksiyon olarak tanımladım. Console bölümünden sonucu görebilirsiniz. Kullanımı için ""dbTemizle(dbAdi)"" olarak çağırmanız lazım.

```javascript
function dbTemizle(databaseName){
    var req = indexedDB.deleteDatabase(databaseName);
    req.onsuccess = function () {
        console.log("DB Silindi");
    };
    req.onerror = function () {
        console.log("DB Silinemedi");
    };
    req.onblocked = function () {
        console.log("DB Kullanımda , Silinemedi");
    };
}
```

Daha detaylı bilgi için [bunu]({{ site.baseurl }}{% post_url 2016-12-05-web-storage-api-indexeddb-kullanimi %}) ve [şunu](https://blogs.shephertz.com/2014/01/14/html5-learn-how-to-use-indexeddb/) okuyabilirsiniz.