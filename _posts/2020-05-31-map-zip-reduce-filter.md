---
layout: post
published: true
title: Map, Zip, Reduce ve Filter Fonksiyonları
subtitle: Python scriptlerinizde her zaman kullanabileceğiniz, faydalı ve hız kazandıracak fonksiyonlar.
permalink: /python-map-zip-reduce-filter-kullanimi
image: /img/2020/map-zip-reduce-filter.png
share-img: /img/2020/map-zip-reduce-filter.png
date: 2020-05-31
categories:
    - "python"
---
Python scriptleri yazmaya başladığınızda, özellikle for döngüleri ve listeler konularını öğrendikten sonra sürekli benzer yaklaşımlar sergilemeye başlıyoruz.
Elindeki listeyi bir döngüye sok, her bir itetasyonda şunları kontrol et sonra onu yeni bir listeye append ederek o yeni listeyi döndür. Bu yaklaşım en sık kullanılan ve ilk akla gelen yöntem
fakat aslında bu tür yaklaşımlarda bizim işimizi çözebilecek çok iyi hazır fonksiyonlar var. Bugun bu fonksiyonlardan sırasıyla

- map()
- zip()
- reduce()
- filter()

üzerinde durmak istiyorum. Eminim ki bu fonksiyonları kesin bir yerlerde görmüşsünüzdür, blog yazısında, github reposunda veya youtube üzerinde bir eğitim izlerken.
Şimdi bu şahane fonksiyonların beraber detaylarını inceleyelim.

## Map()
**map(func, iter1)** şeklinde kullanılır. Yani elinizdeki bir fonksiyona, elinizdeki bir datanın elemanlarını sırasıyla gönderir
ve sonucu tek bir obje olarak geri döner. Ben aşağıdaki örneklerde hep liste kullandım iterasyona sokulabileceği için ama veri tipi
liste olmak zorunda değil demet v.s. de olabilirdi. Döngü içine alınabilen bir data olması yeterli.
 
Hemen bir örnek yapalım.

```python
liste = [2,3,4]
def karesi(sayi):
    return sayi ** 2
```
Burada elimdeki liste içindeki elemanların karesini almak ve yeni bir liste içerisinde bunları toplamak istiyorum.
Aklıma ilk gelen yöntem şu olurdu

```python
bos_liste=[]
for i in liste:
    bos_liste.append(karesi(i))

print(bos_liste)  # [4, 9, 16]
```
Boş bir liste oluşturduktan sonra, for döngüsü ile var olan listemi dolaşır ve her itarasyon için o elemanı karesi fonksiyonuna gönderirdim.
Bu yöntem güzel çalışır evet, ancak bunu benim yerime **map()** fonksiyonuna şöyle yaptırabilirim.

```python
sonuc = map(karesi,liste)
```
Dikkat ettiyseniz map fonksiyonuna 2 parametre gönderdim, ilki hangi fonksiyonu kullanacağım, ikincisi de hangi listeyi bu fonksiyona parametre geçeceğimi söyledim.

Map fonksiyonu bana bir obje döndürür. Eğer bunu bir liste halinde istersem list() fonksiyonundan geçirerek çevirmem gerekir.

```python
print(list(sonuc))  # [4, 9, 16]
```

**map(func, iter1)** şeklinde kullanıyoruz görüldüğü üzere. Ben bu örnek içerisinde karesi isimli bir fonksiyonu gönderdim,
madem ilk parametre olarak fonksiyon gönderiyorum o zaman istersem **lambda** fonksiyonu da yazabilirdim. Aynı örneği
**karesi()** fonksiyonu yapmadan şöyle de yapabilirdim.
 
```pytohn
map(lambda x:x**2,liste)
```

Peki map fonksiyonuna birden fazla liste göndermek istersem?
Elimde 2 adet liste olsun

```python
liste1 = [2,3,4]
liste2 = [1,5,9,11,55]
```

Ben bu listelerin elemanlarını birbiri ile karşılaştırarak, küçük olanları tek bir listede toplamak istesem?

```python
def kucuk(x,y):
    if x < y:
        return x
    return y
```

şöyle bir fonksiyon yazdıktan sonra 2 listeyi de map fonksiyonunu kullanarak kucuk fonksiyonuna parametre olarak gönderebilirim.

```python
list(map(kucuk,liste1,liste2))  # [1, 3, 4]
```

Kaç adet liste göndermek istiyorsak, göndereceğimiz fonksiyon o kadar parametreyi karşılayabilmeli.

Burada şunu farkettinizi mi peki sonuc **[1, 3, 4]** döndü. liste2 içerisinde [1,5,9,11,55] şeklinde 11 ve 55 değerleri de vardı.
Ancak sonuç içerisinde yer almadı. Bu örnekten çıkarmamız gereken sonuç şu, kaç tane liste gönderirsem, uzunluğu en az olan liste kadar iterasyona sokulur.
İlk liste 3 elemanlı olduğu için sonuç da 3 elemanlı oldu.

## Zip()
Kelimenin çevirisine baktığımızda zip, fermuar anlamına geliyor ve tam olarak da bu anlamda kullanılıyor. 2 listeyi birbiri indisleri üzerine birleştiriyor.
Bunu yaparken de en kısa elemanlı listenin uzunluğunu baz alıyor.

```python
isim = ["Sinan","Erdinc","Mehmet"]
yas = [30,24,29,40,39]
sonuc = list(zip(isim,yas))
print(sonuc)  # [('Sinan', 30), ('Erdinc', 24), ('Mehmet', 29)]
```

