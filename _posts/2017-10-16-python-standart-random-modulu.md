---
layout: post
published: false
title: Python Standart Random Modülü
image: /img/2017/python_logo.png
tags: [python, development]
date: 2017-10-16
categories:
    - "python"
---
Python içinde standart olarak gelen Random modülünden bahsedelim biraz. Rastgele sayı üretmeyi sağlayan bir modül olan Random, python 1.4 ve üzerinde kullanılabiliyor ve **mersenne twister** algoritmasını baz alarak çalışıyor. Uygulamanıza import ederek hemen kullanmaya başlayabilirsiniz. Hemen örneklere geçelim.

```
import random
```
şeklinde uygulamamız içerisine çağırdık.

## random()

Bu bize 0 <= n < 1.0 aralığında bir sayı döner.

```
>>> random.random()
0.38872204424977774
```

## uniform(min,max)

Bu bize min + (max - min) * random() işlemi sonucunda bir sayı döner.

```
>>> random.uniform(1,100)
52.19820527331601
```

## randint(min,max)

Min ve max aralığında integer olan bir sayı döner. Max dahildir. min <= n <= max

```
>>> random.randint(1,100)
86
```

## randrange(min,max)

Min ve max aralığında max dahil olmayan bir sayı döner. min <= n < max
3. bir parametre daha alır o parametre de bölünebilmeyi ifade eder.

```
>>> random.randrange(1,100)
71
>>> random.randrange(1,11,3) # 1 ve 11 arasında 3'e bölündüğünde 1 kalan bir sayı getir.
4
>>> random.randrange(1,11,3) # 1 ve 11 arasında 3'e bölündüğünde 1 kalan bir sayı getir.
7
>>> random.randrange(1,11,3) # 1 ve 11 arasında 3'e bölündüğünde 1 kalan bir sayı getir.
1
```

## sample(liste,q)

Liste içinde q adet rastgele değeri döner.

```
>>> sayilar = range(50)
>>> random.sample(sayilar,3)
[1, 19, 16]
>>> random.sample(sayilar,3)
[7, 10, 49]
>>> random.sample(sayilar,3)
[11, 37, 36]
```

## shuffle(list)

Verdiğiniz bir liste içindeki değerlerin sırasını karıştırır.

```
>>> l = list(range(10)) # örnek bir liste oluşturalım.
>>> l # bu listenin içeriğine bakalım.
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> random.shuffle(l) # l listesini karıştıralım.
>>> l
[4, 8, 7, 3, 2, 1, 6, 5, 9, 0]
```
