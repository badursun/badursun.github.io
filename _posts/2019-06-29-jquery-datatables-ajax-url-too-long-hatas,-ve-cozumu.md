---
layout: post
title: "jQuery DataTables Ajax URL Too Long Hatası ve Çözümü"
date: 2019-06-29 11:10
categories: ["Yazılım", "jQuery"]
tags: ["jquery", "javascript", "datatables"]
toc: true
---

**Mini kurabiye**

DataTables'ın jQuery DataTable pluginini server-side yani ajax ile kullanıyorsanız, tablonuzda ki veri aralığı çoğaldıkça ajax processinin parametreleri POST methodu yerine GET methodu ile gönderdiğini göreceksiniz, default olarak bu da IIS tarafında QueryString limitine takılacaktır. Buna çözüm için javascript kodunuzu aşağıda ki gibi düzeltmeniz yeterli olacaktır.

```javascript
$("#AjaxDataTable").DataTable({
    "processing"    : true,
    "serverSide"    : true,
    "ajax": {
        "url"   : "ajax.asp?Cmd=",
        "type"  : "POST"
    },
    autoWidth       : !1,
    rowReorder      : { 
        selector: 'td:nth-child(' + $("#AjaxDataTable").attr("data-orderIndex") + ')' 
    },
    responsive      : true,
    paging          : true,
    searching       : true,
    iDisplayLength  : 10,
    "bSort"         : false,
    lengthMenu      : [
        [10, 20, -1],
        ["10 Satır", "20 Satır", "Hepsi"]
    ],
    language        : { 
        searchPlaceholder: "Kayıtlarda Ara" 
    },
    "drawCallback"  : function(settings) {},
    initComplete    : function(a, b) {}
})
```