şeklinde kullanılır. Aynı map gibi, zip de bir obje döndüğü için list() fonksiyonundan geçirerek bir liste içine alabilirsiniz.
Dikkat ederseniz yas listesi 5 elemanlı ama zip den geçince yanındaki isim listesi 3 elemanlı olduğu için yas listesi de 3 elemana kadar iterasyona sokuluyor.

Bu yeni oluşturduğunuz listeyi de şöyle bir döngü ile görebilirsiniz.
```python
>>> for i,k in sonuc:
...     print("{} is {} years old.".format(i,k))
... 
Sinan is 30 years old.
Erdinc is 24 years old.
Mehmet is 29 years old.
```

## Reduce()
Kullanmak için projeye import edilmesi gerek.
```python
from functools import reduce
```
şeklinde import edebilirsiniz. 
Reduce fonksiyonu, döngüye sokabileceğiniz herhangi bir veri tipi içinde, veri tipinin içindeki tüm elemanları azaltarak dolaşan ve 
karşılaştırma yapmaya imkan tanıyan bir yapıdır. Biz örneklerimizi liste üzerinden yapalım.

```python
numbers = [11,3,9,12,4,15,66]
```
şu şekilde bir listemiz olsun ve bu liste içindeki en büyük sayıya ulaşmak istiyorum. İlk önce bir fonksiyon yazalım ve 2 sayıyı karşılaştırarak büyük olanı bize versin.

```python
def find_max(a,b):
    if a > b:
        return a
    return b
```
Ardından reduce içerisine bu fonksiyonu ve listeyi parametre olarak geçiyorum.

```python
reduce(find_max,numbers)  # 66
```

Burda sırasıyla şunları yaptı.
 - Listenin ilk 2 elamanını aldı ve find_max içerisine gönderdi yani find_max(11,3) yaptı ve 11 sayısını döndü.
 - find_max(11,9) yaptı ve 11 döndü.
 - find_max(11,12) yaptı ve 12 döndü.
 - find_max(12,4) yaptı ve 12 döndü.
 - find_max(12,15) yaptı ve 15 döndü.
 - find_max(15,66) yaptı ve 66 döndü.
 - liste içinde eleman kalmadığı için işlemi sonlandırdı ve son döndüğü değer olan  66 yı döndürdü.
 
Sürekli 2 elemanı fonksiyona parametre gönderdi. Ben burda max değeri buldum fakat sırasıyla kar topu gibi hepsini toplayan, çarpan çıkartan, minumumu bulan gibi örnekler de yapılabilir.

## Filter()
Adından da anlaşılabileceği gibi bir filtreleme yapan fonksiyondur.

Filter (func, iter) şeklinde kullanılır, iter öğelerini true veya false döndüren bir filtre işlevi olarak kullanır. Func (x) işlevinin doğru olduğu tüm x öğelerini içeren yeni bir liste döndürülür.

Bu fonksiyonu googleladığınızda hemen karşınıza bir liste içindeki belirli bir sayıdan büyük veya küçükleri ayıklayan, 2'ye tam bölünebilenleri çıkartan örnekler göreceksiniz.
Biz isterseniz böyle bir örnek yapmayalım. Hikayemiz şöyle olsun, elimizde kişi listesi ve hangi yıl işe girdiğinin ayrı ayrı bir listesi var.

```python
kisiler = ['Sinan', 'Erdinç', 'Mehmet']
tarihler = [2018, 2017, 2020]
```
bulmak istediğimiz 2018 ve 2020 dahil ve arasında işe girenleri ayıklamak. Bunu da filter ile yapmak istiyoruz. Şimdi adım adım gidelim,
ilk önce 2 listeyi birleştirelim.
```python
zip(kisiler,tarihler)  # [('Sinan', 2018), ('Erdinç', 2017), ('Mehmet', 2020)]
```
Zaten zip() fonksiyonunu öğrenmiştik. Şimdi eğer bunu zip edilmiş listeyi filter içerisinden geçirsem ve geçirirken de her bir elemanın içindeki 1. indis yani 2. değeri ki bu da tarihe denk geliyor kontrol etsem nasıl olur?

```python
list(filter(lambda zipped:2018<=zipped[1]<=2020,zip(isim,tarih)))
# [('Sinan', 2018), ('Mehmet', 2020)]
```

## Örnek bir problem ve çözümü

Gerçekten bu fonksiyonları kullanarak gerçek hayattan bir problem çözelim beraber.

Elimde bir metin var, ben bu metin içerisindeki nokta, virgül, ünlem gibi karakterleri sildikten sonra her bir kelimeyi ayıklayıp sonra en uzun kelimeyi, belirli bir karakterin üzerindeki kelimeleri, toplam harf sayısını v.s. bulmak istiyorum.

```python
import re 

text = "Merhaba, benim adım Sinan. Python yazmayı seviyorum!"

# regex ile istemediğim karakterleri siliyorum.
clean_text = re.sub('[!.,]','',text)
print(clean_text)  # 'Merhaba benim adım Sinan Python yazmayı seviyorum'

#Boşluğa göre split ediyorum böylece kelimeleri buluyorum.
words = clean_text.split()
print(words)  # ['Merhaba', 'benim', 'adım', 'Sinan', 'Python', 'yazmayı', 'seviyorum']

# 6 karakterden fazla olan kelimeleri ayıklayalım.
list(filter(lambda word:len(word) > 6,words))  # ['Merhaba', 'yazmayı', 'seviyorum']

# Kelimeler kaç karakterli onların listesini görelim
word_chars = list(map(lambda word:len(word),words))  # [7, 5, 4, 5, 6, 7, 9]

# Toplam tüm kelimelerin karakter sayısını bulalım.
total_chars = reduce(lambda x,y:x+y, word_chars)  # 43
```
