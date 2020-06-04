---
layout: post
published: true
title: The Silver Searcher Paketi Kullanımı
subtitle: Terminal üzerinden arama yaparken işlerinizi kolaylaştıracak bir paket. Daha kolay ve hızlı grep komutunu kullanmanızı sağlar.
permalink: /the-silver-searcher-eklentisi
image: /img/2018/ag.png
share-img: /img/2018/ag.png
date: 2018-02-10
categories:
    - "linux"
---

Terminalden hemen kurulumunu yapabileceğiniz bu paket ile bilgisayarınızda daha kolay arama yapabileceksiniz. Arama derken de bahsettiğim aslında bir yazılım projenizde bir şey aramak. Normalde grep komutu ile yapabileceğiniz işlemleri sizin için daha kolay bir hale getirmiş olan bu eklentiyi biraz inceleyelim. Bu eklentiye Yazılımcı Grep'i diyebiliriz.

## Kurulum

```
sudo apt-get install silversearcher-ag
```

komutu ile bilgisayarınıza kurabilirsiniz.


## Kullanımı
Bu eklentiyi ag komutu ile kullanacağız.

```
ag @todo
```
Mesela bu komut ile bulunduğunuz klasör ve alt klasörler içerisindeki dosyalarınız içerisinde ***@todo*** geçen satırları sizin için bulur ve ekrana basar. Mesela bu komutun çıktısının bir kısmı bende şöyle oluyor;

```
web/lib/Visitor.php
64:     * @todo bu 3 method tek methoda indirilebilir.
102:    * @todo Bu tarz super vb metodlar admina özel olmalı.
162:    * @todo Burada member entity'si için özel bir yapı kurulmalı.
```

Yani diyor ki senin web/lib/ klasörü içindeki Visitor.php de 3 tane @todo geçen satır var. Peki ben detay görmek istemiyorum kardeşim sadece hangi dosyalarda geçtiğini bana söyle yeter derseniz onun için ise;

```
ag @todo -l
```
kullanmalısınız. Bu komutun çıktısı da şöyle olacak.

```
web/lib/Visitor.php
config/Settings.json
vendor/source.xml
```
yani 3 dosyamda @todo geçiyormuş.

Ben sadece web klasöründekileri aramak istiyorum derseniz eğer

```
ag @todo web -l
```
diyebilirsiniz, veya alt klasörü varsa web/design/html gibi uzatabilirsiniz. Fakat zaten alt klasörde de arıyor, sadece web yazarak da görebilirsiniz o klasör ve altındaki tüm klasörleri arar.

Şimdi de şunu dediniz, ya iyi güzel de ben json ve xml uzantıları istemiyorum bana içinde @todo geçen PHP dosyaları gerekiyor. Tamam, o da kolay.

```
ag --php @todo -l
```
Bu da bana sadece  @todo geçen php dosyalarını listeleyecek.

```
web/lib/Visitor.php
```
şeklinde. Ama ben PHP yazmıyorum ki? Ben python veya yml yazıyorum derseniz de

```
ag --list-file-types
```
komutu ile --php yerine kullanabileceğiniz desteklenen uzantıları görebilirsiniz. Aynı zamanda bu komut ile birlikte REGEX de yazabilirsiniz. Mesela float:left veya float:right geçen dosyaları aramak istedim.

```
ag float:\(left\|right\)
```
Normalde float:(left|right) şeklinde yazılır programlama dillerinde ancak terminal kullandığımız için ve karakterlerin farklı anlamları olduğu için başlarına kaçış karakteri ekledim. Bunun çıktısı da bende şöyle oldu.

```
design/storefront/pollResult.twig
26:        <div style="float:left;">
29:        <div style="float:right;">
```
Yani ilgili dosyada 26. ve 29. satırlarda geçiyormuş.

Eğer kodlarınızı github gibi bir yerde saklıyorsanız büyük ihtimal .gitignore dosyanız da vardır. Yani github, bu dosya içindeki klasörlerdeki değişiklikler ile ilgilenmez onları görmez. İşte bu ag komutunu kullandığınızda eğer siz bir klasörü .gitignore dosyanıza yazmışsanız, işte aradığınız şey o klasörde olsa bile size göstermez. Yani sizin .gitignore dosyanızı da göz önünde bulundurur. Süper değil mi?

Aynı şeyi .gitignore yerine elle yapmak isterseniz de

```
ag @todo --ignore-dir web -l
```
şeklinde kullanabilirsiniz. Bu da web klasörü dışında içinde @todo geçen dosyaları size listeler. Eğer projenizde symlinks varsa, -f parametresini komuta ekleyerek takip etmesini sağlayabilirsiniz.

Eğer kaç tane sonuç bulmuş, kaç dosya aranmış gibi detaylı bilgi isterseniz de --stats komutunu kullanmalısınız.

```
ag --py kivy --stats -l
```
Bu komut ile python dosyalarım içinde kivy geçenleri listeledim.

```
kivytut7.py
kivytut6.py
calculator/calc.py
kivytut2.py
kivytut4.py
kivytut1.py
kivytut8.py
kivytut5.py
kivytut3.py
45 matches
9 files contained matches
9 files searched
4213 bytes searched
0.001473 seconds
```
şeklinde bir sonuç döndü. Dikkat ettiyseniz kaç tane arama sonucu bulmuş, kaç dosyada aranmış kaç saniye sürmüş gibi detaylı bilgileri de bana verdi.
