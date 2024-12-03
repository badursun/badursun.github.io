---
layout: post
title: "NodeJS ile Profesyonel File Structure Kurgulamak"
date: 2024-09-28 09:14
categories: ["Yazilim", "NodeJS"]
tags: ["yazilim", "javascript", "nodejs", "file-structure"]
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/nodejs-folder-structure-66f7a4c5e1d86.webp"
  alt: "NodeJS ile Profesyonel File Structure Kurgulamak"
---

NodeJS projelerinde dosya yapısını profesyonel bir şekilde organize etmek, hem geliştirme sürecini daha düzenli hale getirir hem de ekip içinde ortak çalışmayı kolaylaştırır. Bu yazıda, NodeJS ile Express.js framework'ü kullanarak profesyonel bir dosya yapısının nasıl kurgulanacağını ve her klasörün işlevini detaylı bir şekilde anlatacağım. 

Ayrıca, verdiğim dosya yapısının neden bu şekilde kurulduğuna ve nasıl faydalar sağladığına değineceğim.

## Dosya Yapısının Önemi

Bir projede dosya yapısını doğru bir şekilde organize etmek, özellikle projenin büyümesiyle birlikte büyük önem kazanır. **Doğru bir dosya yapısı**, şunları sağlar:
- **Bakımı kolaylaştırır**: Dosya yapısının düzenli olması, kodu incelemeyi ve güncellemeyi kolaylaştırır.
- **Modülerlik**: Kodu işlevsel parçalara ayırarak her bileşeni bağımsız hale getirir. Bu sayede bir bileşeni değiştirmek veya geliştirmek diğer bileşenlere minimum etki yapar.
- **Yeniden kullanılabilirlik**: Ortak fonksiyonlar veya modüller farklı bölümlerde kolayca yeniden kullanılabilir.
- **Ekip Çalışması**: Farklı geliştiriciler üzerinde çalışırken, herkesin kodun nerede olduğunu ve hangi modülün hangi işlevi yerine getirdiğini kolayca anlamasını sağlar.

### Örnek File Structure

Aşağıda örnek bir Express.js projesi dosya yapısı bulunmakta. Bu yapıyı detaylandırarak, her klasörün amacını ve içerdiği dosyaların işlevlerini açıklayacağız:

```bash
my-express-app/ 
│ 
├── public/             # Statik dosyalar (CSS, JS, img vb.) 
│ ├── css/ 
│ ├── js/ 
│ └── images/ 
│ 
├── src/                # Projenin asıl kaynak kodları 
│ ├── config/           # Yapılandırma dosyaları (veritabanı bağlantıları vb.) 
│ ├── controllers/      # Route'larla çalışacak controller dosyaları 
│ ├── middleware/       # Orta katman (middleware) dosyaları 
│ ├── models/           # Veritabanı modelleri (ORM kullanıyorsan) 
│ ├── routes/           # Express yönlendirme dosyaları 
│ ├── services/         # İş mantığı ve servis katmanı (veritabanı işlemleri, API çağrıları) 
│ ├── utils/            # Yardımcı fonksiyonlar, araçlar 
│ ├── validators/       # Veri doğrulama kuralları (Joi, express-validator vb.) 
│ └── views/            # Temel sayfalar ve şablon dosyaları (EJS, Pug vb.) 
│ ├── .env              # Çevre değişkenleri (gizli bilgiler) 
├── .gitignore          # Git'e dahil edilmeyecek dosyalar 
├── package.json        # Proje bağımlılıkları 
├── package-lock.json   # Bağımlılıkların kesin versiyonları 
├── app.js              # Uygulamanın başlangıç dosyası 
└── README.md           # Proje hakkında genel bilgiler
```

### Detaylar

#### 1. `public/`
Bu klasör, kullanıcıya servis edilen **statik dosyaları** barındırır. Bu statik dosyalar, önceden işlenmiş ve sunucu tarafından değiştirilmeden gönderilen dosyalardır (CSS, JavaScript dosyaları, görseller vb.). Bu sayede statik içeriklerin yönetimi ayrı bir yerde toplanır ve tarayıcı önbelleklemesi gibi optimizasyonlar kolaylaşır.

#### 2. `src/`
Bu klasör, projenin tüm **kaynak kodlarını** barındırır. Uygulama ile ilgili tüm kodlar burada bulunduğundan, projenin ana dizininde karmaşıklık önlenmiş olur.

- **`config/`**: **Yapılandırma dosyaları** burada yer alır. Örneğin veritabanı bağlantıları, port bilgileri gibi projenin çalışması için gerekli ayarlar burada tanımlanır. Bu, yapılandırmaların merkezi bir yerden yönetilmesini sağlar.
  
- **`controllers/`**: **Controller dosyaları**, belirli bir iş mantığını yerine getiren kod parçalarını barındırır. Bu katman, **gelen istekleri işler** ve uygun tepkiyi döner. Bu sayede iş mantığı (business logic) ve yönlendirme (routing) ayrılmış olur, projenin modülerliği artar.
  
