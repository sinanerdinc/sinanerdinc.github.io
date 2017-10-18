---
layout: post
published: false
title: Python Requests Modülü
subtitle: Web isteklerinizi python ile yapın
permalink: /python-requests-modulu
image: /img/2017/python_logo.png
tags: [python, development]
date: 2017-10-17
categories:
    - "python"
---
Python, standart modüllerinin yanında harici yüzlerce kullanışlı modül ile birlikte çok güçlü bir dil. Bu gücü veren harika modüller var bunlardan biri de **Requests modülü.**

Bu modül ile web üzerindeki isteklerinizi yöneteceksiniz. Mesela bu modül ile API entpointlerine PUT, DELETE, POST gibi istekler atabilirsiniz.

## Kurulum
Ben pip3 ile kurmuştum, pip3 python3 için paket yöneticisi.
```
sudo apt-get install python3-pip
```

komutu ile pip3 kullanmaya başlayabiliriz. Eğer pip3 yüklüyse buna gerek yok, şimdi requests modülünü kuralım.

```
pip3 install requests
```
Artık modülü kurduk, projemiz içerisine
```
import requests
```

diyerek aktaralım. Şimdi de bu güzel modülün özelliklerine geçelim.

{: .box-note}
**Not:** Bütün örneklerde http://httpbin.org/ adresini kullanacağım, bu adres HTTP isteği yapıp cevap alabileceğiniz bir servis.

# HTTP İstekleri
```
>>> r = requests.get('http://httpbin.org/get')
>>> r = requests.post('http://httpbin.org/post')
>>> r = requests.put('http://httpbin.org/put')
>>> r = requests.delete('http://httpbin.org/delete')
```
Bu şekilde istekler gönderebiliyoruz.  Elbette ki istek göndermek ile olmuyor, parametre geçme ihtiyacı var. Şimdi istek atarken nasıl parametre gönderebileceğimize bakalım.

# Parametre Göndermek
Parametre göndermek için params sözlüğünü kullanıyoruz. Hemen bir örnek verelim.
```
>>> r = requests.get('http://httpbin.org/get', params={"kategori":"elektronik","marka":"samsung"})
>>> r.url
'http://httpbin.org/get?marka=samsung&kategori=elektronik'
```
params sözlüğü içinde parametre geçtim, **url** methodu ile de istek yapılan url adresini görebilirsiniz. Dikkat ettiyseniz verdiğimiz parametreler bağlantı adresinin sonuna eklendi.

# Özel Header Kullanımı
İstek atarken, headers parametresi ile sözlük formatında istediğiniz bilgileri girebilirsiniz.
```
>>> r = requests.post("http://httpbin.org/post",headers={"User-Agent":"Sinan-Chrome"})
>>> r.status_code
200
```
Bu örnek içerisinde headers parametresi ile özel bir user-agent değeri göndermiş oldum. **status_code** methodu ile de isteğe dönen durum kodunu kontrol ettim. Benim örneğimde 200 gelmiş, yani başarılı.

## get(website)
Parametre olarak gönderilen adrese get isteği gönderir, bir değişkene tanımlayıp daha sonra HTTP response & status kodunu da kontrol edebilirsiniz. HTTP status kodlarını incelemek isterseniz  [buraya tıklayabilirsiniz.](https://developer.mozilla.org/tr/docs/Web/HTTP/Status){:target="_blank"}

```
>>> r = requests.get("http://httpbin.org/")
>>> r
<Response [200]>
```

r değişkeni içerisinde kullanabileceğimiz bazı methodlar var.

### text
Sitenin HTML içeriğini döndürür.

```
>>> r = requests.get("http://httpbin.org/")
>>> r.text
```

### headers
Header bilgilerini gösterir.

```
>>> r = requests.get("http://httpbin.org/")
>>> r.headers
{'Content-Length': '13011', 'Content-Type': 'text/html; charset=utf-8', 'Server': 'meinheld/0.6.1', 'Date': 'Tue, 17 Oct 2017 21:29:49 GMT', 'Connection': 'keep-alive', 'Via': '1.1 vegur', 'X-Processed-Time': '0.00539398193359', 'Access-Control-Allow-Credentials': 'true', 'Access-Control-Allow-Origin': '*', 'X-Powered-By': 'Flask'}
```
Dilerseniz istediğiniz header bilgilerini siz get isteği ile birlikte belirtebilirsiniz.

```
r = requests.get("http://httpbin.org/", headers={'user-agent': 'sinanerdinc'})
```

### url
Hangi url adresine istek gönderdiğini döner. İstek yaparken parametre olarak bazı değerler gönderebilirsiniz.

```
>>> r = requests.get("http://httpbin.org/", params={"ad":"sinan","soyad":"erdinc"})
>>> r.url
'http://httpbin.org/?soyad=erdinc&ad=sinan'
```

Url adresinin sonunda verdiğim parametrelerin nasıl iliştirildiğine dikkat edin.
