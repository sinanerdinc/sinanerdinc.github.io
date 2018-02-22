---
layout: post
published: true
title: Python BeautifulSoup Modülü
subtitle: Html veya Xml parse işlemlerinizi python ile yapabilirsiniz.
permalink: /python-beautifulsoup-modulu
image: /img/2017/python-beautifulsoup.png
share-img: /img/2017/python-beautifulsoup.png
date: 2017-10-30
categories:
    - "python"
---
BeautifulSoup, HTML veya XML dosyalarını işlemek için oluşturulmuş güçlü ve hızlı bir kütüphanedir. Adını Alice harikalar diyarında içerisindeki bir kaplumbağanın söylediği hikayeden alır.

Bu modül ile bir kaynak içerisindeki HTML kodlarını ayrıştırıp sadece istediğimiz alanları kesen programlar, daha popüler adıyla BOT yazabilirsiniz.

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

diyerek aktaralım. Bu modül ile bir websitesinin HTML kodlarını alacağımız için daha önce detaylıca [anlattığım](http://www.sinanerdinc.com/python-requests-modulu "Python Requests Modülü") Requests modülünü de kullanacağız.

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
Başarıyla yazıyı alabildik. Peki siz sitedeki belirli bir seçiciye sahip bir url adresini çekmek istiyorsunuz, mesela a class="item_1523a" olan url adresini getirmek istesem?

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
Gördüğünüz gibi döngü içinde her bir link için **get("href")** ile bağlantı adreslerini çektik. Sadece buraya kadar öğrendiklerimiz ile mesela bir uygulama yazabiliriz, bu uygulama da bir site içerisindeki tüm linkleri ekrana yazdırır, web sitenizden hangi sayfalara çıkış yapılabiliyor onları incelemek isteyebilirsiniz.

Peki herhangi bir seçici belirtmeden direkt olarak metin aramak istediğimizde?

```
>>> source.find_all(string=["Sign Up","Log In"])
['Log In', 'Sign Up']
>>> source.find_all(string=["Sign Up","Log Inssss"])
['Sign Up']
```

ilk satırda Sign Up ve Log In metinlerini aradım ikisi de olduğu için aynı şekilde geri liste olarak döndü, ikinci örnekte ise Log Insss aradım, böyle bir metin olmadığı için sadece olan metin Sign Up geri döndü. Bunu bir seçici ile birlikte de kullanabiliriz.

```
>>> source.find_all("span",string="Subscribe")
[<span class="font_9d927 xSmall_1a46e semiBold_e201b buttonContainer_b6eb3 uppercase_a49b4">Subscribe</span>]
```
Burda ise span etiketi içinde string değeri Subscribe olanları getir dedik.

### next_sibling
İngilizcede sibling kardeş anlamına geliyor, burda da sayfa içinde seçtiğiniz html etiketi ile aynı seviyede diğer html etiketini getirir.. Basit bir örnek verelim.

```
<ul>
<b>Test1</b>
<c>Test2</c>
</ul>

```

Yukarıdaki örnek de Test1 ile Test2 kardeş çünkü aynı seviyedeler. Şimdi biz kendi örneğimizde bir sonraki kardeşi bulalım.

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
```
Kaynak içindeki p etiketlerinden text_44214 class değerine sahip olanı çektik.

```
>>> source.select('a[href="/ship"]')
[<a href="/ship">Ship <span style="font-style:normal;font-weight:normal">⛵</span></a>]
```
Linkler içerisinden href değeri /ship olanı çektik. Bazen de tam olarak bağlantı adresini bilemezsiniz, mesela bağlantı adresi belirli bir şablon ile başlayanları çekmek istersiniz. Örnek verelim;

```
>>> linkler = source.select('a[href^="/topics/t"]')
>>> for link in linkler:
...     print(link.text)
...     print(link.get("href"))
...
Tech
/topics/tech
Touch Bar Apps
/topics/touch-bar-apps
```

Burda ise bağlantı adresleri içinde href değeri /topics/t ile başlayanları çektik. Bunlar da **tech** ve **touch-bar-apps** adresleriymiş.

Buraya kadar HTML ağaç yapısı içerisindeki istediğimiz verileri aldık, şimdi de bu verileri modifiye etmeyi görelim.

# Html Kodlarını Modifiye Etme
Bir kaynaktaki html kodun kopyasını aldıktan sonra bu kopyası üzerinde yapabileceğimiz değişikliklere bakalım.

## Bir etiketin id veya class değerini değişirme
Kaynak içindeki bir li etiketine class veya id değeri atayabilirsiniz. Hemen bir örnek yapalım.

```
>>> source.find("ul",attrs={"class":"list_0372b"}).next
<li><a class="item_1523a activeItem_c89bf item_1523a" href="/"><div class="greyIcon_75bcc icon_ae8d4"><span><svg height="12" viewbox="0 0 12 12" width="12" xmlns="http://www.w3.org/2000/svg"><path d="M11.587 5.25L10.5 4.163V1.5a.752.752 0 0 0-.75-.75H9a.752.752 0 0 0-.75.75v.415L6.75.416C6.545.223 6.358 0 6 0s-.545.223-.75.416L.412 5.25C.178 5.494 0 5.672 0 6c0 .422.324.75.75.75h.75v4.5c0 .413.338.75.75.75H4.5V8.25c0-.413.338-.75.75-.75h1.5c.413 0 .75.337.75.75V12h2.25c.413 0 .75-.337.75-.75v-4.5h.75c.426 0 .75-.328.75-.75 0-.328-.178-.506-.413-.75z" fill="#999" fill-rule="evenodd"></path></svg></span></div><span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">Home</span></a></li>
```
Burda producthunt sitesindeki soldaki menülerden ilki olan Home değerini çektik. Dikkat ettiyseniz gelen değer li etiketi ile başlıyor. Şimdi de bu li değerine class atayalım.

```
>>> source.find("ul",attrs={"class":"list_0372b"}).next["class"] = "changed"
>>> source.find("ul",attrs={"class":"list_0372b"}).next
<li class="changed"><a class="item_1523a activeItem_c89bf item_1523a" href="/"><div class="greyIcon_75bcc icon_ae8d4"><span><svg height="12" viewbox="0 0 12 12" width="12" xmlns="http://www.w3.org/2000/svg"><path d="M11.587 5.25L10.5 4.163V1.5a.752.752 0 0 0-.75-.75H9a.752.752 0 0 0-.75.75v.415L6.75.416C6.545.223 6.358 0 6 0s-.545.223-.75.416L.412 5.25C.178 5.494 0 5.672 0 6c0 .422.324.75.75.75h.75v4.5c0 .413.338.75.75.75H4.5V8.25c0-.413.338-.75.75-.75h1.5c.413 0 .75.337.75.75V12h2.25c.413 0 .75-.337.75-.75v-4.5h.75c.426 0 .75-.328.75-.75 0-.328-.178-.506-.413-.75z" fill="#999" fill-rule="evenodd"></path></svg></span></div><span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">Home</span></a></li>
```
Şimdi de bu li etiketine changed adında bir class atadık. Tekrar bu li etiketini sorguladığımızda da görebildik. Hadi bir de id ekleyelim bu li etiketine.

```
>>> source.find("ul",attrs={"class":"list_0372b"}).next["id"] = "newId"
>>> source.find("ul",attrs={"class":"list_0372b"}).next
<li class="changed" id="newId"><a class="item_1523a activeItem_c89bf item_1523a" href="/"><div class="greyIcon_75bcc icon_ae8d4"><span><svg height="12" viewbox="0 0 12 12" width="12" xmlns="http://www.w3.org/2000/svg"><path d="M11.587 5.25L10.5 4.163V1.5a.752.752 0 0 0-.75-.75H9a.752.752 0 0 0-.75.75v.415L6.75.416C6.545.223 6.358 0 6 0s-.545.223-.75.416L.412 5.25C.178 5.494 0 5.672 0 6c0 .422.324.75.75.75h.75v4.5c0 .413.338.75.75.75H4.5V8.25c0-.413.338-.75.75-.75h1.5c.413 0 .75.337.75.75V12h2.25c.413 0 .75-.337.75-.75v-4.5h.75c.426 0 .75-.328.75-.75 0-.328-.178-.506-.413-.75z" fill="#999" fill-rule="evenodd"></path></svg></span></div><span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">Home</span></a></li>
```
Gördüğünüz gibi newId adında bir id ekledim ve **li class="changed" id="newId"** şekline geldi.

## String değerini değiştirmek
Şimdi de producthunt sitesindeki soldaki menülerden ilki olan Home değerini Anasayfa olarak değiştirelim.

```
>>> source.find("span",attrs={"class":"itemText_063f9"})
<span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">Home</span>
>>> source.find("span",attrs={"class":"itemText_063f9"}).string
'Home'
>>> source.find("span",attrs={"class":"itemText_063f9"}).string = "Anasayfa"
>>> source.find("span",attrs={"class":"itemText_063f9"})
<span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">Anasayfa</span>
```
İlk önce span etiketlerinden belirli bir class değerini çekerek Home bağlantısına ulaştık. Ardından .string methodu ile ekrana diret olak html hali olmadan metni bastırdık ve **Home** değerine ulaştık. sonra da bu Home değerini **Anasayfa** olacak şekilde değiştirdik. Sonra tekrar sorguladığımızda Home değerinin artık olmadığını Anasayfa değerinin olduğunu görebildik.

Şimdi de bu Anasayfa yazan yeri .append() methodu ile değiştirelim.

```
>>> source.find("span",attrs={"class":"itemText_063f9"})
<span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">Anasayfa</span>
>>> source.find("span",attrs={"class":"itemText_063f9"}).append("Adresi")
>>> source.find("span",attrs={"class":"itemText_063f9"})
<span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">AnasayfaAdresi</span>
```
Anasayfa yazdığını kontrol ettim sonra ise .append("Adresi") ile Anasayfa metninin AnasayfaAdresi olmasını sağladım. Yani append ile bu alandaki metnin yanına ekleme yaptım.

## Yeni bir etiket ekleme ve silme
Seçtiğimiz bir html etiketi içerisine istersek yeni bir etiket daha ekleyebiliriz.

```
>>> source.find("span",attrs={"class":"itemText_063f9"})
<span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">AnasayfaAdresi</span>
>>>
>>> tag = source.new_tag("b")
>>> tag.string = "Bold Metin"
>>>
>>> source.find("span",attrs={"class":"itemText_063f9"}).string.insert_before(tag)
>>> source.find("span",attrs={"class":"itemText_063f9"}).b
<b>Bold Metin</b>
```
İlk önce **AnasayfaAdresi** adresini seçtik, sonra **tag** adında bir değişken içerisine direkt olarak source içine new_tag methodu ile bir **b** etiketi ekledik. Sonra da tag.string ile bu **b** etiketinin içerisine ne yazması gerektiğini söyledik. Şimdi de AnasayfaAdresi metnininden önce insert_before(tag) methodu ile tag değişkenini enjekte ettik. Sonra da **b** tagı var mı diye kontrol ettik başarıyla geldiğini görebildik.

Burda **insert_before()** methodunu kullandık, böylece AnasayfaAdresi metninin önüne ekledik, eğer sonrasına eklemek isteseydik **insert_after()** methodunu kullanmalıydık.

Bir etiketin içindeki değerleri silmek istersek;

```
>>> source.find("span",attrs={"class":"itemText_063f9"}).b
<b>Bold Metin</b>
>>> source.find("span",attrs={"class":"itemText_063f9"}).b.clear()
>>> source.find("span",attrs={"class":"itemText_063f9"}).b
<b></b>
```
İlk önce az önce eklediğim Bold Metin değerini buldum, sonra clear() methodunu uyguladım, sonra tekrar sorguladığımda Bold Metin değerinin silindiğini gördüm.

Peki metni değil de etiket ve içindekileri komple silmek isteseydik?

```
>>> source.find("span",attrs={"class":"itemText_063f9"}).b.decompose()
>>> source.find("span",attrs={"class":"itemText_063f9"}).b
```
O zaman da decompose() methodunu uygulamamız gerek, ben uyguladım sonra tekrar b etiketini sorguladım herhang birşey dönmedi. Yani sildi.

# Çıktı Formatları
BeautifulSoup html kodlarının çıktısını alırken kullanılmak üzere güzel methodlar barındırır. Hemen inceleyelim;

```
>>> print(source.find("span",attrs={"class":"itemText_063f9"}))
<span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">AnasayfaAdresi</span>
>>> # Şimdi prettify methodunu kullanalım.
>>> print(source.find("span",attrs={"class":"itemText_063f9"}).prettify())
<span class="font_9d927 black_476ed small_231df normal_d2e66 itemText_063f9">
 AnasayfaAdresi
</span>
>>>
```
AnasayfaAdresi olarak değiştirdiğim alanı çektim html olarak span etiketi geldi. Şimdi bu html dosyasını daha güzel bir şekilde ekrana bastırmak istediğimizde **prettify** methodunu kullanabiliyoruz. Kullandığımda html kodlarının nasıl daha okunabilir hale geldiğine dikkat edin. Kullanmadığımda yanyana yazıyordu, şimdi daha okunaklı.

## Encode Etmek
Buraya kadar hep producthunt sitesi üzerinden işlem yapmıştım. Şimdi bu örnek için elle bir html yazıp onun üzerinden anlatım yapacağım.

Eğer işlem yaparken encode değerini değiştirmek isterseniz bunun için bir method mevcut.

```
>>> kaynak = "<div><p><b>Atatürk</b> Ülkeye Çağ Atlattı.</p></div>"
>>> source = BeautifulSoup(kaynak,"lxml")
>>> print(source.prettify())
<html>
 <body>
  <div>
   <p>
    <b>
     Atatürk
    </b>
    Ülkeye Çağ Atlattı.
   </p>
  </div>
 </body>
</html>
>>> source.encode("latin-1")
b'<html><body><div><p><b>Atat\xfcrk</b> \xdclkeye \xc7a&#287; Atlatt&#305;.</p></div></body></html>'
>>> source.encode("utf-8")
b'<html><body><div><p><b>Atat\xc3\xbcrk</b> \xc3\x9clkeye \xc3\x87a\xc4\x9f Atlatt\xc4\xb1.</p></div></body></html>'
```
İlk önce **kaynak** adında bir değişken içerisine html kod yazdım. Sonra **prettify** methodu ile daha okunaklı hale getirdim. Sonra da **.encode("latin-1") ve encode("utf-8")** methodları ile karakterlerin encode değerlerini değiştirebildim.