- **`middleware/`**: Express.js'de **middleware** katmanı, istekleri yönlendirme, yetkilendirme, doğrulama gibi **ön işleme** veya **son işleme** aşamaları için kullanılır. Bu dosyalar, uygulamanın farklı noktalarında ortak görevleri yerine getirmek için kullanılır ve kodun tekrarını önler.

- **`models/`**: **Veritabanı modelleri**, uygulamanın kullandığı veri yapılarını temsil eder. Eğer bir **ORM** kullanıyorsanız (örneğin Sequelize veya Mongoose), bu modeller veritabanı ile etkileşim kurmak için kullanılır. Bu katman, veritabanı sorgularını soyutlayarak daha okunabilir ve modüler bir yapı sağlar.

- **`routes/`**: **Express yönlendirme dosyaları**, gelen isteklerin hangi controller tarafından işleneceğini belirler. Bu dosyalar, uygulamanın URL yapılarını yönetir ve belirli bir URL'nin hangi işleme gitmesi gerektiğini belirtir. Yönlendirme dosyalarını ayrı bir klasörde tutarak, uygulamanın genel mantığını kolayca anlamak mümkün olur.

- **`services/`**: **Servis katmanı**, genellikle **iş mantığı** ve **veritabanı işlemleri** ile ilgilidir. Bu katman, controller'lardan gelen verileri işler ve veritabanı ile etkileşim kurar. Bu yapı, controller katmanını gereksiz iş mantığından arındırarak daha sade hale getirir.

- **`utils/`**: **Yardımcı fonksiyonlar** ve araçlar burada yer alır. Bu fonksiyonlar, projenin birçok yerinde tekrar tekrar kullanılabilecek küçük görevleri yerine getirir (tarih formatlama, dosya işlemleri vb.). Bu, kodun yeniden kullanılabilirliğini artırır ve her yerde aynı fonksiyonun tekrar yazılmasını engeller.

- **`validators/`**: **Veri doğrulama kuralları**, kullanıcıdan gelen verilerin belirli kurallara uygun olup olmadığını kontrol eder. Bu klasörde, kullanıcının girdiği bilgilerin doğru formatta olup olmadığını kontrol eden validator dosyaları bulunur. Bu katman, veri güvenliği ve veri kalitesini sağlamak için önemlidir.

- **`views/`**: Eğer uygulamanızda **sunucu tarafında şablon kullanıyorsanız** (örneğin EJS, Pug), bu dosyalar burada tutulur. Görüntü dosyalarının ayrı bir klasörde tutulması, MVC mimarisine uygun bir yapı sunar ve kodun okunabilirliğini artırır.

#### 3. `.env`
Uygulamanın kullandığı **gizli bilgiler** (API anahtarları, veritabanı bilgileri) burada tanımlanır. Bu bilgiler, güvenlik açısından `.gitignore` dosyasına eklenerek Git ile paylaşılmaz.

#### 4. `.gitignore`
Git versiyon kontrol sistemine **dahil edilmemesi gereken dosyalar** bu dosyada belirtilir. Bu sayede `.env` gibi gizli dosyalar ya da `node_modules` gibi büyük klasörler Git deposuna dahil edilmez.

#### 5. `package.json` ve `package-lock.json`
Bu dosyalar, projenin **bağımlılıklarını** ve bu bağımlılıkların **kesin versiyonlarını** belirtir. Bu, aynı ortamda aynı yazılımın kurulmasını garanti altına alır ve proje bağımlılıklarının yönetimini kolaylaştırır.

#### 6. `app.js`
Bu dosya, **uygulamanın başlangıç noktasıdır**. Uygulamanın başlatılması, gerekli middleware'lerin eklenmesi ve sunucunun hangi portta dinleneceği gibi ayarlar burada yapılır. Bu dosya genellikle en üst düzeyde uygulamanın genel işleyişini kurar.

#### 7. `README.md`
Proje hakkında genel bilgileri içerir. Bu, projenin nasıl kurulacağını ve kullanılacağını açıklayan bir dokümandır ve projeyi yeni bir geliştiriciye tanıtmak için kullanılır.

### Eksik Olan Klasör veya Dosyalar
Önerilen dosya yapısına ek olarak aşağıdaki yapılar da eklenebilir:

- **`logs/`**: Uygulama ile ilgili **log dosyalarını** burada saklayabilirsiniz. Bu sayede hata ayıklama veya performans analizi daha kolay olur.
- **`tests/`**: **Test dosyaları**, uygulamanın işlevlerini test etmek için burada tutulur. Bu sayede projenin doğru çalıştığını ve yeni değişikliklerin önceki işlevselliği bozmadığını kontrol edebilirsiniz.
  
Profesyonel bir dosya yapısı, özellikle büyük ve kompleks projelerde, kodun okunabilirliğini, sürdürülebilirliğini ve modülerliğini büyük ölçüde artırır. Her klasör, belirli bir amaca hizmet eder ve kodun hangi amaca yönelik yazıldığını anlamak kolaylaşır. Bu yazıda verdiğim dosya yapısı, hem ekip çalışmalarında hem de bireysel projelerde kodun daha rahat anlaşılabilir ve yönetilebilir olmasını sağlayacaktır.

> Orderly Life, Orderly Living, Orderly Code