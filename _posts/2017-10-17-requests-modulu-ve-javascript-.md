---
layout: post
published: true
title: Python Dryscrape Modülü ve Javascript
subtitle: Projenizde dryscrape modülünü kullanarak javascript desteği kazandırabilirsiniz.
permalink: /python-dryscrape-modulu-ve-javascript
image: /img/2017/python_logo.png
share-img: /img/2017/python_logo.png
tags: [python, development]
date: 2017-10-26
categories:
    - "python"
---
Bir önceki yazımda Python için Requests modülünün özelliklerini ve kullanımını anlatmıştım. Bu modül ile bir siteye http istekleri yapabiliyorduk. Bu istekler doğrultusunda ise sitenin kaynak kodlarını alabiliyorduk. Buraya kadar bir problem yok, fakat bazen, bağlanmaya çalıştığınız sitenin bazı yerleri javascript ile yükleniyor olabilir. Bu nedenle ancak o alanı javascript çalıştıran bir tarayıcı ile girip, ilgili alanın dolmasını sağlayıp sonra kaynak kodlarını almanız gerekir.

İşte bunun için güzel bir modül daha var, bu modülün adı **dryscrape**

## Kurulum
Ben pip3 ile kurmuştum, pip3 python3 için paket yöneticisi.

```
sudo apt-get install python3-pip
```

komutu ile pip3 kullanmaya başlayabiliriz. Eğer pip3 yüklüyse buna gerek yok, şimdi gerekli modülü kuralım.

```
sudo apt-get install qt5-default libqt5webkit5-dev build-essential python-lxml python-pip xvfb
sudo pip3 install dryscrape
```
Artık modülü kurduk, projemiz içerisine

```
import dryscrape
```

diyerek aktaralım.

{: .box-note}
**Not:** Javascript testi için ben [http://avi.im/stuff/js-or-no-js.html](http://avi.im/stuff/js-or-no-js.html?ref=sinanerdinc.com "http://avi.im/stuff/js-or-no-js.html") sayfasını kullanacağım. Eğer burayı js desteği olan bir tarayıcı ile açarsanız ekranda **Yay! Supports javascript** yazar. Eğer js desteği yoksa **No javascript support** yazar.

## Problemin Örneklenmesi
```
>>> import requests
>>> r = requests.get("http://avi.im/stuff/js-or-no-js.html")
>>> r.content
'<!DOCTYPE html>\n<html>\n<head>\n  <meta charset="utf-8">\n  <title>Javascript scraping test</title>\n</head>\n<body>\n  <p id=\'intro-text\'>No javascript support</p>\n<script type="text/javascript">\n    document.getElementById(\'intro-text\').innerHTML = \'Yay! Supports javascript\';\n</script>\n</body>\n</html>'
```
Örneğimizde requests modülü ile http://avi.im/stuff/js-or-no-js.html sitesine GET isteği yaptık sonra da sayfanın kaynağını ekrana bastırdık. Gelen kaynak içerisinde intro-text içerisinde **No javascript support** yazıyor. Kaydırma çubuğunu biraz sağa götürdüğünüzde görebilirsiniz.

## Problemin Çözümü
Şimdi ise dryscrape modülünü kullanarak bu problemi çözelim.

```
>>> import dryscrape
>>> session = dryscrape.Session()
>>> session.visit("http://avi.im/stuff/js-or-no-js.html")
>>> session.body()
'<!DOCTYPE html><html><head>\n  <meta charset="utf-8">\n  <title>Javascript scraping test</title>\n</head>\n<body>\n  <p id="intro-text">Yay! Supports javascript</p>\n<script type="text/javascript">\n    document.getElementById(\'intro-text\').innerHTML = \'Yay! Supports javascript\';\n</script>\n\n</body></html>'
```
Gördüğünüz gibi siteden dönen body içerisinde **Yay! Supports javascript** yazıyor. Kaydırma çubuğunu biraz sağa götürdüğünüzde görebilirsiniz.

Artık dryscrape modülünden dönen içeriği BeautifulSoup gibi bir modül içine verip sayfanın kaynağında istediğiniz parse işlemlerini yapabilirsiniz.
