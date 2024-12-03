---
layout: post
title: "İTS İlaç Takip Sistemi ve Classic ASP SOAP İletişim Örneği"
date: 2018-04-13 13:22
categories: ["Yazilim", "Classic Asp"]
tags: ["soap", "classic-asp", "asp", "its", "ilac-takip-sistemi", "fonksiyonlar"]
toc: true
---

İTS İlaç Takip Sistemi ve Classic ASP SOAP İletişim için bir takipçimin yardıma ihtiyacı oldu. Bende ona bu konuda yardımcı olup, hazırladığım örneği paylaşabilmek adına kendisinden izin aldım. E-Sağlık hizmetlerinde İTS (İlaç Takip Sistemi) entegrasyonunu Classic ASP ile yapmak istiyorsanız ve SOAP iletişim katmanına pek hakim değilseniz biraz zorlayıcı olabiliyor. Çünkü her iletişim türünün kendine has bazı ince ayrıntıları vardır. Tabii burada kodlama yapısıda çok önemlidir. OOP (Obejct Oriented Programming) gibi nesne tabanlı bir kodlama yada kullanılan bileşenin verdiği yapıya göre yazılımınızı geliştirmeniz gerekiyor.

## İTS İlaç Takip Sistemi Nedir?
İlaç Takip Sistemi, tüm dünyada uygulanan Takip ve İzleme sisteminin ilaç sektörüne uyarlanmış şeklidir. Bu sistemle birlikte ürünlerin tedarik ve dağıtım süreçlerinde bulunduğu konumu belirlemek mümkün olmaktadır. Elektronik ürün kodu teknolojisi sayesinde, ürünlerin yani ilaçların, üretim veya ithalatından itibaren tedarik zincirinde gerçekleştirdiği her hareketi izlemek mümkündür. Buna göre her bir ilaç kutusunun üzerine basılan karekodlar sayesinde ürünün giriş ve çıkışı raporlanarak, ürünün son görüldüğü konum, zaman ve durum kaydedilir ve gerçek zamanlı bir veri tabanında saklanır.

## İTS İlaç Takip Sistemi Web Servis Klavuzu
Beşeri Tıbbi Ürünler Ambalaj ve Etiketleme yönetmeliğinde yapılan değişiklikle, yönetmeliğe tabi ürünlerin tekil olarak tanımlanması ve izlenebilmesi için ürünler üzerinde karekod bulunması zorunlu hale gelmiş, ürünlere ait bilgilerin İlaç ve Eczacılık Genel Müdürlüğü’ne gönderilmesi şeklinde, ürünlerin izlenmesi için bir altyapı sağlanmıştır. Bu altyapı “İlaç Takip Sistemi” (kısaca İTS) adıyla anılmaktadır. Bu sistemin iletişim katmanı olan SOAP yapısını daha iyi anlamak için internette bir çok döküman bulunmakta. Tabii sistemin kendi klavuzları biraz yetersiz kalıyor. Ne derler; "Döküman iyi hazırlanmışsa çok iyi, döküman kötü hazırlanmışsa idare eder". Hiç yoktan iyidir yani :) Ama aşağıda vereceğim klavuz oldukça başarılı aslında.

İTS Web servislerinin işletimine ilişkin sorularınızı its@iegm.gov.tr adresine gönderebilirsiniz.

## En Basit Hali İle Entegrasyon Örneği
Gelelim örnek kodumuza. Ben kodu sadece Ürün Doğrulama için hazırladım. Burada oluşturduğumuz SOAP zarfı içerisinde bir takım bilgiler girmeniz gerekiyor ve birden çok ürün sorgulaması da yapabiliyorsunuz. Önce örnek kodu paylaşacağım sonra altında kullanımı hakkında izah vereceğim.

