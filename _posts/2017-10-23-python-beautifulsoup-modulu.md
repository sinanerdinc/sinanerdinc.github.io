---
layout: post
published: false
title: Python BeautifulSoup Modülü
subtitle: Html veya Xml parse işlemlerinizi python ile yapabilirsiniz.
permalink: /python-beautifulsoup-modulu
image: /img/2017/python_logo.png
tags: [python, development]
date: 2017-10-23
categories:
    - "python"
---
BeautifulSoup, HTML veya XML dosyalarını işlemek için oluşturulmuş güçlü ve hızlı bir kütüphanedir. Adını Alice harikalar diyarında içerisindeki bir kaplumbağanın söylediği hikayeden alır.

Bu modül ile bir kaynak içerisindeki HTML kodlarını ayrıştırıp sadece istediğimiz alanları kesen programlar yazabiliriz.

## Kurulum
Ben pip3 ile kurmuştum, pip3 python3 için paket yöneticisi.

```
sudo apt-get install python3-pip
```

komutu ile pip3 kullanmaya başlayabiliriz. Eğer pip3 yüklüyse buna gerek yok, şimdi bu modülü kuralım.

```
pip3 install beautifulsoup4
```
Artık modülü kurduk, projemiz içerisine

```
from bs4 import BeautifulSoup
```

diyerek aktaralım. Bu modül ile bir websitesinin HTML kodlarını alacağımız için daha önce detaylıca anlattığım Requests modülünü de kullanacağız.

```
import requests
```

Artık kodlarımızı yazabiliriz.

{: .box-note}
**Not:** Ben örneklerimde https://www.producthunt.com sitesine bağlanıp burdaki kodları parse ettim.

# Siteye bağlanma ve parse işlemine hazırlık
```
>>> import requests
>>> from bs4 import BeautifulSoup
>>> r = requests.get('https://www.producthunt.com')
>>> source = BeautifulSoup(r.content,"lxml")
>>> source.title
<title>Product Hunt</title>
```
Requests modülü ile siteye bir GET isteği yaptık bunu bir r objesine atadık, sonra da BeautifulSoup içine r.content değerini verdik, burda hangi kütüphane ile parse etmesini istediğinizi siz belirliyorsunuz. Ben **lxml** seçtim, kullanabileceğiniz kütüphaneler şunlar;

 * lxml
 * html.parser
 * lxml-xml
 * html5lib

Ardından source objesine sayfanın HTML kodlarını aldık, kontrol etmek için de **source.title** ile sayfanın title etiketi içindeki değerini bastırdık. Herşey başarılı gözüküyor. Bundan sonraki örneklerde yukarıdaki kodları tekrar yazmadan direkt source objesinden ilerleyeceğim.

### find(value)
Sayfa kaynağında geçen özel bir değeri çekmenize imkan tanır.

```
>>> source.find("p")
<p class="text_44214">Product Hunt surfaces the best new products, every day. It's a place for product-loving enthusiasts to share and geek out about the latest mobile apps, websites, hardware projects, and tech creations.</p>
```
Bu örnek içerisinde sayfadaki ilk p etiketi içindeki değeri ekrana getirdi. Dikkat ettiyseniz html etiketleri hala duruyor, eğer bunları da kaldırmak ve sadece metne ulaşmak isterseniz ise text değerini kullanabilirsiniz.

```
>>> source.find("p").text
"Product Hunt surfaces the best new products, every day. It's a place for product-loving enthusiasts to share and geek out about the latest mobile apps, websites, hardware projects, and tech creations."
```
Peki herşey çok güzel, ancak siz p değerinin bir niteliği olanı çekmek istiyorsunuz, mesela a class="item_1523a" olan değeri getirmek istiyoruz.

```
>>> source.find("a",attrs={"class":"item_1523a"}).text
'Home'
```
Bu bize https://www.producthunt.com/ sitesindeki solda bulunan FEEDS menüsündeki ilk bağlantı adresinin adını getirdi, yani Home. Bunu almak için de attrs sözlüğünü parametre olarak geçtik.

### find_all(value)
Sayfa kaynağında geçen seçtiğiniz tüm özel değerleri çekmenize imkan tanır. Yani find ile 1 adet, find_all ile tüm değerleri çekiyoruz. Yukarıdaki örneği find_all ile yapalım.

```
>>> solmenu = source.find_all("a",attrs={"class":"item_1523a"})
>>> for link in solmenu:
...     print(link.text)
...
Home
Tech
Games
Books
Artificial Intelligence
Developer Tools
Home
Productivity
Touch Bar Apps
Wearables
All Topics
Customize Your Feed
>>>
```

Gördüğünüz gibi sitedeki sol taraftaki bağlantı adreslerinin hepsini çektik. Burda dikkat etmeniz gereken şu, find_all ile sayfadaki tüm değerleri çektiğimiz için, bunu bir döngüye sokarak içindeki elemanlara ulaşıyoruz.

Peki ya bu linklerin adını değil de yönlendiği adresi çekmek isteseydik? O zaman da **get("href")** değerini kullanmalıydık.

```
>>> for link in solmenu:
...     print(link.get("href"))
...
/
/topics/tech
/topics/games
/topics/books
/topics/artificial-intelligence
/topics/developer-tools
/topics/home
/topics/productivity
/topics/touch-bar-apps
/topics/wearables
/topics
/yours
```
Gördüğünüz gibi döngü içinde her bir link için **get("href")** ile bağlantı adreslerini çektik.
