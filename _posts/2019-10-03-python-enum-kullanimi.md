---
layout: post
published: true
title: Python Enum Kütüphanesi Kullanımı
subtitle: Sınıf içerisindeki değişkenlerin davranışlarını Enum kütüphanesi ile değiştirebilirsiniz.
permalink: /python-enum-kullanimi
image: /img/2019/enum.png
share-img: /img/2019/enum.png
date: 2020-01-27
categories:
    - "python"
---
Python scriptleri geliştirken bol bol değişken kullanıyoruz. Enum kütüphanesi de bu değişkenlerin özelliklerini biraz geliştiren, kodları daha okunaklı yapan ve hata ayıklamayı kolaylaştıran bir yöntem getiriyor.

## Kurulum
Python 3.4 üzerinden itibaren kullanabileceğiniz, [PEP 435](https://www.python.org/dev/peps/pep-0435/) ile eklenen standart bir kütüphanedir. Daha düşük bir versiyon kullanmıyorsanız herhangi bir kurulum gerektirmez.

<div class="youtubeContainer">
<iframe src="//www.youtube.com/embed/NPp5n3ClGPQ"
frameborder="0" allowfullscreen class="youtubeVideo"></iframe>
</div>

## Ne zaman kullanılabilir?
Örneklere başlamadan önce ne zaman **Enum** kullanmalısınız, onunla ilgili konuşalım.

- Sınırlı bir olası değeri olan değişken grubunuz varsa kullanmak uygun olur. Örneğin haftanın günleri veya yönler (doğu, batı v.s.)
- Sınıf içerisindeki değişkenler daha sonradan değiştirilmesin istiyorsanız kullanabilirsiniz.(Immutable)
- Sınıf içerisindeki değişkenleri bir iterasyona sokmak isterseniz kullanabilirsiniz.
- Değişkenlerin tuttuğu değerler benzersiz olsun, yani 2 değişken aynı değere sahip olamasın istiyorsanız kullanabilirsiniz.

## Kullanım
Projeniz içerisine,
```
import enum
```
şeklinde ekleyebilirsiniz. Kullanırken de, 2 yöntem var.

- 1: Oluşturduğunuz sınıfı, enum üzerinden miras alarak oluşturmak.
- 2: Sınıf oluştururken **class** yerine **Enum** kullanmak.

Örneklerimi hep 1. yöntem ile oluşturdum ama aşağıda ikinci kullanım yöntemini de anlattım.

**İlk Yöntem**
```
import enum

class Status(enum.Enum):
    active = 1
    inactive = 0
```
şeklinde, Status sınıfı oluştururken, **enum** üzerinden miras alarak kullanabiliriz.

**İkinci Yöntem**

```
Status = Enum("Status", {"active":1, "passive":0})
```

şeklinde. Ben ilk yöntemi daha kullanışlı buluyorum.

Şimdi de Enum üzerinden oluşturulan sınıflar için kazanılan bazı yetenekleri sıraladım. Bunları anlatırken de **enum.IntEnum** ve **@enum.unique** kavramlarını izah ettim.

### 1- İterasyon
Şöyle bir sınıf olsun.
```
class Status:
    active = 1
    inactive = 0
```
Eğer ben **Status** sınıfı içerisindeki değişkenleri bir iterasyona sokarsam yani döngü içine alırsam hata alırım. Yani bu sınıf için
```
for s in Status:
  print(s)
```
şeklinde bir döngü yazamam. Ancak sınıfı **Enum** üzerinden oluştursaydım o zaman döngüye alabilecektim. Döngü içerisinde değişken adını ve değerini kullanabilirim.

```
import enum

class Status(enum.Enum):
    active = 1
    inactive = 0

for s in Status:
  print(s.name, s.value)
```
Bu kod bloğunun çıktısı şöyle olacaktı.
```
active 1
inactive 0
```

### 2- Sıralama & Ayrıştırma
Bir üstteki örnek içerisinde bir sınıfa iterasyon uygulanamayacağını görmüştük.Bu sebepten dolayı da yapamayacağınız şeylerden biri de sort işlemi.
```
class Status():
  new = 2
  closed = 3
  invalid = 1
```
Şeklinde bir sınıfım olduğunda, bu sınıfın içindeki değişkenleri sort etmek istersem bunu yapamam. Zaten değişkenleri bir iterasyon ile dönemiyorum nasıl sort edebilirim?

```
import enum

class Status(enum.IntEnum):
  new = 2
  closed = 3
  invalid = 1

print(list(s.name for s in sorted(Status)))
```
şeklinde **IntEnum** dan türetilen bir sınıf yaparsanız, değişkenlerin değerine göre sort edebilirsiniz. Bu kodlar bana şunu dönecek.
```
['invalid', 'new', 'closed']
```
Yani 1,2,3 değerlerine göre sıraladım. Burada biraz **IntEnum** özelliğini açmakta fayda var. Sınıf miras alırken **IntEnum** kullanılırsa, içerisindeki değişkenlerin de integer olmasını bekler. Eğer integer olarak çevirebilecek bir değer ise kendi çevirir ama değilse hata verir.

Eğer
```
new = "2"
```
yazmış olsaydım da kendisi bunu integer olarak çevirecekti fakat

```
new="Status2"
```

gibi çeviremeyeceği bir şey yazılırsa ve sınıf **IntEnum** üzerinden oluşturulmuşsa o zaman hata alacaktım.

### 3- Benzersizlik
Normalde bir sınıf içerisindeki değişkenler aynı değerlere sahip olabilir, ancak siz 2 değişken aynı değere sahip olmasın isterseniz bunun için de **Enum** kütüphanesi içerisinde **@enum.unique** adında bir decorator var.

```
import enum

@enum.unique
class Status(enum.Enum):
  new = 2
  closed = 3
  invalid = 1
  inactive = 1
```
şeklinde kullandığımda, 1 değerine sahip iki adet değişken olduğu için hata alacağım.

Yine burada da not düşmekte fayda var. Eğer sınıfı oluştururken **IntEnum** üzerinden oluşturmuş olsaydınız ve
```
invalid = 1
inactive = "1"
```
şeklinde biri integer biri string değer olsaydı yine hata alacaktınız çünkü yukarda belirttiğim gibi integera çevrilebildiğinde kendisi çevirecek ve sonuçta aynı 2 tane değişken olacaktır. Fakat sınıfı **enum.Enum** üzerinden oluşturursanız o zaman biri integer biri string 2 farklı değişken olarak görecek ve hata almayacaktı.

{: .box-note}
Detaylı bilgi için [https://docs.python.org/3/library/enum.html](https://docs.python.org/3/library/enum.html) adresini kullanabilirsiniz.
