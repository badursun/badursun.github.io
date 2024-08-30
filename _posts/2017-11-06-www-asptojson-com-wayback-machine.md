---
layout: post
title: "www.asptojson.com wayback machine"
date: 2017-11-06 10:52
categories: ["Yazılım", "Classic Asp"]
tags: ["json", "classic-asp-json-parse", "asp", "aspjson", "fonksiyonlar", "functions"]
toc: true
---

ASPJSON is a free to use project for generating and reading JSON data into a classic ASP object. The class can be used for reading a string of JSON data as well as writing JSON output from an AJAX file. Below are 2 simple examples of both.

ASPJSON is a free to use project for generating and reading JSON data into a classic ASP object. The class can be used for reading a string of JSON data as well as writing JSON output from an AJAX file. Below are 2 simple examples of both. Please feel free to give feedback or to report a bug.

MIRROR: http://www.aspjson.com

```javascript
<!--#include virtual="/aspJSON1.17.asp"-->
<%
Set oJSON = New aspJSON

With oJSON.data

    .Add "familyName", "Smith"                      'Create value
    .Add "familyMembers", oJSON.Collection()

    With oJSON.data("familyMembers")

        .Add 0, oJSON.Collection()                  'Create object
        With .item(0)
            .Add "firstName", "John"
            .Add "age", 41
        End With

        .Add 1, oJSON.Collection()
        With .item(1)
            .Add "firstName", "Suzan"
            .Add "age", 38
            .Add "interests", oJSON.Collection()    'Create array
            With .item("interests")
                .Add 0, "Reading"
                .Add 1, "Tennis"
                .Add 2, "Painting"
            End With
        End With

        .Add 2, oJSON.Collection()
        With .item(2)
            .Add "firstName", "John Jr."
            .Add "age", 2.5
        End With

    End With

End With

Response.Write oJSON.JSONoutput()                   'Return json string
%>
```

**Release 1.17 Februari 2014**
- Efficiency improvement large data

**Release 1.15 Februari 2014**
- Trailing tabs fixed

**Release 1.14 Februari 2014**
- Colon value within string bug fixed

**Release 1.13 December 2013**
- Encoded data fix
- Now possible to load directly from a URL.
For example: oJSON.loadJSON("https://www.aspjson.com/jsonstream.asp")

**Release 1.12 June 2013**
- vbCrLf fix
- Compatible with Option Explicit
- JSON escape characters

Son Wayback Machine Görüntüsü İçin [tıklayın](https://web.archive.org/web/20160620154733/http://www.aspjson.com:80/)

Benim tarafımdan bazı hataları giderilmiş ve geliştirilmiş güncel sürümü indirmek için: [aspJSON1.17-badursun.asp.zip](https://drive.google.com/file/d/1ephhFyO44BNlp3B6v1BVKc7pLqjJvlRg/view?usp=sharing)