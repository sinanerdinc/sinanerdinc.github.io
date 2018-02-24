---
layout: post
published: true
title: Python BeautifulSoup Eğitim Videoları
subtitle: Python Requests ve BeautifulSoup modülünü kullanarak bot yazabilirsiniz.
permalink: /python-beautifulsoup-egitim-videolari
image: /img/2018/youtube.png
share-img: /img/2018/youtube.png
date: 2018-02-23
categories:
    - "python"
---

Python Requests ve BeautifulSoup modüllerini kullanarak bot yazabilir, bir web sitenin sayfa kaynağından istediğiniz alanı çekebilirsiniz. Python bot yapımı ile ilgili hazırladığım eğitim videolarını buradan izleyebilirsiniz.

Videolar kolaydan zora doğru gidiyor, her video içeriğinde farklı methodlar kullanmaya, alışılmadık problemler çözmeye çalıştım.

# Örnek 1

{: .box-note}
Site: http://steamsales.rhekua.com/


<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/FuiOiltXJbM"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>

Bu örnek içerisinde stem üzerindeki indirimlerin olduğu websitesine bağlandım ve oyunların adını, indirimini, fiyatını aldım.

{: .box-note}
Kodlar: https://github.com/sinanerdinc/ParseHtml/blob/master/steam.py


# Örnek 2

{: .box-note}
Site: https://boxofficeturkiye.com/hafta/?yil=2018&hafta=7


<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/L61uiXspCq4"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>

Bu örnek içerisinde boxofficeturkiye sitesine bağlandım, son çıkan filmlerin adını, izleyici adetlerini ve hasılatını çektim.

{: .box-note}
Kodlar: https://github.com/sinanerdinc/ParseHtml/blob/master/boxoffice.py


# Örnek 3

{: .box-note}
Site: https://www.python.org/jobs


<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/NeYdF5Drsy4"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>

Bu örnek içerisinde python resmi sitesine bağlandım, burdaki güncel iş ilanlarını çektim, 7 sayfa iş ilanı olduğu için kodlarımı her sayfayı ayrı ayrı dolaşarak toplayacak şekilde ayarladım.

{: .box-note}
Kodlar: https://github.com/sinanerdinc/ParseHtml/blob/master/pythonJobs.py
