---
layout: post
published: true
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
Bu şekilde istekler gönderebiliyoruz.  Elbette ki istek göndermek ile olmuyor, parametre gönderme ihtiyacı var. Şimdi istek atarken nasıl parametre gönderebileceğimize bakalım.

# Parametre Göndermek
Parametre göndermek için params sözlüğünü kullanıyoruz. Hemen bir örnek verelim.

```
>>> r = requests.get('http://httpbin.org/get', params={"kategori":"elektronik","marka":"samsung"})
>>> r.url
'http://httpbin.org/get?marka=samsung&kategori=elektronik'
```
params sözlüğü içinde parametre geçtim, **url** methodu ile de istek yapılan url adresini görebilirsiniz. Dikkat ettiyseniz verdiğimiz parametreler bağlantı adresinin sonuna eklendi.

Bazen istek attığınız sayfa başka bir sayfaya yönleniyor olabilir, bu yönlenmeyi takip edebilir veya etmeyebiliriz, bunun için de allow_redirects=True veya False kullanılıyor.

```
>>> r = requests.get("http://httpbin.org/redirect/1",allow_redirects=False)
>>> r.status_code
302
>>> r = requests.get("http://httpbin.org/redirect/1",allow_redirects=True)
>>> r.status_code
200
```
Dikkat ederseniz allow_redirects=False yapıldığında yönlendirmeyi kapattı ve o sayfanın status_code değeri 302 döndü. True yapınca da yönlendirmeye izin verildi ve status_code 200 döndü. HTTP status kodlarını incelemek isterseniz  [buraya tıklayabilirsiniz.](https://developer.mozilla.org/tr/docs/Web/HTTP/Status "buraya tıklayabilirsiniz.")

Bu örnek GET isteği içindi, şimdi bir de POST için sanki bir HTML formu doldurmuş ve o bilgileri post etmişiz gibi davranalım.

```
>>> r = requests.post("http://httpbin.org/post", data={"username":"sinan","password":"asd123"})
>>> r.status_code
200
>>> r.json()
{'files': {}, 'args': {}, 'url': 'http://httpbin.org/post', 'json': None, 'data': '', 'headers': {'Host': 'httpbin.org', 'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'close', 'Content-Length': '30', 'User-Agent': 'python-requests/2.9.1', 'Content-Type': 'application/x-www-form-urlencoded'}, 'origin': '78.190.133.110', 'form': {'username': 'sinan', 'password': 'asd123'}}
>>> r.json()["form"]
{'username': 'sinan', 'password': 'asd123'}

```
Dikkat ettiyseniz data parametresi ile username ve password bilgilerini formdata olarak gönderdim. Ardından **status_code** methodu ile 200 yani başarılı olduğunu kontrol ettim. Ardından **json** methodu ile dönen değeri ekrana bastırdım. Kullandığımız httpbin.org servisi, post ettiğimiz parametreleri bize geri döndürüyor. Bu nedenle json içinden sadece form alanını çekerek gönderdiğim parametreleri görebildim.

İstek atarken timeout değeri de belirleyebilirsiniz, saniye olarak bir değer atayabilirsiniz.

```
>>> r = requests.get("http://httpbin.org/get", timeout=1)
```
Belirtilen timeout içinde cevap alamaz ise hata verecektir.

Burada bir parantez açalım, REST API ile çalışıyorsak, farklı endpointlere json post etmemiz gerekecek. Bu noktada da hemen json modülü bize yardım ediyor. Basit bir örnek yapalım ve bu konuyu bitirelim.

```
>>> import json
>>> import requests
>>> endpoint = "http://httpbin.org/post"
>>> myData =   {
...     "id": 1,
...     "name": "Leanne Graham",
...     "username": "Bret",
...     "email": "Sincere@april.biz",
...     "address": {
...       "street": "Kulas Light",
...       "suite": "Apt. 556",
...       "city": "Gwenborough",
...       "zipcode": "92998-3874",
...       "geo": {
...         "lat": "-37.3159",
...         "lng": "81.1496"
...       }
...     },
...     "phone": "1-770-736-8031 x56442",
...     "website": "hildegard.org",
...     "company": {
...       "name": "Romaguera-Crona",
...       "catchPhrase": "Multi-layered client-server neural-net",
...       "bs": "harness real-time e-markets"
...     }
...   }
>>> r = requests.post(endpoint, data=json.dumps(myData))
>>> r.status_code
200
```

# Özel Header Kullanımı
İstek atarken, headers parametresi ile sözlük formatında istediğiniz bilgileri girebilirsiniz.

```
>>> r = requests.post("http://httpbin.org/post",headers={"User-Agent":"Sinan-Chrome"})
>>> r.status_code
200
```
Bu örnek içerisinde headers parametresi ile özel bir user-agent değeri göndermiş oldum. **status_code** methodu ile de isteğe dönen durum kodunu kontrol ettim. Benim örneğimde 200 gelmiş, yani başarılı. 

# İstek attıktan sonra kullanılan methodlar
Tüm örneklerde bir r değişkenine aktarmıştık attığımız tüm istekleri, şimdi bu r değişkeni içinde yani requests modülü içindeki istek attıktan sonra kullanabileceğimiz methodları inceleyelim.

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

### status_code
İsteğin HTTP durum kodunu döndürür.

```
>>> r = requests.get("http://httpbin.org/")
>>> r.status_code
200
```
### history
Bir istek attınız ve status_code değeri 200 geldi. Ancak belki 2 kere 301 sonra 1 kere 302 yönlendirme ile en son bir sayfaya geldi ve 200 döndü. Buna bakabilmek için bu method kullanılıyor.

```
>>> r = requests.get("http://httpbin.org/redirect/4",allow_redirects=True)
>>> r.history
[<Response [302]>, <Response [302]>, <Response [302]>, <Response [302]>]
```
http://httpbin.org/redirect/4 adresi 4 kere 302 yönlendirmesi yapıyor. İşte buraya istek atınca history methodu ile de kontrol edince geçmişteki HTTP durum kodlarını görebiliyoruz.

### encoding
Sayfanın encode değerini döner.

```
>>> r = requests.get("http://github.com", timeout=1)
>>> r.encoding
'utf-8'
```

### request
Yaptığınız isteğin ne olduğunu döner, GET,POST v.s.

```
>>> r.request
<PreparedRequest [GET]>
>>> r.request.method
'GET'
```

### elapsed
Geçen zamanı döner.

```
>>> r.elapsed.total_seconds()
0.566292
```
