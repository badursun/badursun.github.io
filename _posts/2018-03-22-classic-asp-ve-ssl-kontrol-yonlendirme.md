---
layout: post
title: "Classic ASP ve SSL Kontrol, Yönlendirme"
date: 2018-03-22 10:17
categories: []
tags: []
toc: true
---

HTTPS protokolünün oluşturulma amacı güvensiz bir ağ ortamında güvenli bir iletişim yolu oluşturmaktır. Çünkü HTTP protokolü man-in-the-middle diye tabir edilen saldırılara açıktır. Bu tür saldırılarda ağı dinleyen saldırganlar, kişisel hesap bilgilerini ve parolaları kolay bir şekilde ele geçirebilirler.

Bu yüzden web sitelerimize SSL yükleriz ve bazen yazılımsal müdahaleler ile yönlendirmeler, SSL durumunun aktifliğini kontrol etmek gerekir. Bunun için Classic ASP de aşağıda vereceğim kodlar ile SSL kontrolünü yapabilir, gerekli durumda yönlendirme yaparak kullanıcıların tüm trafiği zorunlu olarak HTTPS protokolü üzerinden yapmasını sağlayabilirsiniz. ASP ve SSL kontrolü yapmak için vereceğim kodları kullanabilir, kendinize göre tekrar özelleştirebilirsiniz.<!--more-->
<h2>SSL Yönlendirmesi Yapan Yeri Kontrol Edin</h2>
Biz kendimiz de yönlendirsek, olası ataklara ve kaçaklara karşı gerçekten kendimizin yönlendirdiğiniz kontrol edelim.
<pre class="prettyprint">&lt;%
' #### Alan Adı Kontrol ve Yönlendirme
If Instr(Request.ServerVariables("HTTP_HOST"), "domain.com") = 0 Then
	response.redirect "http://" &amp; request.servervariables("SERVER_NAME")
	Response.End()
End If
%&gt;
</pre>
&nbsp;
<h2>Gelen Talebin Portuna Bakalım</h2>
HTTP protokolü genel olarak 80 veya 8080 portundan gerçekeleşir. HTTPS ise 443 numaralı portu kullanır. Bu yüzden gelen talebin 80 numaralı porttan olup olmadığını kontrol ediyoruz ve var olan tüm stringleri toplayıp, HTTPS protokolüne yönlendiriyoruz.
<pre class="prettyprint">&lt;%
' #### SSL ve WWW KONTROLU
If request.servervariables("SERVER_PORT") = 80 then
sPath 		= Request.ServerVariables("URL")
sQString 	= Request.Querystring
	response.redirect "https://" &amp; request.servervariables("SERVER_NAME") &amp; request.servervariables("URL")&amp;"?"&amp; sQString 
	Response.End()
End If
%&gt;
</pre>
&nbsp;
<h2>WWW Olmazsa Olmaz Diyorum</h2>
Takıntılıysanız WWW ekinin zorunlu kullanılmasını da sağlayabilirsiniz. Bunun için aşağıda ki kodu kullanmanız yeterli olacaktır. Gelen talebin başında WWW ön eki yoksa, ekleyip, mevcut adrese tekrar yönlendirecektir.
<pre class="prettyprint">&lt;%
If Left(request.servervariables("SERVER_NAME"),4) = "www." then

Else
	response.redirect "https://www." &amp; request.servervariables("SERVER_NAME") &amp; request.servervariables("URL")&amp;"?"&amp; sQString 
	Response.End()
End If
%&gt;</pre>
&nbsp;
    