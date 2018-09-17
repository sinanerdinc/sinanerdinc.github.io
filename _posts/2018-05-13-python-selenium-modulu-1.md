---
layout: post
published: true
title: Python Selenium Modülü Kullanımı Ders 1
subtitle: Python üzerine selenium ve webdriver kurulumunu yaparak, kullanışlı bazı methodları inceliyoruz.
permalink: /python-selenium-modulu-kullanimi-1
image: /img/2018/selenium-ders1.png
share-img: /img/2018/selenium-ders1.png
date: 2018-09-14
categories:
    - "python"
---
Selenium, bilgisayarınıza yükleyeceğiniz bir driver yardımı ile ekrana chrome, firefox gibi bir tarayıcı açarak, gerçek bir insan gibi istediğiniz tüm işlemleri programlama dili yardımıyla çalıştırmanızı sağlayan bir araçtır. Siz bir tarayıcı açtığınızda bir web sitesine bağlanıp belirli alanlara tıklayarak geziniyor, bazı formları dolduruyor veya butonlara tıkladığınızda bazı yerlerin ekrana gelmesini bekliyorsunuz. İşte tüm bu aksiyonları, sizin yerinize python kodları yardımı ile yapan bir ürün hayal edin, bu modülü öğrendiğinizde düzenli yaptığınız işler için otomasyonlar yazabilir veya bir bot yazıyorsanız javascript ile ilgili problemleriniz varsa, bunlara çare bulabilirsiniz.

## Kurulum
Ben pip3 ile kurmuştum, pip3 python3 için paket yöneticisi. Eğer size pip3 modülü yüklü değil ise,

```
sudo apt-get install python3-pip
```
komutu ile pip3 kullanmaya başlayabiliriz. Eğer pip3 yüklüyse buna gerek yok, şimdi bu modülü kuralım.

```
pip3 install selenium
```
Artık modülü kurduk, projemiz içerisine

```
from  selenium import webdriver
```
diyerek aktarabiliriz. Herhangi bir hata almadıysanız, son olarak yapmamız gereken şu, selenium için chrome veya firefox driverları indirmeliyiz.

## Driver İndirme

- Chrome:  [https://chromedriver.storage.googleapis.com/index.html?path=2.35/](https://chromedriver.storage.googleapis.com/index.html?path=2.35/)
- Firefox: [https://github.com/mozilla/geckodriver/releases](https://github.com/mozilla/geckodriver/releases)

adreslerinden ilgili driverları indirin, zipi açın ve bilgisayarınızdaki bir klasör içerisine çıkartın. Bu klasörün yolunu birazdan kullanacağız o yüzden önemli.

Ben **/home/sinan/Downloads/selenium/chromedriver** yoluna çıkarttım. Şimdi kodlarımızı yazalım.

```
from selenium import webdriver

driver_path = "/home/sinan/Downloads/selenium/chromedriver"
browser = webdriver.Chrome(executable_path=driver_path)
```
Dikkat ettiyseniz **executable_path** alanına ben chrome driver için indirdiğim dosyanın yolunu tuttuğum **driver_path** değişkenini yazdım. Selenium artık benim hangi driver üzerinden çalışacağımı anlamış oldu. Bunu yazdığınızda herşey başarılı ise bir chrome tarayıcısı açılmış olmalı.

{: .box-note}
Burda dikkat etmeniz gereken şu, bu klasör yolu linux veya windows için değişiyor. Linux için **/home/sinan/Downloads/selenium/chromedriver** gibi bir şey yazdım ama windows kullanıyorsanız **C:\\users\\sinan\\downloads\\chromedriver.exe** gibi birşey olacaktır.

Artık driver objesi içerisinde selenium içindeki methodları kullanabiliriz. Şimdi biraz bu methodları inceleyelim.


# Kullanışlı methodlar

### 1- get, title, back, refresh, close, quit
Bir websitesini açmak istediğinizde get methodu kullanılır.

```
driver.get("https://www.sinanerdinc.com/")
```

Bir websitesine girdikten sonra, o sitenin meta title yani başlık etiketi içerisindeki metne ulaşmak isterseniz title methodu kullanılır.

```
>>> driver.title
'Sinan Erdinç - İşte, böyle düşüncelerim var.'
```

Bir websitesini açtıktan sonra, o sitede farklı sayfalara giriş yaptığınızı varsayalım, bir önceki sayfaya geri dönmek isterseniz back methodunu kullanmalısınız.

```
driver.back()
```
Açtığınız bir sayfayı yenilemek isterseniz refresh methodunu kullanabilirsiniz.

```
driver.refresh()
```

Bir websitesini açtıktan sonra, webdriver ile açılan tarayıcıda yeni bir sekme de açtığınızı düşünelim. Sadece şuan çalıştığınız sekmeyi kapatmak, ama tüm tarayıcıyı kapatmamak isterseniz o zaman close methodunu kullanmalısınız.

Eğer kaç tane sekme var olursa olsun, tamamen komple tarayıcıyı kapatmak isterseniz o zaman quit methodunu kullanmalısınız.

```
driver.close()  # Sadece sekmeyi kapatır.
driver.quit()  # Tüm tarayıcıyı kapatır.
```

### 2- maximize_window, set_window_size, save_screenshot, page_source

Açtığınız sayfayı tam ekran yapmak isterseniz maximize_window methodunu kullanabilirsiniz.

```
driver.maximize_window()
```

Eğer açtığınız sayfanın boyutlarını kendiniz belirlemek isterseniz (mesela 800 x 600) o zaman set_window_size methodunu kullanabilirsiniz.

```
driver.set_window_size(800,600)
```

Açtığınız sayfanın ekran görüntüsünü almak isterseniz o zaman kaydedilecek tam yol ve dosya ismi ile birlikte save_screenshot methodunu kullanmalısınız.

```
>>> driver.save_screenshot("/home/sinan/test.png")
True
```

Gördüğünüz gibi bu method başarılı bir şekilde ekran görüntüsü aldığında True değerini geri döner.

Açtığınız sayfanın kaynak koduna ulaşmak için ise page_source methodunu kullanmalısınız. Sayfa kaynağını aldıktan sonra bunu beautifulsoup gibi bir modüle aktarıp sonrasında istediğiniz yerleri keserek kendinize bir bot yazabilirsiniz.

```
driver.page_source
```

{: .box-note}
Bir sonraki dersimiz sayfadaki belirli HTML elemanlarını seçebilmek için gerekli olan methodlar ile ilgili olacak.
