---
layout: post
published: false
title: Python Re Modülü
subtitle: Python'da Düzenli İfade (Regular Expression) Kullanımı
permalink: /python-re-modulu
image: /img/2017/python-os.png
share-img: /img/2017/python-os.png
tags: [python, development]
date: 2017-11-22
categories:
    - "python"
---
Düzenli ifadeler (Regular Expressions) yazılım hayatımızda mutlaka karşımıza çıkacak bir alandır. Çok sık çıkmaz ama kesinlikle çıkar. Anlaması biraz zor gelebilir, bazı temel yapılar ile üstesinden gelmeye çalışalım. Zaten bu içeriği de, ihtiyacım olduğunda tekrar geri dönüp, konuyu hatırlayabilmek için yazıyorum.

Düzenli ifadeler, dağınık bir metin içerisinde istediğimiz formattaki metinleri yakalayabilmemize imkan tanır. Mesela, bir yerde geçen tüm e-posta adreslerini veya içinde rakam geçen ve gmail uzantılı tüm mail adreslerini ayıklamak için kullanılabilir. Düzenli ifadeler kullanılmadığında ardı arkasına birçok if - else yazmanız gerekebilir. Bu modül, birkaç saatte yapabileceğiniz bir işlemi saniyeler içerisinde sizin yerinize yapabilir. Uzmanlaşması da biraz vakit alır.

## Kurulum
Python ile standart olarak gelen bir kütüphanedir. Kurulum gerektirmez.

```
import re
```

şeklinde projeniz içerisine aktardıktan sonra kullanmaya başlayabilirsiniz.

## search
Bu method, aranılan bir içeriğin ilgili metin içerisinde olup olmadığını kontrol eder.

```
metin = "Mustafa Kemal Atatürk 1881 yılında Selanik’te doğdu."
kontrol = re.search("Atatürk",metin)
>>> kontrol
<_sre.SRE_Match object; span=(14, 21), match='Atatürk'>
```

İlk önce bir metin adında değişken oluşturdum sonra metin içinde Atatürk geçiyormu diye sorgulayıp gelen objeyi kontrol değişkenine atadım. Sonra kontrol objesine baktım, dikkat ederseniz span ve match diye alanlar var. Burada match aradığınız değeri, span ise nerede olduğunu gösterir. Aradığınız yerdeki 14. ve 21. harfler arasındaymış o zaman kontrol edelim.

```
>>> metin[14:21]
'Atatürk'
```

Gördüğünüz gibi tekrar Atatürk değeri döndü. Search methodu ilgili metnin nerede olduğunu başarıyla hesapladı.
