---
layout: post
title: "Classic ASP ile Opsiyonel Parametreler Kullanımı"
description: "Classic asp ile fonksiyon ve prosedürlerde opsiyonel parametre kullanımının yolunu keşfediyoruz"
date: 2024-12-28 09:30
categories: ["Yazilim", "Classic Asp"]
tags: ["function", "sub", "procedure", "vbscript", "javascript", "classic-asp"]
pin: false
hidden: false
toc: true
image:
  path: "https://raw.githubusercontent.com/badursun/badursun-assets.github.io/refs/heads/main/img/classic-asp-ile-opsiyonel-parametreler-kullanimi-67701de4aa29d.webp"
  alt: "Classic ASP ile Opsiyonel Parametreler Kullanımı"
---

Classic ASP, günümüzde geliştirilmeyen ancak hala geniş bir kullanıcı kitlesine sahip bir platformdur. Günümüzde bu yazılım dilinin geliştirilmesine Microsoft tarafından son verilmiştir.&#x20;

Bu dilin bazı sınırlamaları olduğu fakat komunite tarafından geliştirilmesi ve/veya katkı sağlanmasına devam ediliyor. Bende hala bu komunitenin bir dinazor parçası olarak hem ASP'yi kullanıyor hemde geliştiriyor ve paylaşıyorum.

Dediğim gibi, bazı sınırlamalar  biri de "isteğe bağlı (opsiyonel) parametreler"in fonksiyon veya prosedürlerde desteklenmemesidir. Modern programlama dillerinde standart olan bu özellik, Classic ASP'de eksiktir. Bu yazıda, bu eksikliğin yol açtığı sorunları, çözüm için kullanılan yaratıcı bir yöntemi ve örnek bir uygulamayı ele alacağız.

## Sorun Temeline İnelim!

Bir prosedür veya fonksiyon, belirli bir işi yerine getirmek için parametrelere ihtiyaç duyar. Ancak Classic ASP'de isteğe bağlı parametreler desteklenmediği için, bir parametre eklemek gerektiğinde:

1. O fonksiyon veya prosedür çağrısını kullandığınız her satırı yeniden güncellemeniz gerekir.
2. Her parametreyi göndermeyi zorunlu hale getirirsiniz.

Bu durum, özellikle büyük ölçekli uygulamalarda zaman alıcı ve hata riski yüksek bir süreç yaratır. Örneğin, şu fonksiyonu ele alalım:

```javascript
<%
Sub LogMessage(Message, LogLevel)
    ' Log seviyesiyle birlikte bir mesaj kaydeder.
    Response.Write Message & " - Level: " & LogLevel
End Sub 

' Başarılı Çalışır
Call LogMessage("Test Mesajı", "WARNING")

' Hata Fırlatır
Call LogMessage("Test Mesajı", "WARNING", "Ek Parametre")
%>
```

Bu prosedürü çağırdığınız her yerde `LogLevel` parametresini belirtmek zorundasınız. Eğer yeni bir "opsiyonel" parametre eklemek isterseniz, mevcut tüm çağrılar güncellenmelidir. Çünkü tanımladığımız prosedür de iki parametre belirlemiştik. Bu prosedürü büyük bir projede belki binlerce defa çağırmış olabilirsiniz. Bu durumda ek parametre için tüm tanımları yeniden düzenlemeniz gerekir.

## Sunucu Tarafında JavaScript ile Opsiyonel Parametreler

Classic ASP'de opsiyonel parametreleri desteklemek için, sunucu tarafında JavaScript kullanılabilir. JavaScript'in varsayılan parametre desteğinden faydalanarak, bu eksikliği giderebiliriz.

### Yöntemin Mantığı

1. **Sunucu Tarafında JavaScript Kullanımı:** `<script language="javascript" runat="server">` etiketiyle sunucu tarafında çalışan JavaScript fonksiyonları tanımlanır.
2. **JavaScript Fonksiyonunda Varsayılan Değerler:** Opsiyonel parametrelerin eksik olup olmadığını kontrol edip varsayılan değer atanır.
3. **VBScript ile Etkileşim:** JavaScript fonksiyonu, VBScript prosedürüne veri gönderir.

### Tanımlama Örneği

Temel mantık, kullanmak istediğimiz fonksiyonu sunucu taraflı bir javascript kodu olarak tanımlıyoruz, kontrol mekanizması burada çalışıyor ve sonuç verisi ASP tarafında çalışan fonksiyona aktarılıyor.

Aşağıda, opsiyonel parametreleri destekleyen bir örnek bulunmaktadır:

```javascript
<%@ Language="VBScript" %>
<% Option Explicit %>

<script language="javascript" runat="server">
function LogMessage(message, logLevel) {
    if (typeof logLevel === "undefined") logLevel = "INFO";
    return VBLogMessage(message, logLevel);
}
</script>

<%
Function VBLogMessage(Message, LogLevel)
    VBLogMessage = "Message: " & Message & ", Level: " & LogLevel
End Function

' Çağırma örnekleri
Response.Write LogMessage("System started") & "<br>"
Response.Write LogMessage("Error occurred", "ERROR") & "<br>"
%>

```

### Kod Açıklaması

1. **JavaScript Fonksiyonu:** `LogMessage`, eksik parametreleri kontrol eder ve varsayılan değerleri atar.
2. **VBScript Fonksiyonu:** `VBLogMessage`, mesaj ve log seviyesi ile birleştirilmiş bir metin döner.

### Avantajları

- **Opsiyonel Parametre Desteği:** Bu yöntemle, Classic ASP'de opsiyonel parametreleri destekleyebilirsiniz. Böylece esnek bir yapıya sahip olursunuz.
- **Esneklik Sağlar:** Prosedür çağrılarında parametreleri atlayarak daha esnek kod yazabilirsiniz.

### Dezavantajları

- **Karmaşıklık:** Sunucu tarafında hem JavaScript hem de VBScript kullanmak kodun karmaşıklığını artırabilir. Örneğin, bu yapı büyük projelerde bakım yapmayı zorlaştırabilir.
- **Performans:** İki dil arasındaki etkileşim performans kaybına neden olabilir. Örneğin, sık çağrılan fonksiyonlarda bu yöntem, sunucuda ek bir yük oluşturabilir.

Classic ASP gibi eski ama hala birçok projede kullanılan bir platformda, isteğe bağlı parametre desteği olmaması ciddi bir eksikliktir. Ancak bu tür yaratıcı çözümler sayesinde, bu eksikliklerin üstesinden gelebilir ve daha esnek, bakımı kolay uygulamalar geliştirebilirsiniz.

Opsiyonel parametre desteğini sağlamak için bu tür çözümler oldukça etkili olabilir. Projenizin mevcut yapısını göz önünde bulundurarak, bu yöntemi parça parça uygulamak ve daha esnek bir sistem oluşturmak mümkün olacaktır.