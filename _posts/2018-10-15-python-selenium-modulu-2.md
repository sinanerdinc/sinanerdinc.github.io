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

Bu konu litaratürde **selectors** adı ile anılır. Eğer bu konuda farklı araştırmalar yapmak isterseniz, arama motorlarında **selenium webdriver selectors** şeklinde veya benzer aramalar yapabilirsiniz.

## Belirli bir alanın HTML kodlarına ulaşmak

Yapmak istediğimizi kafamızda canlandırmak için önce biraz özetleyelim. Bir önceki ders içeriğinde öğrendiğimiz gibi selenium ile bir tarayıcı açtık, bir siteye girdik, şimdi bu sitede belirli bir alanlara tıklama, inputların veya text elemanlarının içerisini doldurma işlevi yapacağız. Bir siteye girdikten sonra kullanıcı adı ve şifre girip sonra giriş yap butonuna tıklamak gibi.

Bu tür işlemler için ilerleyeceğimiz yol şu, ilk önce nerde ne işlem yapacak ise oranın html elemanını seçmemiz gerek, ardından seleniuma bu seçtiğim yerde şu şu işlemleri yap diye yönlendirme yapabiliriz. Bunun için de ilgili alanın html elemanlarına ulaşmak gerekiyor, neyse ki bu çok kolay bir işlem.

![inspector kullanımı](https://www.sinanerdinc.com/img/2018/inspector.png)

Google Chrome üzerinden anlatalım, websitesine girdiğinizde mouse üzerinden sağ tıklayın, en alttaki inspect menüsünü seçin, sonra yukarıdaki ekran görüntüsündeki gibi 1 ile işaretlediğim butona basın, sonra da hangi html elemanının kodlarını görmek istiyorsanız mouse onun üzerine gelsin. Direkt olarak ilgili alanın kodlarına ulaşabilirsiniz.

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
Bu şekilde liste id değerinen başlayıp label değerine kadar inebiliyorum. 2 tane label olduğu için **nth-child(1)** ve **nth-child(2)** ile ilk veya ikinci label değerine geçebiliyorum.

Bu methodu güçlü kılan diğer özelliklerden bahsedelim.

```
find_element_by_css_selector("label[for='isim']")
```

Bu seçici ile bir **label** etiketi arıyoruz, ancak **for** diye bir özniteliği (attribute) olmalı ve bu for niteliği de **isim** diye bir değere eşit olmalı.

```
find_elements_by_css_selector("span[id^='category']")
```
Bu seçici ile bir **span** etiketi arıyoruz, ancak bu span etiketi **id** değerine sahip olmalı ve bu id değeri de **category** ile **başlamalı.** Yani **id="categorySelector"** olan bir değeri yakalayabilirsiniz.

```
find_elements_by_css_selector("li[class$='Menu']")
```
Bu seçici ile bir **li** etiketi arıyoruz, ancak bu li etiketi **class** değerine sahip olmalı ve bu class değeri de **menu** ile **bitmeli.** Yani **class="categoryMenu"** olan bir değeri bu seçici ile yakalayabilirsiniz.


```
find_element_by_css_selector("div[class*='buton']")
```
Bu seçici ile bir **div** etiketi arıyoruz, ancak bu div etiketi **class** değerine sahip olmalı ve bu class değeri içerisinde **buton** değeri **geçmeli.** Yani **class="SubmitButon1"** olan bir değeri bu seçici ile yakalayabilirsiniz.

{: .box-note}
Bir sonraki dersimiz, sayfada seçtiğimiz html elemanları ile aksiyon almak üzerine olacak. Tıklamak, içerisine birşey yazmak v.s.
