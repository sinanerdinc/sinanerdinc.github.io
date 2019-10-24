---
layout: post
published: false
title: Python Enum Kütüphanesi Kullanımı
subtitle: Sınıf içerisindeki değişkenlerin davranışlarını Enum kütüphanesi ile değiştirebilirsiniz.
permalink: /python-enum-kullanimi
image: /img/2019/enum.png
share-img: /img/2019/enum.png
date: 2019-10-05
categories:
    - "python"
---
Python scriptleri geliştirken bol bol değişken kullanıyoruz. Değişken oluştururken Enum kütüphanesi bu değişkenlerin özelliklerini biraz geliştiren, okunaklı yapan ve hata ayıklamayı kolaylaştıran bir hale getirir.

## Kurulum
Python 3.4 üzerinden itibaren kullanabileceğiniz, [PEP 435](https://www.python.org/dev/peps/pep-0435/) ile eklenen standart bir kütüphanedir. Daha düşük bir versiyon kullanmıyorsanız herhangi bir kurulum gerektirmez.

## Kullanım
Projeniz içerisine,
```
import enum
```
şeklinde ekleyebilirsiniz. Kullanırken de, 2 yöntem var.

- 1: Oluşturduğunuz sınıfı, enum üzerinden miras alarak oluşturmak.
- 2: Sınıf oluştururken **class** yerine **Enum** kullanmak.

Örneklerimi hep 1. yöntem ile oluşturdum ama aşağıda ikinci kullanım yöntemini de anlattım.

Örneklere başlamadan önce ne zaman **Enum** kullanabilirsiniz onunla ilgili konuşalım.

- Sınırlı bir olası değeri olan değişken grubunuz varsa kullanmak uygun olur. Örneğin haftanın günleri veya yönler (doğu, batı v.s.)
- Sınıf içerisindeki değişkenleri bir iterasyona sokmak isterseniz kullanabilirsiniz.
- Değişkenlerin tuttuğu değerler benzersiz olsun, yani 2 değişken aynı değere sahip olamasın istiyorsanız kullanabilirsiniz.

### Temel İşlemler
```
import enum

class Status(enum.Enum):
    active = 1
    inactive = 0
```
şeklinde, Status sınıfı oluştururken, **enum** üzerinden miras alıyor. Detay ve örnekleri aşağıda verdim şuan sadece kullanım yöntemini örneklendiriyoruz.


### İterasyon
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
şeklinde bir döngü yazamam. Ancak sınıfı **Enum** üzerinden oluştursaydım o zaman döngüye alabilecektim. Döngü içerisinde değişken adını ve değerini kullanabilirim, mesela ekrana bastıralım.

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

### Sort Etmek
Bir üstteki örnek içerisinde bir sınıfa iterasyon uygulanamyacağını görmüştük.Bu sebepten dolayı da yapamayacağınız şeylerden biri de sort işlemi.
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
Yani 1,2,3 değerlerine göre sıraladım.

### Benzersizlik
Normalde bir sınıf içerisindeki değişkenler aynı değerlere sahip olabilir, ancak siz 2 değişken aynı değere sahip olamasın isterseniz bunun için de **Enum** kütüphanesi içerisinde **@enum.unique** adında bir decorator var.

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


{: .box-note}
Çok Güzel
