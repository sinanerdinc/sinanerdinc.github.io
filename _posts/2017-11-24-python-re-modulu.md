---
layout: post
published: false
title: Python Re Modülü
subtitle: Python'da Düzenli İfade (Regular Expression) Kullanımı
permalink: /python-re-modulu
image: /img/2017/python-os.png
share-img: /img/2017/python-os.png
tags: [python, development]
date: 2018-01-01
categories:
    - "python"
---
Düzenli ifadeler (Regular Expressions) yazılım hayatınızda mutlaka karşınıza çıkacak bir terimdir. Çok sık çıkmaz ama kesinlikle çıkar. Anlaması ilk önce biraz zor gelebilir, bazı temel yapıları uygulamalı olarak anlatmaya çalışalım. Zaten ben de bu içeriği, ihtiyacım olduğunda tekrar geri dönüp, konuyu hatırlayabilmek için yazıyorum.

Düzenli ifadeler tüm modern dillerde bulunur, dağınık bir metin içerisinde istediğimiz formattaki metinleri yakalayabilmemize imkan tanır. Mesela, bir kaynakta geçen tüm e-posta adreslerini veya içinde rakam bulunan ve gmail uzantılı olan mail adreslerini ayıklamak için kullanabilirsiniz. Düzenli ifadeler olmasaydı ardı arkasına birçok if - else yazmak gerekebilirdi. Bu modül, birkaç saatte yapabileceğiniz bir işlemi saniyeler içerisinde sizin yerinize yapabiliyor. Uzmanlaşması biraz vakit alsa da unutması en hızlı modüllerden biri bence.

## Kurulum
Python ile standart olarak gelen bir kütüphanedir. Kurulum gerektirmez.

```
import re
```

şeklinde projeniz içerisine aktardıktan sonra kullanmaya başlayabilirsiniz.

## Standart Metin
Tüm düzenli ifade işlemlerim için aşağıda standart bir metin hazırladım. Bu metin üzerinde bol bol farklı örnekler ile birlikte bu modülü inceleyelim.

```
metin = """
Merhaba arkadaşlar ben Sinan, bu eğitim içerisinde kullanacağımız metin aşağıdaki gibidir.

Örnek;
abcdefghijklmnopqrstuvwxyz
ABCDEFGHIJKLMNOPQRSTUVWXYZ
0123456789
test@mail.org
test@gmail.com
https://www.google.com
http://www.google.net
http://www.sinanerdinc.com
https://sinanerdinc.com
www.sinanerdinc.com
+9 0555 222 33 44
"""
```

# 1. search
Bu method, aranılan bir içeriğin ilgili metin içerisinde olup olmadığını kontrol eder.

```
# metin değişkenini yukarıda tanımlamıştık.
>>> kontrol = re.search("Sinan",metin)
>>> kontrol
<_sre.SRE_Match object; span=(24, 29), match='Sinan'>
```

Oluşturduğum metin değişkenindeki yazı içinde Sinan geçiyormu diye sorgulayıp gelen objeyi kontrol değişkenine atadım. Sonra kontrol değişkenine baktım, dikkat ederseniz span ve match diye alanlar var. Burada match aradığınız değeri, span ise nerede olduğunu gösterir. Aradığınız yerdeki 24. ve 29. harfler arasındaymış o zaman kontrol edelim.

```
>>> metin[24:29]
'Sinan'
```

Gördüğünüz gibi tekrar Sinan değeri döndü. Search methodu ilgili metnin nerede olduğunu başarıyla bize söyleyebildi.

## 1.1 start()

Üstteki örnek içerisinde kontrol değişkenine Sinan kelimesini aratıp sonucunu atamıştık, bu değişken üzerinde kullanabileceğimiz methodlarda biri start() methodu. Bu, aratılan kelimenin, kaynakta nerede geçtiğini döndürür. Biz zaten yukarıdaki örnekte (24,29) arasında olduğunu biliyoruz. Sadece burdan 24 değerini çekmek istersek kullanabiliriz.

