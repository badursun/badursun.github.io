---
layout: post
title: "Classic ASP'de Include İşlemini Dinamik Olarak Gerçekleştirmek: FileSystemObject ve ExecuteGlobal Kullanımı"
date: 2024-06-04 11:29
categories: ["Yazılım", "Classic Asp"]
tags: ["dynamic-include", "classic-asp", "asp", "executeglobal"]
toc: true
image:
  path: "/assets/img/asp.jpg"
  alt: "Javascript"
---

# Classic ASP'de Include İşlemini Dinamik Olarak Gerçekleştirmek: FileSystemObject ve ExecuteGlobal Kullanımı

Classic ASP'de '#include' direktifi, diğer ASP dosyalarını mevcut dosyaya dahil etmek için yaygın olarak kullanılır. Ancak, bazen include işlemini dinamik olarak yapmak gerekebilir. Bu yazıda, bir ASP dosyasını 'FileSystemObject (FSO)' ile okuyup, 'ExecuteGlobal' ile çalıştırarak dinamik bir include işlemini nasıl gerçekleştirebileceğinizi göstereceğim.

## Dinamik Include İşlemi Neden Gerekli?
Dinamik include işlemi, bazı senaryolarda gerekli olabilir:
- Kodun hangi dosyayı dahil edeceğinin koşullara göre belirlenmesi.
- Uygulamanın modüler hale getirilmesi.
- Daha esnek ve yönetilebilir bir kod yapısı oluşturma ihtiyacı.

## FileSystemObject ve ExecuteGlobal Kullanımı
ASP'de dosya sistemine erişmek ve bir dosyanın içeriğini okumak için 'FileSystemObject (FSO)' kullanılır. Okunan bu içeriği, ASP'nin çalıştırabileceği hale getirmek için ise 'ExecuteGlobal' fonksiyonu devreye girer. Şimdi, bu işlemi nasıl yapabileceğinizi adım adım inceleyelim.

### Adım 1: FileSystemObject ile Dosyayı Okumak
İlk olarak, bir dosyanın içeriğini okumak için FSO'yu kullanmamız gerekiyor. Aşağıdaki fonksiyon, belirtilen dosyanın içeriğini okuyup bir string olarak geri döner:

```javascript
<%
Function ReadFileContent(ByVal filePath)
    Dim fso, file, content
    Set fso = Server.CreateObject("Scripting.FileSystemObject")
    On Error Resume Next
    
    If fso.FileExists(filePath) Then
        Set file = fso.OpenTextFile(filePath, 1) '1: ForReading
        content = file.ReadAll
        file.Close
        Set file = Nothing
    Else
        content = "" ' Dosya bulunamazsa boş döner
    End If
    
    Set fso = Nothing
    ReadFileContent = content
End Function
%>
```

### Adım 2: ExecuteGlobal ile İçeriği Çalıştırmak
Dosya içeriği okunduktan sonra, bu içeriği mevcut scope'ta çalıştırmamız gerekiyor. Bunun için ExecuteGlobal fonksiyonunu kullanacağız. Aşağıdaki fonksiyon, dinamik olarak bir dosyayı dahil eder ve içerikteki kodu çalıştırır:

```javascript
<%
Function IncludeDynamic(filePath)
    Dim fileContent
    fileContent = ReadFileContent(filePath)
    
    If fileContent <> "" Then
        ExecuteGlobal fileContent
    End If
End Function
%>
```

### Adım 3: Fonksiyonu Kullanmak
Bu fonksiyonu kullanarak dinamik olarak istediğiniz dosyayı ASP sayfanıza dahil edebilirsiniz:

```javascript
<%
Call IncludeDynamic(Server.MapPath("path/to/yourfile.asp"))
%>
```
Bu şekilde, koşullara bağlı olarak dosyaları dinamik bir şekilde sayfanıza dahil edebilirsiniz.

## Performans ve Güvenlik İpuçları
- **Performans:** Bu yöntem, çok sayıda dosya okuma ve yürütme işlemi içerdiğinden, performans sorunlarına yol açabilir. Mümkünse bu tür işlemleri minimize etmek veya cache kullanarak optimize etmek iyi bir fikir olabilir.
- **Güvenlik:** Dinamik dosya dahil etme işlemi güvenlik riskleri oluşturabilir. Dahil edilecek dosyanın yolu kullanıcıdan gelen bir girişle belirleniyorsa, bu girişi mutlaka doğrulayın ve sanitize edin.