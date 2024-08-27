---
layout: post
title: "Classic ASP ile SOAP iletişimi"
date: 2016-11-26 13:18
categories: ["Yazılım", "Classic Asp"]
tags: ["soap", "classic-asp", "asp"]
---

ASP ile SOAP kullanımı biraz dertlidir. Ben yakın zamanda ki bir projemde iletişimi sağlamak için hazırladığım kod örneğini paylaşıyorum. Tabii yapıya göre, node isimlerine göre düzenlemeniz gerekebilir fakat ilk adım iletişimi sağlamaktır. Aşağıda ki kod örneği ile bu iletişimi sağlayabilirsiniz. s= stringi içinde SOAP yığını oluşturulur. 

Bu yığın POST_URL kısmında ki adrese POST edilir. Güvenlik için bazı header bilgilerine ihtiyaç duyabilirsiniz. Kod içinde ilgili örnekler mevcuttur. Gidecek header bilgilerini yapınıza ve iletişim kurduğunuz sisteme göre düzenlemeniz mümkündür.

## ASP İle Soap İletişim Örneği
```javascript
<%
' Sayfa Tanımları ve Değişkenler
Response.Charset = "UTF-8"
Response.CodePage = 65001
Cmd = Request.Querystring("Cmd")

'İŞLEM SÜRESİNİ TAKİP ETMEK İÇİN ZAMANINI BAŞLAT
starttime = Timer 

' HATALARI MANİPÜLE EDELİM
On Error Resume Next

'SOAP YAPISINI KURALIM
s= "<s:Envelope xmlns:s=""http://schemas.xmlsoap.org/soap/envelope/"">"
s=s& "  <s:Body>"
s=s& "      <UyeBul xmlns=""http://other.url.com.sample/"">"
s=s& "      <login xmlns:a=""http://schemas.datacontract.org/2004/07/Services.DTO"" xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"">"
s=s& "          <a:KullaniciAdi>XXX</a:KullaniciAdi>"
s=s& "          <a:Parola>XXXXXX</a:Parola>"
s=s& "      </login>"
s=s& "      <skip i:nil=""true"" xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" />"
s=s& "      <take i:nil=""true"" xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" />"
s=s& "          <uyeTipleri xmlns:a=""http://schemas.microsoft.com/2003/10/Serialization/Arrays"" xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"">"
s=s& "          </uyeTipleri>"
s=s& "      <ozellikIdler i:nil=""true"" xmlns:a=""http://schemas.datacontract.org/2004/07/System"" xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" />"
s=s& "      <anahtarKelime>QUERY</anahtarKelime>"
s=s& "      </UyeBul>"
s=s& "      </s:Body>"
s=s& "</s:Envelope>"

' FONKSİYONLAŞTIRMA
Function ReturnNode(objXMLDocResponse, NodePath, Tip)
    If Tip = "NAME" Then 
        ReturnNode = objXMLDocResponse.selectSingleNode(NodePath).NodeName
    End If
    If Tip = "VAL" Then 
        ReturnNode = objXMLDocResponse.selectSingleNode(NodePath).Text
    End If
End Function

'İLETİŞİME GEÇELİM
' Set oRequest = Server.CreateObject("MSXML2.ServerXMLHTTP")
Set oRequest = Server.CreateObject("Microsoft.XMLHTTP")
    ' oRequest.setOption(2) = 13056 'SXH_SERVER_CERT_IGNORE_ALL_SERVER_ERRORS
    ' oRequest.setTimeouts 10000, 10000, 10000, 10000
    oRequest.Open "POST", "POST_URL", False
    oRequest.setRequestHeader "Content-type", "text/xml; charset=utf-8"
    oRequest.setRequestHeader "SOAPAction", "SOAP_HEADER_DATASI"
    oRequest.send s

'HATA VARSA YAZ, YOKSA CEVABI DÖNDÜR.
If Err <> 0 Then
    Response.Write "<pre>"
    Response.Write Err.description
    Response.Write "</pre>"
Else
    If Cmd = "xml" Then 
        Response.ContentType = "text/xml"
        Response.Write oRequest.responseText
    Else
        ' XML PARSE
        Set objXMLDocResponse = oRequest.responseXML
        str_HATA_DURUMU = objXMLDocResponse.selectSingleNode("s:Envelope/s:Body/s:Fault/faultstring").Text

        If str_HATA_DURUMU = "" Then
            Response.Write "HATA YOK<br>"
        Else
            Response.Write "HATA KODU: " & ReturnNode(objXMLDocResponse, "s:Envelope/s:Body/s:Fault/faultcode", "VAL")
            Response.Write "<br>"
            Response.Write "HATA AÇIKLAMA: " & ReturnNode(objXMLDocResponse, "s:Envelope/s:Body/s:Fault/faultstring", "VAL")
            Response.Write "<br>"
            Response.Write "ÜRETİM SÜRESİ: " & FormatNumber(Timer - starttime , 4) &" s."
        End If
        Set objXMLDocResponse = Nothing
    End If

    ' Response.Write oRequest.responseXML.xml
    ' Response.Write oRequest.responseXML.getElementsByTagName("faultstring")(0).innerText
End If
%>
```