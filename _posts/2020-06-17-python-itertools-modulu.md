---
layout: post
published: true
title: Python Itertools Modülü
subtitle: Tekrarlanan, yinelenen veriler için hızlı, belleği verimli kullanan fonksiyonlar sunan bir modül olan itertools modülünün kullanımını öğrenelim.
permalink: /python-itertools-modulu
image: /img/2020/itertools.jpg
share-img: /img/2020/itertools.jpg
date: 2020-06-25
categories:
    - "python"
---
Python betikleri yazarken sık sık döngüleri kullanıyoruz. Bildiğiniz üzere döngü içerisinde bir veri tipini dolaşabilmek için o veri tipinin tekrarlanabilen, iterasyon içine alınabilen bir durumda olması gerekir. Mesela tipi int olan bir değeri döngüye sokamazsınız.

Döngüleri kullanırken herşey toz pembe ilerler fakat veri büyüdükçe döngüye sokmak biraz zorlaşmaya başlar. Bu tür durumlar için python ile beraber gelen core modüllerden biri olan **itertools** bize yardımcı oluyor. Hızlı çalışan, bilgisayarın kaynaklarını verimli kullanan fonksiyonlar barındırıyor. Şimdi isterseniz bu fonksiyonları inceleyelim.

## Kurulum
Kurulum gerektirmez, python ile beraber gelir ve projeniz içerisine dahil ederek kullanabilirsiniz.

```python
import itertools as itt
```
olarak projeme ekledim. Böylece **itt** kısaltması ile ilgili fonksiyonlara ulaşabilirim.

## cycle
Elinizdeki veriyi hiç bitmeyen bir döngüye sokar. Son elemana gelirse, başa döndürür ve döngüyü sürdürür.


```python
names = ['Sinan', 'Erdinç', 'Yasemin', 'Merve']
```
Elimizde şöyle bir liste olsun.

```python
c1 = itt.cycle(names)
print(next(c1))
print(next(c1))
print(next(c1))
print(next(c1))
print(next(c1))
```
Bu listeyi **cycle** fonksiyonuna parametre geçerek **c1** isimli bir değişkene attıktan sonra **next** fonksiyonuna bu c1 değişkenini gönderdiğimde bana şöyle bir çıktı verecek.


    Sinan
    Erdinç
    Yasemin
    Merve
    Sinan


Dikkat ederseniz tüm döngüyü dolaştı sonrasında tekrar Sinan yazdı.

## count
Basit bir sayaç oluşturur. Range fonksiyonu gibi bitiş limiti yoktur, başlangıç ve kaçar kaçar ilerleyeceğini girerek yönetebilirsiniz.

```python
c2 = itt.count(3,-1) # kaçtan başla, kaçar kaçar ilerle.
print(next(c2))
print(next(c2))
print(next(c2))
print(next(c2))
print(next(c2))
```
Görüldüğü üzere **count** fonksiyonuna 3 ve -1 değerlerini parametre olarak geçtim. 3'den başla ve -1 azaltarak devam et demiş oldum, ancak bir sınır vermedim sürekli bu devam edecek. **next** fonksiyonunu kullanarak ekrana bastığımda şöyle bir çıktı verecek.

    3
    2
    1
    0
    -1

Eğer next fonksiyonu ile sonraki elemanı ekrana yazmaya devam edersem azalmaya devam edecektir.

{: .box-note}
Yukarıda anlattığım cycle ve count hakkında hazırladığım videoyu izleyebilirsiniz.

<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/uYgwJAhXMBM"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>

## accumulate
Döngü içerisindeki tüm elemanları üst üste yığa yığa ilerler. 2 parametre alır, birincisi döngüye sokmak istenen veri, ikincisi de hangi fonksiyonu kullanarak yığma işlemi yapacağıdır.

```python
sayilar = [10,20,30,40,30,20,55] # elimizde şöyle bir liste olsun.

c3 = itt.accumulate(sayilar)
print(list(c3))
```
    [10, 30, 60, 100, 130, 150, 205]

Listenin ilk elemanının üzerine ikinciyi, sonra üçüncüyü toplayarak liste bitene kadar bunu sürdürdü. Dikkat ettiyseniz, yığma işlemini yaparken kullanacağı bir parametre geçmedim. Parametresiz çalıştığında buradaki fonksiyon **toplama işlemi** yapar.

Mesela biz çıkartma yaparak yığsın istiyoruz. O zaman **accumulate** içerisine ikinci parametre olarak basit bir lambda fonksiyonu yazabiliriz.

