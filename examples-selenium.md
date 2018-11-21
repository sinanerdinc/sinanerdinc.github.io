---
layout: page
title: Selenium Çalışma Örnekleri
subtitle: Selenium dersi için methodları test etmek istediğinizde kullanabileceğiniz çalışma ortamıdır.
permalink: selenium.html
---

<style>
  .rowbg {
  background: #f8f9fa;
}

.border1px {
  border: solid 1px #6c757d;
}
p {
  font-style: italic;
}
label{
  display: table-header-group;
}

span{
  display: table-header-group;
}

</style>


<div class="container">
  <div class="row rowbg">
    <div class="col-lg-3">
      <ul class="ornek1">
      <p>
      find_element_by_id
      </p>
      <li id="linux">Linux</li>
      <li id="python">Python</li>
      <li id="teknoloji">Teknoloji</li>
      </ul>
    </div>
    <div class="col-lg-3 border1px">
      <ul class="ornek2">
      <p>
     find_element_by_class_name
      </p>
      <li class="linux">Linux</li>
      <li class="python">Python</li>
      <li class="teknoloji">Teknoloji</li>
      </ul>
    </div>
    <div class="col-lg-3 border1px">
      <p>
      find_element_by_css_selector
      </p>
       <fieldset>
          <label>Hakkımda</label>
          <label>İletişim</label>
       </fieldset>
    </div>
  </div>
  
  <div class="row rowbg">
   <div class="col-lg-3 border1px">
      <p>
      find_element_by_css_selector
      </p>
       <fieldset>
          <label data="isim">Sinan</label>
          <label data="soyisim">Erdinç</label>
          <label data="site">www.sinanerdinc.com</label>
          <label data="menu1">Menü 1</label>
          <label data="menu2">Menü 2</label>
        </fieldset>
    </div>
    <div class="col-lg-3 border1px">
      <p>
      find_element_by_name
      </p>
       <span name="text1">Selenium modülü öğreniyorum.</span>
       <span name="text2">Python lover.</span>
    </div>
    <div class="col-lg-3 border1px">
      <p>
      find_element_by_xpath
      </p>
      <div class="col">
        <div>
        <ul>
        <li>Hakkımda</li>
        <li>İletişim</li>
        </ul>
        </div>
       <button data="submit">Onayla</button>
      </div>
    </div>
  </div>  
</div>