```
>>> kontrol.start()
24
```

## 1.2 end()

Üstteki start methodunun tersini yapar, yani aratılan kelime hangi aralıkta geçiyorsa onun son değerini döndürür. Biz kaynakta Sinan kelimesinin (24,29) arasında geçtiğini biliyoruz. Start methodu 24 dönmüştü, end methodu da 29 değerini dönecektir.

```
>>> kontrol.end()
29
```

## 1.3 endpos()
Kaynak içerisindeki karakterlerin toplam sayısını döndürür. \n gibi satır atlatma karakterleri de sayılır.

```
>>> kontrol.endpos
328
>>> metin[:328]
'\nMerhaba arkadaşlar ben Sinan, bu eğitim içerisinde kullanacağımız metin aşağıdaki gibidir.\n\nÖrnek;\nabcdefghijklmnopqrstuvwxyz\nABCDEFGHIJKLMNOPQRSTUVWXYZ\n0123456789\ntest@mail.org\ntest@gmail.com\nhttps://www.google.com\nhttp://www.google.net\nhttp://www.sinanerdinc.com\nhttps://sinanerdinc.com\nwww.sinanerdinc.com\n+9 0555 222 33 44\n'
```
İlk önce endpos ile 328 karakter olduğunu gördüm, sonra metin değişkenini 328'e kadar görüntüledim ve doğruluğunu teyid ettim.





## YUKARISINI DÜZELT METİNLER DEĞİŞECEK







# 2. findall
Bu method ile bir kaynak içerisinde istediğimiz metnin kaç kere geçtiğini inceleyebiliriz.

```
>>> print(re.findall("zaman","Tam zamanında geldin, sensiz zaman geçmiyor"))
['zaman', 'zaman']
```
Oluşturduğum metin içerisinde **zaman** kelimesi var mı diye sorguladım. Liste olarak geri döndü. Eğer bunu adet olarak görmek isteseydim len() methodundan geçirebilirdim.

```
>>> print(len(re.findall("zaman","Tam zamanında geldin, sensiz zaman geçmiyor")))
2
```
Bu şekilde 2 kere geçtiğini de görmüş oldum.

# 2. Metakarakterler
Bu noktaya kadar bir kaynak içerisinde belirli bir metin aradık, ancak bu modülü güçlü yapan şey bu noktadan sonra anlatacağımız metakarakterlerdir. Bu metakarakterler sayesinde, belirlediğiniz bir düzene uyan metinleri arayabilirsiniz. Mesela sinan metnini aramak yerine, sinan, sunan, sühan, solan gibi belirli bir şablon içerisine giren metinleri de arayabilirsiniz. İşte bunu yapmamızı sağlayan, aranılan metne eklediğimiz bazı özel karakterler var, bu karakterleri inceleyelim.


## . Karakteri
Yeni satır karakteri hariç herhangi tek bir karakterin yerini tutar.

```
>>> re.findall(".aman","O her zaman, saman altından su yürütürdü.")
['zaman', 'saman']
```
Mesela bu örnek içerisinde zaman ve saman kelimelerini buldu. Çünkü herhangi bir karakter ile başlayıp aman ile devam eden bir şablon belirledik.

## *  Karakteri
Bir ifadenin bütün tekrarlanmalarını bulur.

```
>>> re.findall("@g*mail","E-posta adresimiz test@mail.com, test@gmail.com ve test@ggggggmail.com")
['@mail', '@gmail', '@ggggggmail']
```
g harfinden sonra * koyduk, yani bu g karakteri 0 kere de geçebilir 100 kere de geçebilir. Bu ayrım sadece kendinden önceki harfi kapsıyor. Bu nedenle @mail, @gmail ve @ggggggmail döndü.

## +  Karakteri
Kendinden önce gelen karakterin bir veya daha fazla kullanılmasını arar. Üstte yaptığımız * ile ilgili örneği bu sefer + ile yaparsak;