```javascript
<%
Cmd = Request.Querystring("Cmd")

' ANAHTARLAR
paydas_gln 	= "1122334455666" 		' Paydaşın GLN Numarası
auth_user 	= paydas_gln & "0000" 	' Auth için User (GLN Numarasına 4 sıfır eklenince...)
auth_pass 	= "121212" 				' Auth İçin Pass


Function UrunDogrulama(ByVal GelenData)
	r="<?xml version='1.0' encoding='utf-8'?>"
	r=r&"<soapenv:Envelope xmlns:soapenv='http://schemas.xmlsoap.org/soap/envelope/' xmlns:gen='http://its.iegm.gov.tr/bildirim/BR/v1/UrunDogrulama/Genel'>"
	r=r&"	<soapenv:Header/>"
	r=r&"	<soapenv:Body>"
	r=r&"		<gen:UrunDogrulamaBildirim>"
	r=r&"			<DT>V</DT>" ' Validate için V olmalı.
	r=r&"			<FR>"& paydas_gln &"</FR>"
	r=r&"			<URUNLER>"
	' Gelen veri içinde # varsa çoklu üründür, split et, döngü oluştur.
		If Instr(1, GelenData, "#") > 0 Then
			GelenUrunler = Split(GelenData, "#")
			For i = 0 To UBound(GelenUrunler)
				Bilgiler = Split( GelenUrunler(i), "|" )
				r=r&"				<URUN>"
				r=r&"					<GTIN>"& Bilgiler(0) &"</GTIN>"
				r=r&"					<BN>"& Bilgiler(1) &"</BN>"
				r=r&"					<SN>"& Bilgiler(2) &"</SN>"
				r=r&"					<XD>"& Bilgiler(3) &"</XD>"
				r=r&"				</URUN>"
			Next
		Else
		' Aksi halde tek üründür, | split et, data oluştur.
			r=r&"				<URUN>"
			r=r&"					<GTIN>"& GTIN &"</GTIN>"
			r=r&"					<BN>"& BN &"</BN>"
			r=r&"					<SN>"& SN &"</SN>"
			r=r&"					<XD>"& XD &"</XD>"
			r=r&"				</URUN>"
		End If
	r=r&"            </URUNLER>"
	r=r&"        </gen:UrunDogrulamaBildirim>"
	r=r&"    </soapenv:Body>"
	r=r&"</soapenv:Envelope>"

	SOAP_Sorgu_wsdl = "http://its.saglik.gov.tr/UrunDogrulama/UrunDogrulamaReceiverService"
	Set objHTTP = Server.CreateObject("MSXML2.ServerXMLHTTP.6.0") 
	With objHTTP
		.Open "POST", SOAP_Sorgu_wsdl, False, auth_user, auth_pass
		.setRequestHeader "Content-Type", "text/xml; charset=utf-8"
		.setTimeouts 5000, 5000, 10000, 10000 'ms - resolve, connect, send, receive
		.send r
	End With

	UrunDogrulama = objHTTP.responseText 
End Function 

If Cmd = "Test" Then
	response.ContentType="text/xml"

	' Çoklu Ürün Sorgulama
	' Son ürün datasının sonunda # diyez olmayacak !
	Response.Write UrunDogrulama("00681044040022|12312|00000000000003535190|190531#" & "00681044040022|12312|00000000000003535191|190531#" & "00681044040022|12312|00000000000003535192|190531#" & "00681044040022|12312|00000000000003535193|190531")

	' Teklif Ürün Sorgulama
	' Response.Write UrunDogrulama("08681094040022|16146|00000000000003535121|190531")
End If
%>
```

Mümkün olduğunca kodlama bölümünde nasıl çalıştığını, hangi parametreleri girmeniz gerektiğini anlattım. Fakat özetle yukarıda hazırladığım yapıyı kullanırsanız 2 method ile cevap alabilirsiniz. Bu arada alacağınız cevap XML yapıda olacaktır. Buna göre node parçalayıp, ihtiyaç duyduğunuz verileri alabilirsiniz. Ayrıca cevap bir değişkene atamak isterseniz Response.Write yerine Degiskenim = UrunDogrulama("****") olarak yapılandırabilirsiniz.

## Tekli Ürün Doğrulama
```javascript
Response.Write UrunDogrulama("08681094040022|16146|00000000000003535121|190531")
```

Bu kod yapısında dönen cevabı direkt ekrana döküyoruz. UrunDogrulama() fonksiyonun alt parametreleri tek öbekten oluşup özel delimiter (işaretleyici) ile girilmektedir.

UrunDogrulama() sırasıyla GTIN Numarası, BN Numarası, SN Numarası, XD Numarası parametrelerini girmeniz gerekiyor. Bu numaralar zaten sistemin içindeyseniz aşina olduğunuz bilgilerdir.

## Çoklu Ürün Doğrulama
```javascript
Response.Write UrunDogrulama("00681044040022|12312|00000000000003535190|190531#" & "00681044040022|12312|00000000000003535191|190531#" & "00681044040022|12312|00000000000003535192|190531#" & "00681044040022|12312|00000000000003535193|190531")
```

