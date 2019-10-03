---
layout: post
published: true
title: Python Click Modülü Kullanımı
subtitle: Terminalden, yazdığınız betik içerisine bir parametre & arguman göndermek için rahatlıkla kullanabileceğiniz bir paket.
permalink: /python-click-modulu
image: /img/2019/click-modulu.png
share-img: /img/2019/click-modulu.png
date: 2019-11-03
categories:
    - "python"
---

Python öğrenmeye başladığınızda, bir süre sadece terminal üzerinde çalışan uygulamalar yapacaksınız. Bu uygulamaları küçümsemeyin sakın, terminal aslında gün boyu en çok kullandığımız araçlardan biri. 
Terminal üzerinde bir python betiğini çalıştırmak için
```
python app.py
python3 app.py
```
gibi python versiyonunuza göre değişen bir komut kullanıyorsunuz. Bu komutlar sayesinde yazdığınız kodlar çalışıyor ve ekrana birşey yazdırmışsanız, terminalde onları görebiliyorsunuz. Çoğu zaman, yazdığınız fonksiyonlar birer parametre alacak ve bu da betik içerisinden gönderilmiş olacak, fakat sizin ihtiyacınız olan terminalden betiğinize bir parametre göndermek.İşte bu noktada click modülünü bu süreci çok kolaylaştırıyor.

Beraber terminalde çalışan basit bir uygulama yapalım.

```
import requests

def check(url):   
    r = requests.get(url)  
    if r.status_code == 200:  
        print("Girdiğiniz sayfa açılıyor.")  
    else:  
        print("Böyle bir sayfa bulunamadı.")

check("http://www.sinanerdinc.com")
```     
Bu uygulama, girdiğiniz bir bağlantı adresine GET isteği atıyor, eğer http status 200 dönerse sayfanın açıldığını söylüyor, eğer 200 haricinde bir kod dönerse sayfanın bulunamadığını söylüyor.

Herşey buraya kadar çok güzel, python yazmak için kullandığınız editörde, url adresini kısmını değiştirip çalıştırıyor hayatımıza devam ediyorsunuz.  Peki bu python betiği bir sunucuda olsa çalıştırmak için ne yapacaksınız?
```
python3 check.py
```
yazacaksınız. Gelelim problem olan alana, ben farklı siteler için bu betiği çalıştırmak istediğimde her seferinde bu betiğin içerisindeki url kısmını değiştirip kaydetmek istemiyorum, direkt olarak terminal üzerinden bir parametre geçmek istiyorum. Yani şöyle kullanmak istiyorum.
```
python3 check.py --url http://www.sinanerdinc.com
```
Ne zaman bir python betiğini terminal üzerinden çalıştırırken yanına parametre geçmek isterseniz bunun için click modülünü kullanabilirsiniz. Şimdi betiğinizi click modülü ile nasıl bu hale getirebileceğinize bakalım.

## Kurulum
Eğer yüklü değilse ilk önce pip3 yani paket yöneticisini kuralım.

```
sudo apt-get install python3-pip
```

komutu ile pip3 kurulumunu yaptıktan sonra click modülü kurulumuna geçelim.
```
pip3 install click
```
komutunu kullanabilirsiniz. Eğer bilgisayarınızda tek python sürümü varsa, (bende hem python 2 hem 3 yüklü.) o zaman sadece **pip install click** yazabilirsiniz.

## Kullanım
Betiğinize click modülünü import ederek başlamanız gerekir
```
import click
```
ardından bu modülü kullanmak için
```
@click.command()
```
şeklinde bir **decorator** eklemeniz gerekiyor. Son olarak ise, hangi fonksiyonunuz terminalden parametre alsın istiyorsanız o fonksiyonunuzun üzerine
```
@click.option("--url", default="http://www.sinanerdinc.com", prompt="Link", help="Kontrol etmek istediğiniz bağlantı adresini giriniz.")
```
ekleyebilirsiniz. Biraz burayı açalım,
 - **--url** alanı, terminalden vereceğimiz parametrenin önüne yazmamız gereken metni ifade ediyor ve ayrıca python betiğinizde url adında   bir değişken oluşturur.
  - **default** alanı, eğer terminalden bir parametre göndermezsem, standart olarak bunu kabul et demek.
  - **prompt** alanı, eğer terminalden bir parametre göndermezsem ve default özelliğini kullanmamışsam, direkt olarak terminal benden bu
   prompt alanına yazdığım metni gösterip benden içerisini doldurmamı
   bekliyor. Yani **input("Link")** kullanmışız gibi.
  - **help** alanı, terminalden betiğinizi çalıştırırken --help komutunu kullandığınızda, açıklama olarak istenilen parametreleri ve bu alana yazdığınız metni gösterir.
```
Options:
  --url TEXT  Kontrol etmek istediğiniz bağlantı adresini giriniz.
  --help      Show this message and exit.
```
şeklinde kullanım bilgisi verir.

Genel olarak toparlarsak, python betiğimiz şu hale gelmiş oldu.
```
import click  
import requests  
  
  
@click.command()  
@click.option("--url", prompt="Link", help="Kontrol etmek istediğiniz bağlantı adresini giriniz.")  
def check(url):  
    r = requests.get(url)  
    if r.status_code == 200:  
        print("website is online")  
    else:  
        print("website is offline")  
  
  
check()
```

{: .box-note}
check() fonksiyonuna artık **url** parametresi göndermiyorum. Artık click modülü bizim yerimize bunu gönderiyor.