```python
c3 = itt.accumulate(sayilar, (lambda x,y: x-y))
print(list(c3))
```

    [10, -10, -40, -80, -110, -130, -185]

Şimdi de yığma işlemini çıkartarak yaptı. İstediğiniz lambda fonksiyonunu yazarak bunu tüm listeye yığabilirsiniz.
Dikkat etmeniz gereken lambda fonksiyonunu yazarken 2 adet parametre bekleyecek şekilde yazmalısınız. Çünkü yığma işlemi yapılırken accumulate fonksiyonu size 2 adet parametre gönderecek.

## chain
Döngüye sokabileceğin verileri parametre olarak geçtiğinde sana hepsini birleştirir. Aşağıdaki örnek içerisinde ben **list** fonksiyonundan geçirerek ekrana öyle bastırdım.


```python
c4 = itt.chain([1,2,3],"xyzd",{"k":"v"},("98",97))
print(list(c4))
```

    [1, 2, 3, 'x', 'y', 'z', 'd', 'k', '98', 97]

Gördüğünüz gibi fonksiyona liste, string, sözlük ve demet gönderdim, sonra hepsini birleştirip liste yaptım. Burada dikkat etmeniz gereken şey, **{"k":"v"}** sözlüğü içerisinde sadece **k** değerini aldı.

Şimdi aklınızda şöyle bir soru olacak, zaten ben iki listesi toplar gibi birleştirebiliyorum, yani

```python
k1 = [1,2,3]
k2 = [4,5,6]
k1 + k2
[1,2,3,4,5,6]
```
şeklinde yapabiliyorum, o zaman chain() fonksiyonu farklı ne? Normalde iki listeyi toplar gibi birleştirdiğinizde sonuç yeni bir liste olur. 
Ancak chain() ile birleştirdiğinizde bu bir iterator olan chain objesidir ve yeni bir liste oluşturmadan var olan listeleri birleştirip döngüye sokabileceğiniz bir hale getirir. 
Büyük verilerde hayat kurtarabilir.

{: .box-note}
Yukarıda anlattığım **accumulate** ve **chain** hakkında hazırladığım videoyu izleyebilirsiniz.

<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/543gImzq7I8"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>

## dropwhile, takewhile
Bu 2 fonksiyon, kendilerine gönderilen bir liste tipindeki bilgiyi alır, yine kendilerine gönderilen bir fonksiyondan geçirir, dönen değerin **True** veya **False** olmasına göre ise farklı şekilde davranır. Bu 2 fonksiyon biraz map ve filter ile karıştırılabilir ama küçük farkları var.


```python
sayilar = [3,4,5,6,7,2,1]  # elimizde şöyle bir liste olsun.

def test_filter(x):
    return x < 5

c5 = itt.dropwhile(test_filter, sayilar)  
print(list(c5))
```
    [5, 6, 7, 2, 1]

Eklediğimiz test_filter fonksiyonu içerisine, **dropwhile** üzerinden **sayilar** listesini gönderiyoruz. Fonksiyondan bize **True** dönüyorsa o değeri  atlıyor. İlk defa False döndüğü bir an olduğunda ise, artık döngüden çıkıyor.