Bu kod yapısında dönen cevabı direkt ekrana döküyoruz. UrunDogrulama() fonksiyonun alt parametreleri tek öbekten oluşup özel delimiter (işaretleyici) ile girilmektedir.

UrunDogrulama() sırasıyla GTIN Numarası, BN Numarası, SN Numarası, XD Numarası parametrelerini girmeniz gerekiyor. Tekli ürün sorgulamadan farkı, ikinci bir delimiter kullanıyor olmamız. Her bir ürün için 4 parametre girmemiz gerekiyor (Sırasıyla GTIN|BN|SN|XD) ve her ürünün parametreleri için delimiter olarak | işaretini kullandık. Birden çok ürünü ise ilk pattern'de olduğu gibi ard arda eklemeye devam ediyoruz fakat iki ürün bilgisi arasında ki sıraya sadık kalarak bu sefer diyez (kare) # yada nam-ı diğer hashtag kullanıyoruz. Örnekleyecek olursak 3 ürünü sorgulamak için GTIN|BN|SN|XD**#**GTIN|BN|SN|XD**#**GTIN|BN|SN|XD yapısını kullanmamız gerekiyor. Yani daha özetle A#B#C bize 2 split ve toplamda 3 veri veriyor.

## Sistem Erişim Protokolü
İTS Sistemini kullanabilmek için paydaş erişim bilgilerini almanız gerekiyor. Bunlardan bir tanesi paydaş kodu, bir tanesi Basic Auth doğrulaması (güvenlik için) verilen kullanıcı kodu ve şifresidir. Örnek kodun en üstünde göreceğiniz bu üç bilgiyi mutlaka doğru olarak girdiğinize emin olun.

```javascript
<%
'###############################
' ANAHTARLAR
'###############################
' Paydaşın GLN Numarası
paydas_gln 	= "1122334455666"
' Auth için User (GLN Numarasına 4 sıfır eklenince...)
auth_user 	= paydas_gln & "0000"
' Auth İçin Pass
auth_pass 	= "121212"
%>
```

Bu bilgilerden ilki paydaşın GLN numarasıdır. Bunu portalınızdan alabilirsiniz. İkinci bilgileri ise güvenlik katmanını geçmeniz için verilen kullanıcı kodu ve parolasıdır. Yalnız şu noktayı belirtelim, Auth için kullanıcı kodu, Paydaş GLN numaranızın sonunda 4 sıfır eklenmiş halidir. Bu yüzden kodlamada siz sadece 2 adet değişkeni doldurmanız yeterlidir.

Auth işlemi her sorguda tekrarlanıyor. Bunu **MSXML2.ServerXMLHTTP.6.0** katmanında gerçekleştiriyoruz.

```javascript
<%
Set objHTTP = Server.CreateObject("MSXML2.ServerXMLHTTP.6.0") 
With objHTTP
	.Open "POST", SOAP_Sorgu_wsdl, False, auth_user, auth_pass
	.setRequestHeader "Content-Type", "text/xml; charset=utf-8"
	.setTimeouts 5000, 5000, 10000, 10000 'ms - resolve, connect, send, receive
	.send r
End With
%>
```

**MSXML2.ServerXMLHTTP.6.0** protokolünün OPEN işlevinde tanımlayıcılar sırasıyla

**Open =** Protokolü Aç
**URL=** Protokolün iletişim adresi (Genelde SOAP wsdl adresidir)
**Asenkron Durumu=** False (bizim için yeterli çünkü zamanlamayı bilmiyoruz)
**Auth User Name** (Opsiyonel)
**Auth Password** (Opsiyonel)

şeklinde tanımlıyoruz. Genelde internette son 2 parametre olmadan kullanıldığını görürsün, eğer basic auth yöntemi kullanılmıyorsa burayı boş geçmenizde sorun yoktur. Fakat iletişim katmanı bir auth ile korunuyorsa, bu korumayı geçmek için doğru bilgileri bu alanda iletmeniz gerekmektedir.

İTS İlaç Takip Sistemi için en basit SOAP iletişim yöntemine dair Klasik ASP ile örneğimizi tamamladık. Bundan sonra istediğiniz gibi kod yapısında oynayıp, takla attırarak entegrasyonunuzu tamamlayabilirsiniz.

Eğer profesyonel bir destek ihtiyacı duyarsanız **[şirketimden](https://www.adjans.com.tr)** yardım talep edebilirsiniz.