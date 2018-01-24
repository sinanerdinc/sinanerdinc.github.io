---
layout: post
published: true
title: Python PyAutoGui Modülü Kullanımı
subtitle: Python ile mouse konumu, tıklama, klavye kontrolü ve ekran görüntüsü alma gibi işlemleri yönetebilir, dilerseniz otomatikleştirebilirsiniz.
permalink: /python-pyautogui-modulu-kullanimi
image: /img/2018/pythonautogui/python-pyautogui.png
share-img: /img/2018/pythonautogui/python-pyautogui.png
tags: [python, development]
date: 2018-01-22
categories:
    - "python"
---

Bugün sizlere acayip eğlenceli bir modül anlatıyorum. Modülün adı **pyautogui**, bu modül ile bilgisayarınızdaki birçok işi otomatize edebilirsiniz. Mouse hareketlerinizi, klavyenizi yönetebilir, ekran görüntüleri alabilir birçok işlemi otomatize edebilirsiniz.

# İçindekiler
- [Kurulum](#kurulum)
- [Genel Fonksiyonlar](#genel-fonksiyonlar)
- [Mouse Aksiyon Fonksiyonları](#mouse-aksiyon-fonksiyonlar)
- [Klavye Aksiyon Fonksiyonları](#klavye-aksiyon-fonksiyonlar)
- [Mesaj Kutusu Fonksiyonları](#mesaj-kutusu-fonksiyonlar)
- [Ekran Görüntüsü Fonksiyonları](#ekran-grnts-fonksiyonlar)
- [Kaynaklar](#kaynaklar)

# Kurulum

Eğer yüklü değilse ilk önce pip3 yüklemek gerek.

```
sudo apt-get install python3-pip
```

komutu ile pip3 yükledikten sonra **pyautogui modülünün kurulumuna** geçelim.

```
sudo pip3 install python3-xlib
sudo apt-get install scrot
sudo apt-get install python3-tk
sudo pip3 install pyautogui
```
Şeklinde bilgisayarınıza kurabilirsiniz.

```
import pyautogui
```
şeklinde ise projeniz içerisine aktarabilirsiniz.

# Genel Fonksiyonlar

### 1- position()

Bilgisayarınızdaki mouse için, o an içerisinde bulunduğu konumu, pozisyonu döner.

```
>>> pyautogui.position()
(2594, 495)
>>> pyautogui.position()
(1579, 229)
>>> pyautogui.position()
(2668, 231)
>>> pyautogui.position()
(2050, 811)
```

Ben burda 4 kere çalıştırdım ve her seferinde mouse farklı bir konumdaydı.

### 2- size()
Ekran çözünürlük bilginizi geri döner.

```
>>> pyautogui.size()
(3286, 1080)
```

Ben şuan bu yazıyı yazarken, çift ekran kullandığım için biraz böyle orantısız çıktı.

### 3- onScreen(x,y)
Parametre olarak geçilen x ve y değerlerinin ekran içerisinde olup olmadığını True veya False olarak geri döner.

```
>>> pyautogui.size()
(3286, 1080)
>>> pyautogui.onScreen(4000,1080)
False
>>> pyautogui.onScreen(2000,1080)
False
>>> pyautogui.onScreen(2000,1070)
True
>>> pyautogui.onScreen(3286,1070)
False
>>> pyautogui.onScreen(3285,1070)
True
```

İlk önce **size()** methodu ile çözünürlüğe baktım **3286 x 1080**. Ardından **4000 x 1080** noktası benim çözünürlüğüm içinde mi diye sordum **False** döndü. **2000 x 1080** içinde mi diye sordum yine **False** döndü. Halbuki True döner diye beklemiştim. Demek ki burada küçük eşittir diye bir kontrol yok, sadece küçüktür diye bir kontrol var. O yüzden **3286 x 1080** den 1px düşük bir aralıkta başarıyla True dönecektir. Aynı şekilde **3286 x 1070** sorguladığımda False dönerken, **3285 x 1070** yani 1px düşüğü True dönüyor.

# Mouse Aksiyon Fonksiyonları
Şimdi biraz mouse hareketlerini değiştirmek için kullanılabilecek methodlara geçelim.

## 1- displayMousePosition()
Bu method size o an mouse koordinatlarını döner, canlı şekilde değişir. Ayrıca mouse hangi noktanın üzerinde ise oranın RGB renk kodunu da döndürür.

```
>>> pyautogui.displayMousePosition()
Press Ctrl-C to quit.
X: 2795 Y: 1020 RGB: ( 81, 192, 219)
```

## 2- moveTo(x,y)
Bu method ile mouse pozisyonunu bir noktadan diğerine sürükleyebilirsiniz.

```
>>> pyautogui.moveTo(1000,50)
```

Bunu çalıştırdığınızda tabiri caizce şaak diye mouse o noktaya gitti. Eğer bunu daha insancıl bir şekilde sürükleyerek götürmek isterseniz **duration** diye bir parametre daha alır ve içerisine saniye olarak bir değer girersiniz.


```
>>> pyautogui.moveTo(1000,50,duration=3)
```
Bu şekilde 3 saniyede ilgili x ve y konumuna sürüklendi.

## 3- moveRel(x,y)
Bu method ile aynı üstteki moveTo gibi mouse poziyonunu bir noktadan diğerine sürükleyebilirsiniz, ancak moveTo 'dan farkı relatif bir sürükleme yaparsınız. Peki relatif sürükleme nedir derseniz de onu şöyle izah edelim. Bazen bulunduğunuz mouse koordinatından sağa 100px veya yukarı 200px gitmek isteyebilirsiniz. En sonunda hangi koordinata gideceğinizi bilmiyorsunuz ama bulunduğunuz koordinatın üzerine bir değer eklemek veya çıkarmak istiyorsunuz. O zaman kullanabilirsiniz.

```
>>> pyautogui.moveRel(-400,0,duration=3)
>>> pyautogui.moveRel(0,200,duration=1.5)
```
Birinci satırda 3sn içinde mouse için o anki koordinattan sola 400px gittim. İkincisinde ise 1.5 saniye içerisinde aşağıya 200px gittim.

## 4- dragTo(x,y)
Sürükle bırak işlemini yapan methoddur. Mouse o an hangi pozisyonda ise, ordan dragTo içerisindeki konuma sürükle bırak yapar.

```
pyautogui.dragTo(1394,530,duration=4)
```
Yine duration ile saniye cinsinden değer gönderebilirsiniz.

## 5- dragRel(x,y)
Sürükle bırak işlemini yapan methoddur. Yukarıdaki dragTo methodundan farklı olarak relatif olarak sürükle bırak yapar yani belirli bir konuma gitmez de, var olan konumdan x veya y yönünde belirli bir noktaya iner veya çıkar.

```
>>> pyautogui.dragRel(0,100,duration=3)
```
Yine duration ile saniye cinsinden değer gönderebilirsiniz.

## 6- click(x,y)
Tıklama işlemini yapar. Ek olarak button parametresi alır yani sol buton ile mi tıklansın, sağ buton ile mi yoksa ortadaki buton ile mi. Burda button parametresi 3 değer alabilir. Bunlar 'left', 'middle', ve 'right' . Bir de click parametresi alır, kaç kere tıklansın anlamındadır.

```
pyautogui.click(button='right', clicks=3, interval=0.25)
```
Bu örnekte 3 kere sağ tıklama işlemi yapılır ve  her seferinde 0.25 saniye bekler.

## 7- rightClick(x,y)
Sağ tıklar.

```
>>> pyautogui.rightClick(x=1393,y=333)
```
Dilersem x= ve y= de yazabilirim. Bunu öyle yazdım. Yazmadan (1393,333) de yapabiliriz.

## 8- doubleClick(x,y)
Çift tıklar.

```
>>> pyautogui.doubleClick(1403,54)
```

## 9- scrool(miktar)
Sayfada scrool yapar.

```
>>> pyautogui.scroll(10)
```
Sanki 10 kere mouse üzerindeki tekerlek ile scrool yapmışım gibi davranır.

## 10- mouseDown() ve mouseUp()
Bu methodlardan ilki olan mouseDown() mouse tıklama işlevini yapar. Ancak bastığınız butonu bırakmaz, ta ki siz bırak diyene kadar. Peki bırak nasıl deriz, onu da mouseUp() ile yapıyoruz.

Peki bu tıklamayı sol buton ile mi sağ ile mi yoksa ortadaki buton ile mi yapabiliriz? Bunu da parametre olarak geçebilirsiniz.

```
>>> pyautogui.mouseDown(x, y, button='left')
>>> pyautogui.mouseUp(x, y, button='left')
```

# Klavye Aksiyon Fonksiyonları
Sanki klavyenizi kullanmışcasına işlemler yapabileceğiniz methodlar şöyle.

## 1- typewrite()
Bir metin yazmak için kullanılır.

```
pyautogui.typewrite("www.sinanerdinc.com", interval=0.05)
```
Mouse hangi konumdaysa oraya www.sinanerdinc.com yazar ve her bir harfden sonra 0.05 sn bekler.

## 2- press()
Bu method ile enter, capslock ve space gibi butonlara da basabilirsiniz. Bunun için method içine ne yazmanız gerektiği ile ilgili listeye ulaşmak için

```
pyautogui.KEYBOARD_KEYS
```

komutunu yazabilirsiniz. Bu size kullanabileceğiniz anahtarların listesini döndürür.

```
pyautogui.press("enter")
```
şeklinde de kullanabilirsiniz.

{: .box-note}
**Not:** Bu methodu kullanırken doğru mu yapıyorum diye merak ederseniz [http://www.keyboardtester.com/tester.html](http://www.keyboardtester.com/tester.html?ref=sinanerdinc.com "http://www.keyboardtester.com/tester.html") bağlantısını kullanabilirsiniz. Hangi butona bastığınızı güzelce gösteriyor.

##  3- keyDown() ve keyUp()

Yukarıdaki press() methodu bas ve bırak işlemini yaparken sadece basma sonra da sadece bırakma işlemleri için keyDown ve keyUp methodlarını kullanabilirsiniz.

```
for i in range(5):
  pyautogui.keyDown('shift')
  pyautogui.typewrite("aaa")
  pyautogui.keyUp('shift')
  pyautogui.typewrite("bbb")
```
Yukarıdaki örnek eğer mouse koordinatı yazı yazılabilecek bir yer ise **AAAbbbAAAbbbAAAbbbAAAbbbAAAbbb** şeklinde birşey yazar. Neden böyle yazdığına bakalım. İlk önce shift butonuna bas ve bırakmadan aaa yaz diyoruz, bu nedenle shift basılı olduğundan AAA yazıyor. Ardından shift i bırak ve bbb yaz diyoruz. Bu nedenle büyük AAA ve küçük bbb şeklinde yanyana 5 kere yazıyor. Çünkü for döngüsüne soktuk 5 kere yaz dedik.

## 4- hotkey()
Bu method ise ctrl + c  veya ctrl + v  gibi komutları çalıştırmaya yarar. Bu method olmasa keyDown ve keyUp methodları ile uzun uzun yazmak zorunda kalabilirdik.

```
pyautogui.hotkey('ctrl', 'c')
pyautogui.hotkey('ctrl', 'v')
```
Kopyala ve yapıştır yapmış olduk.


# Mesaj Kutusu Fonksiyonları

Bu modül ile işlemler yaparken bazen kullanıcıdan onay almak isteyebilirsiniz, böyle anlarda bir mesaj kutusu çıkarmanız gerekecek. Şimdi de bunları inceleyelim.

## 1- alert() ve confirm()
Ekrana uyarı ve onay kutuları çıkartırlar.

```
>>> pyautogui.alert(text='Başvurunuz Alındı.', title='www.sinanerdinc.com', button="Tamam");
'Tamam'
```
Bu örnekte ekranda başlığı www.sinanerdinc.com olan ve içinde Başvurunuz Alındı yazan bir kutu çıkar altında da Tamam butonu bulunur. Eğer Bu butona basılırsa geri Tamam değeri döner.

![pyautogui alert](/img/2018/pythonautogui/alert.png#center "pyautogui alert")

```
>>> pyautogui.confirm(text='Başvurunuzu iptal etmek istediğinizden emin misiniz?', title='www.sinanerdinc.com', buttons=["İptal","Tamam"]);
'İptal'
```
Yine aynı şekilde bu sefer 2 buton çıkar ve hangisine basılırsa onun değeri geri döner.

![pyautogui confirm](/img/2018/pythonautogui/confirm.png#center "pyautogui confirm")

## 2- prompt() ve password()
Metin kutusu oluşturmanızı sağlar.

```
>>> pyautogui.prompt(text='Lütfen adınızı giriniz.', title='www.sinanerdinc.com' , default='')
'Sinan'
```

![pyautogui prompt](/img/2018/pythonautogui/prompt.png#center "pyautogui prompt")

Eğer girilen metin kutusunda şifre gibi özel maskeli birşeyler olacak ise o zaman password() methodunu kullanabilirsiniz.

```
>>> pyautogui.password(text='Lütfen şifrenizi giriniz.', title='www.sinanerdinc.com' , mask="*")
```
![pyautogui password](/img/2018/pythonautogui/password.png#center "pyautogui password")


# Ekran Görüntüsü Fonksiyonları
Ekran görüntüsü ile ilgili işlemlerin yapıldığı methodları inceleyelim.

## 1- screenshot()
Ekran görüntüsü almak için kullanılan method.

```
>>> pyautogui.screenshot("/home/sinan/Downloads/ekrangoruntum.png")
<PIL.PngImagePlugin.PngImageFile image mode=RGB size=3040x900 at 0x7FA7DF6AA208>
```
Bu şekilde ekran görüntüsünü belirli bir yola kaydedebilirsiniz.

## 2- locateCenterOnScreen()
Bu kullanışlı method ise bilgisayarınızdaki bir resim, ekranda varsa hangi koordinatlarda olduğunu size döner. Ben avatarımı kestim ve bilgisayarıma kaydettim.

![sinan avatar](/img/2018/pythonautogui/sinan.png#center "sinan avatar")

Ardından bu avatar sayfada hangi koordinatlarda onu aradım.

```
x,y = pyautogui.locateCenterOnScreen("/home/sinan/Pictures/sinan.png")
>>> x,y
(740, 181)
>>> pyautogui.moveTo(x,y,duration=1.5)
```
Ardından gelen verileri kontrol ettim, sonrasında ise moveTo() methodu ile mouse avatarımın bulunduğu yere gitt 1.5 saniye içinde. Çok güzel değil mi?

Resim bulmayı hızlandırmak için aramayı siyah beyaz resim olarak yapmak için **grayscale=True** parametresi de var.

```
pyautogui.locateCenterOnScreen("/home/sinan/Pictures/sinan.png",grayscale=True)
```
şeklinde kullanılır. Aradığınız görselin belirgin olması bulmayı kolaylaştırır. Ayrıca eğer siyah beyaz arama yapacaksanız, aynı şekle sahip sadece farklı renkteki resimler de bulunabilir. Aklınızda bulunsun.

# Kaynaklar
- https://pyautogui.readthedocs.io/en/latest/cheatsheet.html
- https://automatetheboringstuff.com
