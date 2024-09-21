---
layout: post
title: "ASP ile IP Adresi Validate Fonksiyonu"
date: 2013-01-28 09:39
categories: ["Yazılım","Classic Asp"]
tags: ["google", "search engine", "privacy", "fonksiyon", "function"]
toc: true
---

ASP ile bazı değerlerin validate edilmesi bazen zor olabilir. Her değerin validate edilebilmesi için de farklı fonksiyonlar ve kurgular gerekebiliyor. Bunun için bir IP Validate fonksiyonu hazırladım. Buyrun kullanın

### ASP İle IP Validate Fonksiyonu Kullanımı
size true yada false olarak dönecektir.

### ASP IP Validate Fonksiyonu

```javascript
Function ValidateIP(GelenIp)
	TotalScore = 0
	GelenIp = Trim(GelenIp)
	If Instr(GelenIp, ".") = 0 Then
		ValidateIP = False
		Exit Function
	Else
		Parcala = Split(GelenIp, ".")
		ToplamParca = Ubound(Parcala)
		If ToplamParca = 3 Then
			For IPValCount = 0 To UBound(Parcala)
				str_dimsel_part = Parcala(IPValCount)
				If isNull(str_dimsel_part) Or IsEmpty(str_dimsel_part) Or Not IsNumeric(str_dimsel_part) Or str_dimsel_part = "" Then
					ValidateIP = False
					Exit Function
				End If

				str_dimsel_part = Int(str_dimsel_part)
				If str_dimsel_part > 0 or str_dimsel_part < 255 Then
					TotalScore = TotalScore + 1
				Else
					ValidateIP = False
					Exit Function
				End If
			Next
		
			If TotalScore = 4 Then
				ValidateIP = True
				Exit Function
			Else
				ValidateIP = False
				Exit Function
			End If
		Else
			ValidateIP = False
			Exit Function
		End If
	End If
End Function

Response.Write ValidateIP("192.168.2.1")
```

Benzer fonksiyonları hazırladıkça ekleyeceğim.