```
>>> re.findall("@g+mail","E-posta adresimiz test@mail.com, test@gmail.com ve test@ggggggmail.com")
['@gmail', '@ggggggmail']

```
şeklinde @gmail ve @ggggggmail döner. Çünkü g harfinden sonra + koyarak bu harfin 1 veya daha fazla geçmesi gerektiğini söyledik.

## ?  Karakteri
Kendinden önce gelen karakterin 0 veya 1 kere tekrar etmesini sorgular.

```
>>> re.findall("@g?mail","E-posta adresimiz test@mail.com, test@gmail.com ve test@ggggggmail.com")
['@mail', '@gmail']
```
Dikkat ettiyseniz @ggggggmail dönmedi çünkü 0 veya 1 kere olmasını sorguladık, 1'den fazla olanlar bu sonuca dahil değil.

## [ ] Karakteri
Bu köşeli parantezler arasına yazılan bütün karakterler sorgulanır.

```
>>> re.findall("[HB]ilal","Hilal ve Bilal çok iyi arkadaştır.")
['Hilal', 'Bilal']
```
Bu şablonda H veya B ile başlayan sonrasında ilal ile devam eden bir şablon belirledik ve Hilal, Bilal sonuçlarını aldık.

```
>>> re.findall("s[ai]m[ei]t","Kardeşim samet, markete simit almaya gitti.")
['samet', 'simit']
```
Burda ise s ile başlayan a veya i artı m ile devam eden, e veya i artı t ile biten sonuçları aradık. Sonrasında samet ve simit sonuçlarını başarıyla gördük.

Biraz daha işleri karıştıralım. Köşeli parantez kullanırken aralık da belirleme imkanınız var.

* [A-Z] A'dan Z'ye tüm BÜYÜK harfler.
* [a-z] A'dan Z'ye tüm küçük harfler.
* [0-9] 0'dan 9'a tüm rakamlar.
* [1-4] 1'den 4'e tüm rakamlar.

O zaman madem Hilal ve Bilal büyük harf ile başlıyor, şöyle yazalım.

```
>>> re.findall("[A-Z]ilal","Hilal ve Bilal çok iyi arkadaştır.")
['Hilal', 'Bilal']
```
A-Z arasındaki büyük bir harfle başlayan ve sonra **ilal** ile devam edenleri bulduk. Peki küçük harfle arasaydık ne olurdu?

```
>>> re.findall("[a-z]ilal","Hilal ve Bilal çok iyi arkadaştır.")
[]
```
Gördüğünüz gibi hiçbir sonuç dönmedi. Çünkü Hilal ve Bilal büyük harfler ile başlıyor.

## { } Karakterleri

Belirli bir sayıda tekrar anlamındadır. Şimdi yukarıda öğrendiklerimiz ile birlikte biraz daha karışık bir örnek yapalım. Kaynağımız **Bence saat tamir etmek zor zanaat.** olsun. Burda küçük harflerle başlayan aat ile biten tüm herşeyi yakalamak istedik.

* Adım 1: **[a-z]** ile küçük harf ayrımı yapmamız gerek.
* Adım 2: **[a-z]*** ile küçük harflerden 1 veya n (sonsuz) kere geçmesini söyledik.
* Adım 3: **[a-z]*a** ile küçük harf ile başlayıp a ile devam etsin dedik.
* Adım 4: **[a-z]a{2}** ile a harfinden 2 adet olması gerektiğini belirttik.
* Adım 5: **[a-z]a{2}t** en son da t ile bitsin dedik.

```
>>> re.findall("[a-z]*a{2}t","Bence saat tamir etmek zor zanaat.")
['saat', 'zanaat']
```

Bingo!

## ^ Karakteri
İfadenin başlangıcını kontrol eder ve [ ] karakterleri ile birlikte kullanılırsa da **hariç** anlamına gelir.

