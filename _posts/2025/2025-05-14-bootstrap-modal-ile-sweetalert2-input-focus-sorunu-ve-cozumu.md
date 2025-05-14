---
layout: post
title: "Bootstrap Modal ile SweetAlert2 Input Focus Sorunu ve Çözümü"
description: "Bootstrap Modal açıkken SweetAlert2 input'larına yazılamaması sorununu ve bunun teknik nedenleriyle çözüm yolunu detaylıca inceliyoruz."
date: 2025-05-14 10:04
categories: ["Bootstrap","SweetAlert2"]
tags: ["bootstrap", "sweetalert2", "modal", "input", "focus", "focus-trap", "focus-trap-sweet-alert2"]
pin: false
hidden: false
toc: true
image:
 path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/bootstrap-modal-ile-sweetalert2-input-focus-sorunu-ve-cozumu.webp"
 alt: "Bootstrap Modal ile SweetAlert2 Input Focus Sorunu ve Cozumu"
---

Bootstrap modal kullanırken, aynı anda SweetAlert2 ile bir input penceresi açmak istiyorsanız dikkat! Özellikle modal hala açıkken bir `<input>` alanı içeren SweetAlert penceresi açtığınızda, input alanına yazı yazılamadığını fark etmiş olabilirsiniz.

Bu yazıda bu sorun neden olur, nasıl anlaşılır ve nasıl çözülür, teknik detaylarıyla anlatacağım.

## 🎯 Problem Tanımı

Aşağıdaki gibi bir senaryoda:

```js
// Bootstrap Modal açıkken çalışıyor
Swal.fire({
  title: 'Bir şey yaz',
  input: 'text',
  ...
});
````

**Input’a yazı yazamazsınız.** Klavyeden hiçbir karakter girilemez. Çünkü Bootstrap modal’ın **"focus trap" (odak tuzağı)** devrede kalmaya devam eder ve dışarıdaki hiçbir odaklanma olayına izin vermez.

## Teknik Sebep

Bootstrap Modal (v5+), açıldığında tüm sayfa odaklarını içerideki modal’a kilitleyen bir sistem kullanır. Bu sistem `FocusTrap` denen bir yapı ile modal dışına yapılan `focusin` olaylarını engeller.

SweetAlert2, kendi penceresini body’nin sonuna (`<body>` tag’ı dışına) ekler. Ancak Bootstrap modal hala açık olduğundan, bu alert’in içindeki `<input>` gibi alanlara yapılan odaklama Bootstrap tarafından engellenir.

Bu çakışma, özellikle SweetAlert2'nin input'lu prompt'larında belirgin hale gelir.

## Çözüm

SweetAlert2 açıldığında, onun `.swal2-container` elemanını, **Bootstrap modal'ın içine** alırsak, focus çakışması ortadan kalkar. Böylece SweetAlert bileşeni de modal içinde "güvenli" bir alan olarak kabul edilir.

Aşağıdaki çözüm bunu yapar:

```js
// Bootstrap 5.1.x environment.
// Sweat Alert 2@11
Swal.fire({
  title: 'İsminizi girin',
  input: 'text',
  didOpen: () => {
    // MAGIC - Move SweetAlert2 window to Bootstrap modal body
    const swalContainer = document.querySelector('.swal2-container');
    const modalBody = document.querySelector('.modal-body');
    if (swalContainer && modalBody) {
      modalBody.appendChild(swalContainer);
    }
  }
});
```

Bu `didOpen` fonksiyonu sayesinde:

* SweetAlert2’nin root elementi `.swal2-container`, Bootstrap modal’ın içindeki `.modal-body` içerisine taşınır.
* Bootstrap artık bu alert’i kendi modal alanı olarak görür.
* Böylece input alanı odaklanabilir ve kullanıcı rahatça veri girişi yapabilir.

---

## Bonus: Neden `focus: false` veya `focustrap.deactivate()` işe yaramaz?

`new bootstrap.Modal(..., { focus: false })` veya `modal._focustrap.deactivate()` kullanmak teorik olarak işe yarayabilir. Ancak bu ayarlar:

* Modal daha önce JS ile başlatılmadıysa uygulanamaz.
* Framework üzerinden init edilmişse `getInstance` çalışmaz.
* Özellikle Vue/React/CoreUI gibi yapılar bu `bootstrap.Modal` örneğine ulaşmayı engelleyebilir.

Bu nedenle, SweetAlert’in kendisini modal içine taşımak, **her senaryoda çalışan, evrensel bir çözümdür.**

## Bulunmuş Farklı Çözümler

İnternette araştırdığımızda farklı çözüm önerileri de geliyor. Tabii bunlar da işe yarıyor olabilir, sonuçta herkes aynı versiyonları kullanmıyor. Versiyon farklılıklarıda bize farklı çözümler bulmamızı gerektiriyor. İşte derlediğim bazı çözüm olmuş yöntemler:

```js
    // Bootstrap 5.1.x environment.
    $(document).off('focusin');  //NG
    $(document).off('focusin.bs.focustrap'); //NG
```

```js
    // Bootstrap 5.1.x environment.
    $('#myModal').on('shown.bs.modal', function() {
        $(document).off('focusin.modal');
    });
```

```html
<!-- Add data-bs-focus="false" to the modal html. -->
<div class="modal fade" id="dialog_box" data-bs-focus="false" aria-hidden="true" tabindex="-1">
...
</div>
```

Bootstrap Modal açıkken SweetAlert2 ile input kullanmak istiyorsanız:

* Ya modal focus tuzağını manuel devre dışı bırakın (zor).
* Ya da daha temiz bir şekilde SweetAlert2'yi modal içine taşıyın (kolay ve sağlam).

SweetAlert2’nin `.swal2-container` elemanını `.modal-body` içine eklemek, tüm focus sorunlarını çözen pratik bir yöntemdir.