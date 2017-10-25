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

Bu modül ile bir kaynak içerisindeki HTML kodlarını ayrıştırıp sadece istediğimiz alanları kesen programlar, daha popüler olarak çeşitli siteler için BOT yazabilirsiniz.

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
**Not:** Ben örneklerimde [https://www.producthunt.com](https://www.producthunt.com?ref=sinanerdinc.com "https://www.producthunt.com") sitesine bağlanıp burdaki kodları parse ettim.

# Siteye Bağlanma ve Parse İşlemine Hazırlık
```
>>> import requests
>>> from bs4 import BeautifulSoup
>>> r = requests.get('https://www.producthunt.com')
>>> source = BeautifulSoup(r.content,"lxml")
>>> source.title
<title>Product Hunt</title>
```
Requests modülü ile siteye bir GET isteği yaptık bunu bir r objesine atadık, sonra da BeautifulSoup içine **r.content** değerini verdik, burda hangi kütüphane ile parse etmesini istediğinizi siz belirliyorsunuz. Ben **lxml** seçtim, çünkü hafif ve hızlı, kullanabileceğiniz kütüphaneler şunlar;

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
Bu örnek içerisinde sayfadaki ilk p etiketi içindeki değeri ekrana getirdi. Dikkat ettiyseniz html etiketleri hala duruyor, eğer bunları da kaldırmak ve sadece yazıya ulaşmak isterseniz ise **text** methodunu kullanabilirsiniz.

```
>>> source.find("p").text
"Product Hunt surfaces the best new products, every day. It's a place for product-loving enthusiasts to share and geek out about the latest mobile apps, websites, hardware projects, and tech creations."
```
Başarıyla yazıyı alabildik. Peki siz sitedeki bir url adresini çekmek istiyorsunuz ancak belir bir niteliği olan url adresini istiyorsunuz, mesela a class="item_1523a" olan url adresini getirmek istesem?

```
>>> source.find("a",attrs={"class":"item_1523a"}).text
'Home'
```
Bu bize https://www.producthunt.com/ sitesindeki solda bulunan FEEDS menüsündeki ilk bağlantı adresinin adını getirdi, yani Home. Bunu almak için de **attrs** sözlüğünü parametre olarak geçtik. Aslında attrs yazmadan direkt olarak sözlüğü de parametre olarak geçebilirsiniz ben yazmayı seviyorum.

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

Gördüğünüz gibi sitedeki sol taraftaki bağlantı adreslerinin hepsini çektik. Burda dikkat etmeniz gereken şu, find_all ile sayfadaki tüm değerleri çektiğimiz için, bunu bir döngüye sokarak içindeki elemanlara ulaşıyoruz. Ek olarak find_all methodunu limitleyebilirsiniz. Yani herşeyi getirme sadece 3 tanesini getir diyebilirsiniz, bunun için de **limit=x** gibi bir parametre geçmeniz gerek.
```
source.find_all("a",attrs={"class","item_1523a"}, limit=2)
```

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

### next_sibling
İngilizcede sibling kardeş anlamına geliyor, burda da sayfa içinde seçtiğiniz html etiketi ile aynı seviyede diğer html etiketini getirir.. Basit bir örnek verelim.
```
<ul>
<b>Test1</b>
<c>Test2</c>
</ul>
```

Yukarıdaki örnek de Test1 ile Test2 kardeş çünkü aynı seviyedeler.

```
>>> source.find("ul",attrs={"class":"list_0372b"}).li.text
'Home'
>>> source.find("ul",attrs={"class":"list_0372b"}).li.next_sibling.text
'Tech'
```
Ben de yine soldaki menüde bulunan linklere ulaşmak istedim, bu linkler bir ul etiketi içindeydi, onun class değerini parametre olarak geçtim sonra da bu **ul** etiketinin altındaki ilk **li** değerinin text sonucu aldım. Bu da **Home** değeriymiş. Sonra da next_sibling methodu ile de bu **Home** değerinden sonraki linki istedim o da geldi **Tech** olarak.

### previous_sibling
Yukarıda anlatılan olayın tam tersi,  yani seçilen etiketin kardeşi olan ama bu etiketten bir önce geleni getirir.
### next_siblings ve previous_siblings
Bunlar da find ile find_all arasındaki ilişki gibi, önce veya sonra 1 kardeş etiketi değil tüm kardeş etiketi getir anlamında kullanılıyor. Seçtiğiniz etiket ile kardeş altında veya üzerinde olan alanları getirmek istediğinizde kullanabilirsiniz.
### select
Css selector kullanarak da seçim yapabilirsiniz, mesela sayfa kaynağındaki sol taraftaki menülerden 3. sünün adını getirelim.
```
>>> get_links = source.find("ul",attrs={"class":"list_0372b"}).select("li:nth-of-type(3)")
>>> for i in get_links:
...     print(i.text)
...
Games
```
Görüldüğü gibi Games geldi. Bir örnek daha yapalım
```
>>> source.select("html > body > main > div > div > div > div > div > div > a:nth-of-type(2) > span")
[<span>The best new products, every day</span>]
```
şimdi de sitenin sloganını çektik. En tepeden html etiketinden başlayarak alt etiketlere indik.
```
>>> source.select("p.text_44214")
[<p class="text_44214">Product Hunt surfaces the best new products, every day. It's a place for product-loving enthusiasts to share and geek out about the latest mobile apps, websites, hardware projects, and tech creations.</p>]
>>> source.select("main#app")
```
Class ve Id için de yukarıdaki gibi seçiciler kullanıyoruz. Niteliğine göre de seçicileri düzenleyebiliriz.
```
soup.select('a[href="http://example.com/elsie"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

soup.select('a[href^="http://example.com/"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select('a[href$="tillie"]')
# [<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select('a[href*=".com/el"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
```
Bu örneği BeautifulSoup dökümanından aldım.
