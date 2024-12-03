---
layout: post
title: "Input Placeholder Rengi Değiştirme (Pure CSS)"
date: 2018-10-23 12:26
categories: ["Yazilim", "Css"]
tags: ["css", "input", "placeholder"]
toc: true
---

Input elementleri için çoğu zaman placeholder kullanırız. Bu komut, içi boş olan bir input için nitelik tanımlayıcı olabilir. Örneğin adınızı yazın gibi bir mesaj verebilirsiniz. Fakat genelde rengi çok açık gri olduğu için ya okunmaz yada sizin tasarımınızda konseptinize uymaz. İşte aşağıda ki kod ile input elementleriniz içinde ki placeholder bölümünün rengini değiştirebilirsiniz.

## CSS Kodu
```css
::-webkit-input-placeholder { /* Chrome/Opera/Safari */
  color: red;
}
input::-moz-placeholder { /* Firefox 19+ */
  color: red;
}
input:-ms-input-placeholder { /* IE 10+ */
  color: red;
}
input:-moz-placeholder { /* Firefox 18- */
  color: red;
}
input::-ms-input-placeholder { /* IE Edge */
  color: red;
}
```

## Kullanılabilir Class Değerleri
Bu element için yazabileceğiniz class'lar ise şöyledir;

- **font:**
- **color:**
- **background:**
- **word-spacing:**
- **letter-spacing:**
- **text-decoration:**
- **vertical-align:**
- **text-transform:**
- **line-height:**
- **text-indent:**
- **opacity:**

## Tarayıcı Desteği
<table class="browser-support-table">
<thead>
<tr>
<th class="chrome">Chrome</th>
<th class="safari">Safari</th>
<th class="firefox">Firefox</th>
<th class="opera">Opera</th>
<th class="ie">IE</th>
<th class="android">Android</th>
<th class="iOS">iOS</th>
</tr>
</thead>
<tbody>
<tr>
<td class="yep" data-browser-name="Chrome">4+</td>
<td class="yep" data-browser-name="Safari">5+</td>
<td class="yep" data-browser-name="Firefox">4+</td>
<td class="yep" data-browser-name="Opera">15+</td>
<td class="yep" data-browser-name="IE">10+</td>
<td class="yep" data-browser-name="Android">Any</td>
<td class="yep" data-browser-name="iOS">Any</td>
</tr>
</tbody>
</table>

Firefox tarayıcısı 18 ve 19+ olarak desteklemektedir. Bazı tarayıcıların major - minor sürümlerinde bu destek bulunmayabilir.

Kolay gelsin.