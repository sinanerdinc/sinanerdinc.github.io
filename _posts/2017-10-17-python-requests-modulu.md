---
layout: post
published: false
title: Python Requests Modülü
subtitle: Web isteklerinizi python ile yapın
permalink: /python-requests-modulu
image: /img/2017/python_logo.png
tags: [python, development]
date: 2017-10-17
categories:
    - "python"
---
Python, standart modüllerinin yanında harici yüzlerce kullanışlı modül ile birlikte çok güçlü bir dil. Bu gücü veren harika modüller var bunlardan biri de Requests modülü.

Bu modül ile web üzerindeki isteklerinizi yöneteceksiniz. Mesela bu modül ile bir web service üzerinde işlem yapabilirsiniz veya API entpointlerine PUT, DELETE,POST gibi istekler atabilirsiniz.

## Kurulum

```
sudo apt-get install python3-pip
```

komutu ile pip3 kullanmaya başlayabiliriz. Eğer pip3 yüklüyse buna gerek yok, şimdi requests modülünü kuralım.

```
pip3 install requests
```
Artık modülü kurduk, projemiz içerisine
```
import requests
```

diyerek aktaralım. Şimdi de bu güzel modülün özelliklerine geçelim.

## random()
