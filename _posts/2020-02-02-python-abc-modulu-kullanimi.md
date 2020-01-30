---
layout: post
published: true
title: Python ABC Modülü Kullanımı
subtitle: Abstract base class, sınıfların hiyerarşisini belirler, sürdürülebilir, programcı dostu, bakımı kolay sistemler tasarlamaya yardımcı olur.
permalink: /python-abc-modulu-kullanimi
image: /img/2020/abc.png
share-img: /img/2020/abc.png
date: 2020-02-05
categories:
    - "python"
---
Bu ders içerisinde, soyut temel sınıfların avantajlarını ve bunları Python’un standart bir modülü olan ABC modülüyle nasıl yapabileceğimizi öğreneceksiniz.

ABC modülü Abstract Base Class kelimelerinin baş harflerinden türetilmiştir. Ne işe yaradığını anlatmaya başlamadan önce, bu yaklaşımın dile bizzat Guido van Rossum tarafından eklendiğini, kullanmanın bir zorunluluk olmadığını ancak herşeyin bir obje olduğu Python dilinde bu tür yaklaşımları kullanmanın, sürdürebilir, bakımı kolay sistemler tasarlamakta çok faydalı olacağını söylemek gerekir.

Abstract Base Class yaklaşımı, bir işlemi yapan tek bir sınıf oluşturarak, bu işlemi yapmak isteyen diğer sınıfların kendisinden miras alınmasını ister. Oluşturulan bu tek sınıf, diğerlerine hangi methodları oluşturması konusunda bir yol haritası çizer, herhangi bir implementasyon barındırmaz. Tüm işlemler, miras aldıktan sonra yeni oluşturulan sınıf içerisinde yapılmalıdır.

Bu tür yaklaşımlar da rahatlıkla sisteme yeni bir servis entegre etmeyi kolaylaştırır ve bunu yaparken de sizin doğru yolda ilerlemenize yardımcı olur.

## Senaryo & Problem
Tüm hikayeyi, problemi anlatmak için bir senaryo uyduralım. Uygulamamızda database olarak **MSSQL** kullandığımızı hayal edelim. Fakat bazı yerlerde **MYSQL** veya **MongoDB** de kullanıyoruz. İstediğimiz zaman da birbiri arasında geçiş yapabilmek istiyoruz. Bunu yaparken de, belki ileride PostgreSQL de kullanırız, sistemi öyle bir tasarlayalım, entegre ederken problem yaşamayalım istiyoruz.


## Kurulum & Kullanım
Herhangi bir kurulum gerektirmez. Standart bir kütüphanedir. Şimdi ABC modülününün kullanımına geçmeden önce soyut sınıflar python üzerinde alternatif olarak nasıl oluşturulur onu inceleyelim. Yukarıdaki senaryoyu çoğu kullanıcı şöyle kodlar.

<script src="https://gist.github.com/sinanerdinc/4372260421ce2012ddad2a07421d84f8.js"></script>

**AbstractDatabase** isimli ana bir sınıf var 2 adet methoda sahip, bunu miras alan da **MsSql** ve **MongoDB** adında 2 tane sınıf daha var. İkisinde de **connect()** methodu tanımlanmış ama **close()** methodu tanımlanmamış. Daha sonra MongoDB sınıfının connect ve close methodları çağrıldığında bir tanesi başarıyla çalışırken **MongoDB().close()** ise NotImplementedError error verecek. Fakat eğer **MongoDB().close()** çağırmazsam bir hata almadan devam edeceğim.

Şimdi bu örneği ABC modülü ile birlikte hazırlayalım.

<script src="https://gist.github.com/sinanerdinc/a6ad2c1f376a9a94266a07ed7883eeed.js"></script>

Burada dikkat etmemiz gereken bazı alanlar var, ilk önce abc modülünün **ABCMeta, abstractmethod** özelliklerini import ettik.AbstractDatabase sınıfı da **metaclass=ABCMeta** üzerinden oluşturduk. Sonrasında ise, bu temel sınıf miras alınırken, miras alan sınıfın içerisinde kesinlikle tanımlanması gereken methodların üzerine **@abstractmethod**  dekoratörünü ekledik.

Burda şunu söylemiş olduk, **MongoDB , MsSql** gibi **AbstractDatabase** sınıfını miras alarak oluşturulan veya yeni oluşturulacak tüm sınıflar, üzerinde **@abstractmethod** yazan yani connect ve close methodlarını **tanımlanmak zorundadır.** Tanımlamazsa sistem hata verir ve tanımlamasını söyler. Hata vermesi için de ilgili methodu çağırmasına gerek yok, base sınıftan bir kopya türetildiği anda abstractmethod değerleri kontrol edilir ve tanımlanmamış olan varsa hata verir.

Dikkat ettiyseniz bir de **retry** adında bir method var ve **@abstractmethod** olarak tanımlanmamış. Bu nedenle bunu MsSql veya MongoDB sınıflarında implemente etmesem bile hata almayacağım.

{: .box-note}
Detaylı bilgi için [https://docs.python.org/3.8/library/abc.html](https://docs.python.org/3.8/library/abc.html) adresini kullanabilirsiniz.