```
>>> re.findall("^Oku","Oku oğul, sesli oku.")
['Oku']
>>> re.findall("^sesli","Oku oğul, sesli oku.")
[]
```
Kaynağımız Oku ile başladığı için ilk örnek başarıyla ^Oku sayesinde çalıştı. Ancak diğer örnekte ^sesli ayracı birşey dönmedi, halbuki kaynak içerisinde bu metin var fakat başında değil.

Bu meta karakter sabit bir metin için kullanışlı gelmeyebilir. Çünkü zaten metnin başını gözle de görebiliyorsunuz, fakat bir listeyi for döngüsüne sokarak böyle bir metakarakter kullanılırsa çok faydalı olabilir. Bir örnek yapalım.

```
metin = "Mustafa başkomiser ve yardımcısı Kemalettin, 34XY6699 plakalı arabanın peşinde."
liste = metin.split()
for i in liste:
  sonuc = re.findall("^[A-Z]+[a-z]+",i)
  if sonuc:
    print(sonuc)

['Mustafa']
['Kemalettin']
```

Örnek içerisinde metin değişkenindeki içeriği split() methodu ile kelime kelime parçalayıp liste içerisine attık. Sonra da bir for döngüsü ile her bir kelimeyi kontrol ederek büyük harfli kelimeleri ayırdık.

Şimdi de plakayı biraz uzun bir yol kullanarak bulalım.

```
for i in liste:
  sonuc = re.findall("[^A-Za-z-][0-9]+[A-Z]+[0-9]+",i)
  if sonuc:
    print(sonuc)

['34XY6699']
```

[ ] arasındaki A-Za-z ifadesi büyük veya küçük harf anlamını taşıyor. Başına da ^ eklediğimizde büyük veya küçük harf ile **başlamasın** demiş oluyoruz. Sonra 0-9 ile devam etsin sonra tekrar büyük harf ve sonra tekrar 0-9 ile devam etsin. Sonuç olarak plakayı çektik.

Peki plakayı daha kısa bir yoldan bulmak isteseydik nasıl bulurduk? Bu da ödev olsun yorum olarak yazarsınız.


## $ Karakteri
Yukarıda ifadenin başlangıcını kontrol etmiştik, $ metakarakteri ile de ifadenin sonunu yani ne ile bittiğini kontrol edebiliyoruz.

```
metin = "Silikon vadisi, google.com ve apple.com arasındaki rekabeti tartışıyor."
liste = metin.split()
for i in liste:
  sonuc = re.search(".com$",i)
  if sonuc:
    print(sonuc.string)

google.com
apple.com
```
Örnek bir metin oluşturup içerisinde google.com ve apple.com alan adlarını yerleştirdim. Ardından bu metni split() methodu ile bir liste içerisine aldım. Sonra bir for döngüsü ile her bir kelimenin .com ile bitip bitmediğini, eğer bitiyorsa ekrana yazmasını istedim. Bu şekilde bir metinde geçen .com ile biten kelimeleri çıkarabildim.

Daha anlatmadım, aşağıda anlatacağım ama yeri gelmişken yazayım istedim. Eğer burdaki .com veya .net ile bitenleri bulmak istersek aradaki **veya** ifadesini karşılamak için \| karakterini kullanıyoruz. Örnek;

```
metin = "Silikon vadisi, google.com ve apple.net arasındaki rekabeti tartışıyor."
liste = metin.split()
for i in liste:
  sonuc = re.search("(.com|.net)$",i)
  if sonuc:
    print(sonuc.string)
```
Bu kod parçası da metin içerisinde geçen .com veya .net geçenleri ekrana bastırır.

## \| Karakteri
Yukarıda verdiğim örnek içerisinde kullanmıştım, veya anlamına gelir.

```
>>> re.findall("(siyah|beyaz)","Beşiktaş, siyah ve beyaz renkleri ile anılır.")
['siyah', 'beyaz']
```
Kaynaktan siyah veya beyaz metinlerini aldık.
