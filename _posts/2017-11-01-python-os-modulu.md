---
layout: post
published: true
title: Python Os Modülü
subtitle: İşletim sistemi işlemlerinizi Python ile yapabilirsiniz.
permalink: /python-os-modulu
image: /img/2017/python-os.png
share-img: /img/2017/python-os.png
date: 2017-06-11
categories:
    - "python"
---
Python, çoklu işletim sistemlerinde çalışabilen bir dil, dosya ve klasör yönetimi ile haşir neşir olmak da bunun bir parçası. Bu nedenle işletim sistemlerinin temel ihtiyaçlarına destek sağlayabilir. Bunun için oluşturulmuş OS modülünün en sık kullanılan methodlarını inceleyelim.

## Kurulum
Python ile standart olarak gelen bir kütüphanedir. Kurulum gerektirmez.

```
import os
```

şeklinde projeniz içerisine aktardıktan sonra kullanmaya başlayabilirsiniz.

## name()
```
os.name
```
ile işletim sisteminin adını alabilirsiniz. Windows ise nt linux ise posix dönecektir. Bunun kullanabileceğiniz en uygun yerlerden biri, eğer işletim sistemi Windows ile programı çalıştırmamak veya farklı methodlar kullanmak olabilir.

## system()
Kullanırken çok dikkat edeceğiniz bir method var, o da system() methodu.

```
>>> os.system("ls -l")
total 12
-rw-rw-r-- 1 sinan sinan   34 Kas  1 22:31 os.py
drwxrwxr-x 2 sinan sinan 4096 Kas  1 22:31 __pycache__
drwxrwxr-x 2 sinan sinan 4096 Kas  1 22:48 yeniKlasor
```

Bu method ile sanki terminal üzerinden ls -l komutunu vermişim gibi çalıştırıp sonucu döndürdü. Çok dikkatli kullanmanız gereken bir method.

Mesela klavyeden aldığınız bir veriyi direkt olarak bu methodun içinde çalıştırmak isterseniz kötü niyetli bir kişi sizi zor durumda bırakacak kodlar yazabilir.

## getcwd()

```
>>> os.getcwd()
'/home/sinan'
```
Bu method bizlere **o an içinde bulunduğumuz klasörü** döndürür.

## chdir()

```
>>> os.getcwd()
'/home/sinan'
>>> os.chdir("/home/sinan/Documents")
>>> os.getcwd()
'/home/sinan/Documents'
```
os.chdir() ile (change directory) **farklı bir klasör içerisine geçebilirsiniz.** İlk önce **getcwd()** ile nerde olduğuma baktım sonra **chdir()** ile **Documents** klasörüne indim. Tekrar **getcwd()** ile Documents klasöründe olduğumu doğruladım.

## listdir()

```
>>> os.chdir("/home/sinan/PycharmProjects/osModulu")
>>> os.listdir()
['.idea', 'os.py', '__pycache__']
```
os.listdir() ile hangi klasördeyseniz onun içerisindeki dosya ve klasörleri döndürür. Ben bu örnekte ilk önce **/home/sinan/PycharmProjects/osModulu** klasörüne gittim sonra da listdir() ile içindekileri görebildim. Bunu yapmak için o klasör içerisinde olmanıza gerek yok, **direkt olarak klasör yolunu parametre olarak da geçebilirsiniz.**

```
os.listdir("/home/sinan/PycharmProjects/osModulu")
```
gibi. Burada en sık ihtiyacınız olan durum o klasördeki belirli bir uzantılı dosyaları göstermek olacaktır. Zaten dikkat ettiyseniz sonuç bize bir liste döndürüyor. O zaman bir for döngüsü ile ilgili işlemi yapabiliriz.

```
>>> dosyalar = os.listdir()
>>> for dosya in dosyalar:
...     if dosya.endswith(".py"):
...             print(dosya)
...
os.py

```
İlk önce **dosyalar** adında bir değişken içerisine oradaki tüm dosyaların listesini attım. Sonra bunu bir for döngüsüne soktum ve ardından eğer bu döngüdeki her bir dosya veya klasör adının uzantısı .py ile bitiyorsa onu ekrana bastır.

Sonuç olarak sadece **os.py** ekrana basıldı.

## mkdir()

Eğer bulunduğunuz klasör içerisinde yeni bir klasör oluşturmak isterseniz **os.mkdir()** methodunu kullanıyoruz.

```
>> os.getcwd()
'/home/sinan/PycharmProjects/osModulu'
>>> os.mkdir("aa")
>>> os.listdir()
['.idea', 'aa', 'os.py', '__pycache__']
```

