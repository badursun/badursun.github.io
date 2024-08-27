---
layout: post
title: "Classic ASP - E-Finans ve E-Arşiv Web Servis Entegrasyonu"
date: 2020-08-26 12:48
categories: ["Yazılım", "Classic Asp"]
tags: ["efinans", "efatura", "finans-bank", "asp", "asp-class"]
---

Eğer sizinde aradığınız soru E-Finans E-Arşiv Web Servis Entegrasyonu ve cevabı ise, doğru yerdesiniz.

### E-Finans ve E-Arşiv Web Servis Entegrasyonu
Eğer Classic ASP ile E-Finans / Finans Bank E-Arşiv Web Servis Entegrasyonu yapmak istiyorsanız, bu işlem için oldukça detaylı bir web servisi geliştirmeniz gerekmektedir. Bu ihtiyaca yönelik olarak, E-Finans E-Arşiv fatura web servisi entegrasyonu için hazır bir sınıf yazmış bulunmaktayım.

E-Finans E-Arşiv Web Servis Entegrasyonunu ASP ile entegre ederek, e-ticaret sitenizde satış işlemi gerçekleştirildikten sonra otomatik olarak e-arşiv faturanızı beyan edebilir ve fatura süreçlerinizi tamamen otomatikleştirebilirsiniz. Classic ASP ile geliştirilmiş web sitenize, Finansbank E-Finans E-Arşiv uygulamasını kolaylıkla entegre edebiliriz.

Hazırlamış olduğum bu sınıfın kullanımı oldukça basittir; herhangi bir DLL yüklemenize gerek kalmaz. Tek yapmanız gereken, sınıfın bulunduğu dosyayı mevcut kodlarınıza **include** etmek ve ardından ilgili E-Finans sınıfını çağırarak gerekli alanları doldurmaktır.

## Örnek E-Arşiv Faturası Oluşturma Kodu
```javascript
<!--#include file="EFinans.WebServis.asp"-->
<%
	Set EFinans = New EFinansClass
		' Kurumsal Bilgiler
		'---------------------------------
		EFinans.IslemID 		= CreateUUID()
		EFinans.json 			= Array("donenBelgeFormati", "2") ' 0 (UBL) 2 (HTML) 3 (PDF) 9 (YOK)
		EFinans.json 			= Array("goruntuOlusturulsunMu", "1") ' Add XSLT by Server
		EFinans.json 			= Array("islemId", EFinans.IslemID() ) ' Benzersiz Fatura UUID
		EFinans.json 			= Array("vkn", "1122334455") ' Vergi Kimlik Numaranız
		EFinans.json 			= Array("sube", "DFLT") ' Varsa Şube Kodu
		EFinans.json 			= Array("kasa", "DFLT") ' Varsa Kasa Kodu
		EFinans.json 			= Array("numaraVerilsinMi", "1") '  0 (HAYIR) 1 (EVET)
		EFinans.json 			= Array("faturaSeri", "AB") ' Fatura Seri 
		EFinans.json 			= Array("erpKodu", "XYZ1234") ' Varsa ERP Kodu
		' Input Data JSON
		'---------------------------------
		EFinans.faturaInput 	= EFinans.json()

		' Invoice Type
		'---------------------------------
		EFinans.faturaFormat 	= "UBL" ' "ÖZEL XML" "PDF_CUSTOM" "PDF_UBL"

		' Fatura İçerik Bilgileri
		'---------------------------------
		EFinans.FaturaAliciAdi 			= "Anthony Burak"
		EFinans.FaturaAliciSoyAdi 		= "DURSUN"
		EFinans.Fatura_Alici_Ulke 		= "Türkiye"
		EFinans.FaturaAliciVKN 			= "11111111111"
		EFinans.FaturaTarihi 			= Now()
		EFinans.FaturaAliciMail 		        = "badursun@gmail.com"
		EFinans.Fatura_Urun 			= "Casio Kol Saati ABC123" ' Ürün Adı
		EFinans.Fatura_Urun_Adet		= "1" ' Adet
		EFinans.Fatura_Urun_Birim_Fiyat   = "299.90"
		EFinans.Fatura_ParaBirimi 		= "TRY" ' Para Birimi (TRY, EUR, USD)
		EFinans.Fatura_KDV_Orani 		= "18" ' KDV Oranı %
		EFinans.faturaIcerik 	                 = EFinans.xml_faturaOlustur()

		' SOAP Paketi Oluşturuluyor
		'---------------------------------
		str_fatura_soap = EFinans.faturaOlustur()

		' Paketin Gönderileri (Test/Üretim Ortamı)
		'---------------------------------
		Result = EFinans.SendSoap(str_fatura_soap, "https://earsivtest.efinans.com.tr/earsiv/ws/EarsivWebService?wsdl", "faturaOlustur")
		' Result = EFinans.SendSoap(str_fatura_soap, "https://earsiv.efinans.com.tr/earsiv/ws/EarsivWebService", "faturaOlustur")

Response.Write Result ' Servisden Dönen Sonuç (XML)
%>
```

Oldukça pratik ve esnek bir yapıya sahip. Versiyon geliştirmesine devam etmekteyim, E-Finans E-Arşiv entegrasyonu ihtiyacınız için badursun@gmail.com adresinden bana ulaşabilirsiniz. İster kendiniz entegre edebilir, isterseniz de entegrasyonu sizin için ben yapabilirim.