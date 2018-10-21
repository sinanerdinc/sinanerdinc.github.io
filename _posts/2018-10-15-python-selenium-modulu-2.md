---
layout: post
published: false
title: Python Selenium Modülü Kullanımı Ders 2
subtitle: Sayfadaki HTML elemanlarını seçebileceğimiz methodları inceliyoruz. (Find Element)
permalink: /python-selenium-modulu-kullanimi-2
image: /img/2018/selenium-ders1.png
share-img: /img/2018/selenium-ders1.png
date: 2018-10-15
categories:
    - "python"
---
Bir önceki [Selenium Modülü Ders 1](https://www.sinanerdinc.com/python-selenium-modulu-kullanimi-1) yazımda chrome webdriver üzerinden python selenium modülünü kullanmaya başlamıştık. Şimdi de açtığımız sayfa içerisindeki HTML elemanlarını seçebileceğimiz methodları inceleyeceğiz.

Bu konu literatürde **Locating Elements** adı ile anılır. Eğer bu konuda farklı araştırmalar yapmak isterseniz, arama motorlarında **Selenium Webdriver Locating Elements** şeklinde veya benzer aramalar yapabilirsiniz.

## Belirli bir alanın HTML kodlarına ulaşmak

Yapmak istediğimizi kafamızda canlandırmak için önce biraz özetleyelim. Bir önceki ders içeriğinde öğrendiğimiz gibi selenium ile bir tarayıcı açtık, bir siteye girdik, şimdi bu sitede belirli bir alanlara tıklama, inputların veya text elemanlarının içerisini doldurma işlevi yapacağız. Bir siteye girdikten sonra kullanıcı adı ve şifre girip sonra giriş yap butonuna tıklamak gibi.

Bu tür işlemler için ilerleyeceğimiz yol şu, ilk önce nerde ne işlem yapacak ise oranın html elemanını seçmemiz gerek, ardından python kodları ile bu seçtiğim yerde şu şu işlemleri yap diye yönlendirme yapabiliriz.Tabii ki selenium kullanarak. Bunun için de ilgili alanın html elemanlarına ulaşmak gerekiyor, neyse ki bu çok kolay bir işlem.

![inspector kullanımı](https://www.sinanerdinc.com/img/2018/inspector.png)

Google Chrome üzerinden anlatalım, websitesine girdiğinizde mouse üzerinden sağ tıklayın, en alttaki inspect menüsünü seçin, sonra yukarıdaki ekran görüntüsündeki gibi 1 ile işaretlediğim butona basın, sonra da hangi html elemanının kodlarını görmek istiyorsanız mouse onun üzerine gelsin. Direkt olarak ilgili alanın kodlarına ulaşabilirsiniz.

Artık üzerinde işlem yapmak istediğim alanın HTML kodlarına ve dolayısı ile seçicilerine ulaşabilirim. Bu seçicilere ulaştıktan sonra geriye yapabileceğimiz işlemlerden bahsetmek kalıyor.

# Kullanabileceğimiz methodlar

### 1- find_element_by_id()

```
<div id="mousehover">Sinan</div>
```

Bu şekilde bir html elemanı olsun, yani bir html elemanı **id** özniteliğine sahip olsun, bu değeri seçebilmek için **find_element_by_id()** methodunu kullanacağız.

```
driver = webdriver.Chrome()
driver.find_element_by_id("mousehover")
```

Bu şekilde id değeri **mousehover** olan alanı seçmiş olduk. Seçtik ama herhangi birşey olmadı. Ekrana birşey yazmadım, tıklamadım v.s. Merak etmeyin seçtikten sonra yapabileceğimiz işlemlerden de bahsedeceğim. Şuan sadece seçme işlemini yapıyoruz.


### 2- find_element_by_class_name()

```
<div class="mousehover">Sinan</div>
```

Bu şekilde bir html elemanı olsun, yani bir html elemanı **class** özniteliğine sahip olsun, bu değeri seçebilmek için **find_element_by_class_name()** methodunu kullanacağız.

```
driver.find_element_by_class_name("mousehover")
```
Bu şekilde class değeri **mousehover** olan alanı seçmiş olduk.

### 3- find_element_by_css_selector()

Bazen bir html elemanını direkt olarak id veya css ile seçemeyebilirsiniz. Çünkü böyle bir öznitelik atanmamış olabilir. O zaman css selector kullanarak o elemana ulaşabilirsiniz. Örneklerle inceleyelim.

```
<div id="liste">
  <fieldset>
    <label>Sinan</label>
    <label>Erdinç</label>
  </fieldset>
</div>
```
Şu şekilde bir html elemanı olsun ve ben de **label** içerisindeki değerlere ulaşmak istiyorum. Burdaki label değerlerinin id veya css özniteliği yok, ama üstündeki div değerinin var, burdan alta label değerlerini seçebiliriz.

```
driver.find_element_by_css_selector("#liste > fieldset > label:nth-child(1)")
driver.find_element_by_css_selector("#liste > fieldset > label:nth-child(2)")
```
Bu şekilde **liste** id değerinen başlayıp **label** değerine kadar inebiliyorum. 2 tane label olduğu için **nth-child(1)** ve **nth-child(2)** ile ilk veya ikinci label değerine geçebiliyorum.

Bu methodu güçlü kılan diğer özelliklerden bahsedelim.

```
find_element_by_css_selector("label[data='isim']")
```

Bu seçici ile bir **label** etiketi arıyoruz, ancak **data** diye bir özniteliği (attribute) olmalı ve bu for niteliği de **isim** diye bir değere eşit olmalı.

```
find_elements_by_css_selector("span[id^='category']")
```
Bu seçici ile bir **span** etiketi arıyoruz, ancak bu span etiketi **id** değerine sahip olmalı ve bu id değeri de **category** ile **başlamalı.** Yani **id="categorySelector"** olan bir değeri yakalayabilirsiniz.

```
find_elements_by_css_selector("li[class$='Menu']")
```
Bu seçici ile bir **li** etiketi arıyoruz, ancak bu li etiketi **class** değerine sahip olmalı ve bu class değeri de **menu** ile **bitmeli.** Yani **class="categoryMenu"** olan bir değeri bu seçici ile yakalayabilirsiniz.


```
find_element_by_css_selector("div[class*='Buton']")
```
Bu seçici ile bir **div** etiketi arıyoruz, ancak bu div etiketi **class** değerine sahip olmalı ve bu class değeri içerisinde **Buton** değeri **geçmeli.** Yani **class="SubmitButon1"** olan bir değeri bu seçici ile yakalayabilirsiniz.

### 4- find_element_by_name()

```
<span name="description"> Selenium modülü öğreniyorum.</span>
```
Bu şekilde bir html elemanı olsun, yani bir html elemanı **name** özniteliğine sahip olsun, bu değeri seçebilmek için **find_element_by_name()** methodunu kullanacağız.

```
driver.find_element_by_name("description")
```
Bu şekilde name değeri **description** olan alanı seçmiş olduk.

### 5- find_elements_by_tag_name()

```
<footer> Tüm hakları saklıdır. sinanerdinc.com </footer>
```
Bu şekilde bir html elemanı olsun, yani bir html elemanı **footer** etiketine sahip olsun, bu değeri seçebilmek için **find_elements_by_tag_name()** methodunu kullanacağız.

```
driver.find_elements_by_tag_name("footer")
```
Bu şekilde etiket değeri **footer** olan alanı seçmiş olduk.

### 6- find_element_by_link_text()

```
<a href="/">Anasayfa</a>
```
Girdiğiniz websitesinde onlarca bağlantı (link) bulunur. Sadece istediğiniz metne sahip bir bağlantıyı seçebilmek için **find_elements_by_tag_name()** methodunu kullanacağız.

```
driver.find_element_by_link_text("Anasayfa")
```
Bu şekilde metni **Anasayfa** olan bağlantı adresini seçmiş olduk.


### 7- find_element_by_xpath()

Eğer xpath konusunda bir uzmanlığınız yoksa, ve hiçbir şekilde o HTML elemanına diğer methodlar ile ulaşamazsanız ancak o zaman bunu kullanın. Çünkü xpath eüer en basit yazılımı ile kullanılırsa çok kırılgandır. Xpath en basit kullanımı ile en baştaki HTML elemanından, aramak istediğiniz yere kadar olan tüm yolları seçer.

```
/html/body/div[1]/div[2]/div/ul/li[4]/ul/li[5]/a
```

Mesela bu bir xpath örneğidir. Sayfanın en başındaki html etiketinden, aradığımız a etiketine kadar inen tüm yolları gösterir. Çok kullanmamanız gereken, kırılgan olan alan da burası, sayfaya bir bildirim eklediniz, bir slider eklediniz, bir html elemanı eklediğiniz, artık yukarıdaki xpath bozulur, bu xpath yoluna bir html elemanı daha gelir. Bu nedenle, en son başvurmanız gereken bir seçicidir. Fakat bazı güzel özellikleri ile aslında xpath sürekli kullandığınız bir method da olabilir.

```
driver.find_element_by_xpath("/html/body/div[1]/div[2]/div/ul/li[4]/ul/li[2]/a")
```
Bu şekilde en basit yöntem ile sayfada html elemanından itibaren tüm etiket yolu **/html/body/div[1]/div[2]/div/ul/li[4]/ul/li[2]/a** olan bağlantı adresini seçmiş olduk.

### 7.1 Xpath İleri Seviye Kullanımı

Şimdi biraz da xpath için güçlü yönlerinden bahsedelim.

```
<button data="submit">Onayla</button>
```

Mesela şöyle bir html kodu var ve biz burda button değerini seçmek istiyoruz.

```
driver.find_element_by_xpath("//button[@data='submit']")
```

şeklinde bir kod yazarak seçebiliriz.

```
<li><a href="python">Python</a></li>
<li><a href="/">Anasayfa</a></li>
<li><a href="hakkimda">Hakkımda</a></li>
```
şöyle bir html kod içerisinden sadece Python metnine sahip **<li>** değerini seçmek istiyoruz.

```
driver.find_element_by_xpath("//li/a[contains(text(),'Python')]")
```

şeklinde seçebiliriz. Başlangıç olarak **//li/a** kullandık, ama istersek tüm sayfa içerisinde de arayabilirdik, başlangıç olarak bir yol vermeyebilirdik. Onu da şöyle yapardık.

```
driver.find_element_by_xpath("//*[contains(text(),'Python')]")
```

Aslında xpath, yukarda anlattığım diğer methodları da kapsayan, hepsinin yerine kullanabileceğiniz bir özellik.

Mesela xpath ile şöyle şeyler yapabilirdik.

```
driver.find_element_by_xpath("//*[@class='container']")
driver.find_element_by_xpath('//*[@id="section1"]/div[1]/div[1]')
driver.find_element_by_xpath("//a[contains(text(),'Anasayfa')]")
```
Yani class veya id değeri üzerinden bir arama yapabilir veya bağlantı metni Anasayfa olan linki bul diyebilirdik. Eğer xpath kullanacaksanız bu şekilde kullanın, direkt olarak en baştan ilgili html elemanına direkt giden bir yol kullanmayın.


{: .box-note}
Bir sonraki dersimiz, sayfada seçtiğimiz html elemanları ile aksiyon almak üzerine olacak. Tıklamak, içerisine birşey yazmak v.s.