İlk önce **getcwd** ile nerede olduğumu kontrol ettim. Sonra **mkdir** ile aa adında bir klasör oluşturdum. Sonra da **listdir** ile aa klasörünün orada olduğunu kontrol ettim. Eğer yetkinizin olmadığı bir yerde klasör açmak isterseniz hata alabilirsiniz.

## rename()

Şimdi de **aa** klasörünün adını **yeniKlasor** yapalım.

```
>>> os.rename("aa","yeniKlasor")
>>> os.listdir()
['.idea', 'yeniKlasor', 'os.py', '__pycache__']
```
**os.rename()** ile **aa** olan klasör adını **yeniKlasor** yaptım. Sonra da listdir ile kontrol ettim ve başarıyla bu işlemin gerçekleştiğini gördüm.

## rmdir()
İçi boş olan bir klasörü silmek için kullanılır, az önce oluşturduğumuz yeniKlasor isimli dizini silelim.

```
>>> os.rmdir("yeniKlasor")
>>> os.listdir()
['.idea', 'os.py', '__pycache__']
```
rmdir() ile bu silme işlemini yaptım ardından listdir methodu ile bu klasördeki dosyaları listeledim.

## stat()
Herhangi bir dosyanın boyutu, değiştirilme tarihi gibi bilgileri döndürür.

```
>>> os.stat("os.py")
os.stat_result(st_mode=33204, st_ino=286555, st_dev=2050, st_nlink=1, st_uid=1000, st_gid=1000, st_size=34, st_atime=1509564693, st_mtime=1509564691, st_ctime=1509564691)
```

## walk()
Verdiğiniz bir klasörün altındaki dizinleri ve dosyaları görebileceğiniz bir method.

```
>>> for i in os.walk("/home/sinan/pythonProject"):
...     print(i)
...
('/home/sinan/pythonProject', ['adventureGame', 'learning'], [])
('/home/sinan/pythonProject/adventureGame', [], ['index.py'])
('/home/sinan/pythonProject/learning', ['modules'], ['index.py'])
('/home/sinan/pythonProject/learning/modules', ['__pycache__'], ['module.py'])
('/home/sinan/pythonProject/learning/modules/__pycache__', [], ['module.cpython-35.pyc'])
```

Örneğimizde **home/sinan/pythonProject** klasörü içindekileri görebilmek için **walk()** methodunu kullandım ve bir döngü içerisine soktum. Gelen değerlere dikkat ettiyseniz, **kökdizin, altdizin ve dosyalar.**

## path()
Bir dosya ve klasörün var olup olmadığını kontrol etmek istediğinizde **path.exists()** methodunu kullanabilirsiniz.

```
>>> os.path.exists("/home")
True
>>> os.path.exists("/home/sinan.py")
False
```
İlk önce /home klasörünü aradım ve True cevabını alabildim ardından **/home/sinan.py** aradım ve böyle bir dosya olmadığı için False cevabını aldım.

Verdiğiniz parametrenin bir dizin mi yoksa dosya mı olduğunu da sorgulayabilirsiniz. Bunun için ise **path.isdir()** ve **path.isfile()** methodlarını kullanıyoruz.

```
>>> os.path.isdir("/home/sinan")
True
>>> os.path.isdir("/home/sinan/olmayanklasor")
False
>>> os.path.isfile("/home/sinan/pythonProject/adventureGame/index.py")
True
>>> os.path.isfile("/home/sinan/pythonProject/adventureGame/olmayandosya.py")
False
>>>
```
Örnekte var olan bir klasörüm ve olmayan bir klasörümü sorguladım. Ardından var olan bir dosya ve olmayan bir dosyayı sorguladım, başarıyla True ve False değerlerini görebildim.

Elinizde bir dosya yolu olsun, burdaki dosya yolundan dosyanın adını ve uzantısını ayrı ayrı çekmek istiyorsunuz. O zaman **path.split()** ve **path.splitext()** methodları yardımınıza koşar.

```
>>> a , b = os.path.split("/home/sinan/pythonProject/adventureGame/olmayandosya.py")
>>> a
'/home/sinan/pythonProject/adventureGame'
>>> b
'olmayandosya.py'
>>> os.path.splitext(b)
('olmayandosya', '.py')
>>>
```
İlk önce path.split methodunu kullandım, bu method dosya yolunun son kısmını ayırır, bu işlemi yaparken de sonucu a ve b diye değişkenlere atadım. Yani b değeri dosyanın adı ve uzantısını, a değeri de bu dosya adından önceki yolu gösteriyor.

Ardından elimde sadece dosya adı ve uzantısı olan b değişkeni var, bunu da path.splitext() methoduna verince ad ve uzantı ayrılarak karşıma gelmiş oldu.
