---
layout: post
published: true
title: Python Virtualenv Kullanımı
subtitle: Python ile sanal bir geliştirme ortamı kurabilir ve istediğiniz modülleri bilgisayarınızdan bağımsız kullanabilirsiniz.
permalink: /python-virtualenv-kullanimi
image: /img/2018/virtualenv.png
share-img: /img/2018/virtualenv.png
date: 2018-04-29
categories:
    - "python"
---

Python modülleri ile haşır neşir olmaya başladığınızda bir bakarsınız onlarca modülü bilgisayarınıza yüklemişsiniz. Belki meraktan yüklenen onlarca modül sizi rahatsız edebilir, bazen de bilgisayarınızdaki bir modülün daha düşük versiyonlu halini, geliştirdiğiniz farklı bir projenizde kullanmak isteyebilirsiniz. İşte bunun gibi ihtiyaçlardan dolayı virtualenv diye bir araç ortaya çıktı.

Bu araç size bilgisayarınızdan bağımsız bir geliştirme ortamı sunar, siz istediğiniz modülleri bu geliştirme ortamında kurarsınız, yüklenen modüller sadece o proje için geçerli olup bilgisayarınıza yüklenmez.

Aslında pycharm kullanıyorsanız buna alışkın olmalısınız. Pycharm size her yeni projede yeni bir virtualenv kurar, bu nedenle bazen şöyle sorular sormuş olabilirsiniz;

- Bu modülü bilgisayarıma yükledim neden pycharm projemde kullanamıyorum?
- Pycharm da projeme x modülünü kurdum neden bilgisayarımda farklı projemde kullanamıyorum.

Şimdi kendimize yeni bir virtualenv nasıl kurulur onu inceleyelim.

# Kurulum

Eğer yüklü değilse ilk önce pip3 yani paket yöneticisini kuralım.

```
sudo apt-get install python3-pip
```
komutu ile pip3 kurulumunu yaptıktan sonra virtualenv kurulumuna geçelim.

```
sudo pip3 install virtualenv
```
Bu komut ile virtualenv artık bilgisayarımıza kuruldu. Kontrol etmek için

```
virtualenv --version
```
komutunu çalıştırabilirsiniz.

Herşey yolunda ise devam edelim. Şimdi ise bir klasöre virtualenv kuracağız ve o bizim artık geliştirme ortamımız olacak.

```
➜  cd /tmp
➜  virtualenv ornekProje  
Using base prefix '/usr'
New python executable in /tmp/ornekProje/bin/python3
Also creating executable in /tmp/ornekProje/bin/python
Installing setuptools, pip, wheel...done.
➜  cd ornekProje

```
İlk önce tmp klasörüne giriş yaptım. sonrasında **virtualenv ornekProje** komutu ile bir geliştirme ortamı kurdum. Sonrasında **cd ornekProje** komutu ile o klasör içerisine girdim. Geliştirme ortamımı kurdum ancak daha aktif hale getirmedim. Bunun için de

```
source bin/activate
```
komutunu çalıştırıyorum. Bu komutu çalıştırmak için geliştirme ortamı için oluşturduğum klasör içerisinde olmam gerek. eğer bu klasör içerisinde değilsem **source ornekProje/bin/activate** gibi bir komut yazmam gerekirdi.

Bu komutu yazdığınızda terminaliniz, parantez içerisinde (ornekProje) yazan bir hale gelecek. Artık geliştirme ortamınız hazır. Artık burada **pip3 ile istediğiniz paketi** kurabilirsiniz. Burda kurduğunuz paketler bilgisayarınıza yüklenmez sadece bu projeye yüklenir ve projenizi sildiğinizde de bilgisayarınızdan silinmiş olur.

Eğer bu geliştirme ortamından çıkmak isterseniz de;

```
deactivate
```
komutunu yazmanız yeterli.

Eğer pycharm kullanıyorsanız, yeni bir proje açtığınızda içerisinde venv diye bir klasör göreceksiniz. Pycharm ile açtığınız projeye yeni bir modül yüklemek istediğinizde ise

```
source venv/bin/activate
```

komutu ile geliştirme ortamınıza bağlanabilir sonrasında pip3 ile istediğiniz paketi yüklersiniz.

Unutmayın, geliştirme ortamına giriş yapmadan pip3 ile bir paket yüklerseniz, yüklediğiniz bu paket sizin virtualenv ile açtığınız geliştirme ortamınız içerisine dahil olmaz.