Bu nedenle 3 ve 4 değerleri için **test_filter()** bize True döndüğü için atladı, (5'den küçük olduğu için) ancak 5'e geldiğinde, 5 < 5 değeri False döndüğünden dolayı bu noktadan sonra diğer listenin elemanlarını **test_filter()** içerisine göndermedi. Yani döngüyü bitirdi, bunu 2 ve 1 değerlerini de ayıklamamasından anlayabilirsiniz. Onlar normalde 5'den küçük.


Bunun tam tersi işlemi de takewhile yapıyor.

```python
c6 = itt.takewhile(test_filter,sayilar)
print(list(c6))
```
    [3, 4]

Eklediğimiz test_filter fonksiyonu içerisine, **takewhile** üzerinden **sayilar** listesini gönderiyoruz. Fonksiyondan bize ilk kez **False** dönene kadar tüm  **True** dönen yani 5'den küçük sayıları ayırıyor . İlk defa False döndüğü bir an olduğunda ise, artık döngüden çıkıyor.

Bu örnekte 3 ve 4 test_filter dan True dönenler, sonra 5 değeri test_filter dan False döndüğü için kalanlarına bakmadı ve döngüyü bitirerek sadece 2 adet sonuç döndürdü.


Şimdi aşağıya bir örnek daha ekliyorum, içinde **12** geçen şifreler ile ilgili bazı işlemler yaptım. 

```python
passwords = ["sinan12","b1236","y123","sinan","_2sinan","wqasd12"]
def p_filter(var):
    return "12" in var


print(list(itt.takewhile(p_filter, passwords)))
```
    ['sinan12', 'b1236', 'y123']


```python
print(list(itt.dropwhile(p_filter, passwords)))
```

    ['sinan', '_2sinan', 'wqasd12']

İlk önce şifre içerisinde **12** geçmesi ile ilgili bir fonksiyon yazdım. Sonrasında ise **takewhile** fonksiyonunu kullandım. Bu bana içerisinde 12 geçmeyen ilk şifreye bulana kadar içerisinde 12 geçen şifreleri döndü.
Ardından **dropwhile** fonksiyonunu kullandım ve içerisinde 12 geçen şifreleri atlayarak, ilk kez 12 geçmeyen şifreye gelince kalanlarını bana döndürdü.
Bu arada takewhile ve dropwhile sonucunda dönen elemanların toplamının listenin kendisini verdiğine dikkat edin.

{: .box-note}
Yukarıda anlattığım **dropwhile** ve **takewhile** hakkında hazırladığım videoyu izleyebilirsiniz.

<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/nUkw3ImXxiA"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>

## groupby
Bir liste içindeki elemanları, belirlediğimiz bir fonksiyonun sonucuna göre gruplar


```python
numbers = [1,2,3,4,3,2,1,0,5,1]
```

elimizde şöyle bir liste olsun. Bu liste içerisindeki sayıları, 3'e eşit veya küçük olacak şekilde gruplamak istiyorum.

```python
r1 = itt.groupby(numbers, key=lambda x:x <= 3)
```
şeklinde groupby() fonksiyonuna gruplamak istediğim listeyi ve sonrasında bir lambda fonksiyonu gönderdim. Başarıyla gruplandı ve lamda fonksiyonu sonucunda gelen 
True veya False değeri ve yanında grupladığı değerleri barındıran bir demet oluşturdu.

Bunu ekrana basabilmek için döngüye sokarak **k,v** şeklinde 2 değişkene değerleri dağıttım.

```python
for k,v in r1:
    print(k, list(v))
```

    True [1, 2, 3]
    False [4]
    True [3, 2, 1, 0]
    False [5]
    True [1]

3 den küçük olacak şekilde grupladı ve sonucunda grubun şartı sağlayıp sağlayamadığını True, False olarak söyleyip yanında ilgili listeyi de paylaştı.

Eğer kaynak bir liste değil de bir sözlük yani dict olsaydı o zaman şöyle yapmamız gerekirdi.


```python
stocks = {"code1":22,"code2":13,"code3":13,"code4":23,"code5":6,"code6":6}
```
Elimizde şöyle bir sözlük olsun, barkod değerleri ve kaç adet stok olduğunu tutuyor. Şimdi biz aynı adetli stokları gruplamak istiyoruz diyelim.

```python
r2 = itt.groupby(list(stocks.items()), key=lambda x:x[1])
for k,v in r2:
    print(k, list(v))
```

    22 [('code1', 22)]
    13 [('code2', 13), ('code3', 13)]
    23 [('code4', 23)]
    6 [('code5', 6), ('code6', 6)]


Gruplarken sözlüğün itemlarını çağırdım ve bunu da bir liste içine aldım sonra da her bir item için 1. indis yani stok değerine göre gruplamasını söyledim böylece aynı stok adedine sahip olanları bana listeledi.

## repeat
Adı üzerinde tekrar edilecek işler için kullanılır. Ek olarak bir parametre daha alır o da kaç kere tekrar edeceğini sınırlar.


```python
for k in itt.repeat("Hello",2):
    print(k)
```

```python
Hello
Hello
```

{: .box-note}
Yukarıda anlattığım **groupby** ve **repeat** hakkında hazırladığım videoyu izleyebilirsiniz.

<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/UctO4hqUEZU"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>

## zip_longest
Bu fonksiyonu anlatmadan önce aslında **zip()** fonksiyonunu anlatmak gerek. Detaylı olarak bu fonksiyonu [şurada](https://www.sinanerdinc.com/python-map-zip-reduce-filter-kullanimi "Map, Zip, Reduce ve Filter Fonksiyonları") yazmıştım. Yine de kısaca bahsedelim.

Şöyle bir hikaye uyduralım, İstanbul'da yağmur yağmış. Sizin elinizde de 4 adet ilçe ve oralara m2 başına kaç kg yağmur düştüğünün verisi olsun.

```python
k1 = ["taksim","istinye","beylikdüzü","kadıköy"]
k2 = [80,70,110]
```

ancak kadıköy ilçesinde bu yağmuru ölçen aletimiz bozulmuş ve ölçememiş. Bu nedenle k1 listesinde 4 ilçe varken, k2 listesinde 3 değer var. Ben bu iki listesi birbirleriyle aynı indise gelen karşılıklarını birleştirmek istersem,

```python
list(zip(k1,k2))
[('taksim', 80), ('istinye', 70), ('beylikdüzü', 110)]
```

şeklinde zip() fonksiyonunu kullanabiliyorum ancak dikkat ettiyseniz **kadıköy** bu yeni liste içerisinde yok çünkü zip fonksiyonu en az elemana sahip listenin değeri kadar birleştirme yapar.
Bir liste içinde 4 diğerinde 3 tane eleman varsa o zaman max 3 elemanlı bir liste oluşacaktır.

İşte bu durumu en az elemanlı değil de, en çok, en uzun elemana sahip liste üzerinden yapmak isterseniz o zaman **zip_longest()** kullanmak gerekir. Aynı örneği bunun üzerinden yapalım.

```python
list(itt.zip_longest(k1,k2))
[('taksim', 80), ('istinye', 70), ('beylikdüzü', 110), ('kadıköy', None)]
```

şimdi dikkat ettiyseniz **kadıköy** geldi ve değer olarak **None** yazıyor.

## islice
Bu fonksiyon da elinizdeki liste içerisinde belirli bir aralığı ulaşmak istediğinizde kulllanışlıdır. 

```python
a1 = [1, 2, 3, 'x', 'y', 'z', 'd', 'k']
```
şeklinde bir liste olsun ve biz bu listenin x ve y değerleri dışarıya çıkarmak istiyoruz. O zaman

```python
list(itt.islice(a1,3,5))
['x', 'y']
```
şeklinde döndürebiliriz. Aldığı 1. parametre elinizdeki liste, ikincisi nereden başlasın, üçüncüsü ise nereye kadar gitsin. Burda 3 dahil ancak 5 dahil değil.

Hemen şöyle bir soru aklınıza gelecektir, ben zaten bunu

```python
a1[3:5]
['x', 'y']
```
şeklinde yapabiliyorum neden islice() fonksiyonu kullanayım? 
Eğer köşeli parantezler ile bir aralık verirsem bu bana yeni bir liste oluşturur ancak islice() kullanırsam dönen sonuç bir iterator olan islice objesidir ve döngüye sokabilirsiniz. Elinizdeki veri büyük ise kullanışlı olacaktır.


{: .box-note}
Yukarıda anlattığım **zip_longest** ve **islice** hakkında hazırladığım videoyu izleyebilirsiniz.

<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/Tm_37Sk8QkA"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>

## filterfalse

Bir fonksiyona geçtiğimiz parametre eğer False dönüyorsa, filterFalse bunları yakalayarak bize geri dönüyor. Hemen bir örnek yapalım.

```python
a1 = [1,2,3,4,5]
```
Bu liste içerisinde 3'den büyük olanları bulmak istiyorum. Bir lambda fonksiyonu yazarak True veya False döndürebilirim ve bunu da filterfalse fonksiyonundan geçirirsem istediğimi yapabilirim.

```python
list(itt.filterfalse(lambda x:x <= 3, a1))
[4, 5]
```
Normalde lambda fonksiyonu içerisinde  **x <= 3** yapıyorum yani 3 ve 3'den küçük olanlar için True dön dedik. Yani 4 ve 5 için False dönüyor, filterfalse fonksiyonu da işte bu False dönenleri yakaladı.

## compress
Verilen listeleri birleştirerek, bu birleştirme esnasında sadece **True** olanları kullanan bir method.

```python
d1 = ["Kadıköy", "Beşiktaş", "Sarıyer"]
d2 = [True, False, False]

r1 = itt.compress(d1, d2)
print(list(r1))

['Kadıköy']

d2 = [True, False, True]
r1 = itt.compress(d1, d2)
print(list(r1))

['Kadıköy', 'Sarıyer']
```

Burada d1 ve d2 listelerini compress ile birleştirdik fakat bu esnada d2 içerisinde d1'in karşılığı olarak sadece True olanları
 birleştirdi bu nedenle ilk örnekte Kadıköy döndü sadece. Sonra d2 içerisinde Sarıyer'in karşılığı olarak True yazdık ve 
tekrar compress methodundan geçirdiğimizde bu sefer Kadıköy ve Sarıyer döndü.

{: .box-note}
Yukarıda anlattığım **filterfalse** ve **compress** hakkında hazırladığım videoyu izleyebilirsiniz.

<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/U_sOgKDtB9M"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>
