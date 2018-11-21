---
layout: base
title: Selenium Çalışma Örnekleri
subtitle: Selenium Çalışma Örnekleri
permalink: selenium.html
---

<style>
.row {
  background: #f8f9fa;
  margin-top: 20px;
}
.col {
  border: solid 1px #6c757d;
  padding: 10px;
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
footer{
    display: table;
    text-align: center;
    margin-left: auto;
    margin-right: auto;
}
</style>
<div class="container">
  <div class="row">
    <div class="col">
      <ul class="ornek1">
      <p>
      find_element_by_id
      </p>
      <li id="linux">Linux</li>
      <li id="python">Python</li>
      <li id="teknoloji">Teknoloji</li>
      </ul>
    </div>
    <div class="col">
      <ul class="ornek2">
      <p>
     find_element_by_class_name
      </p>
      <li class="linux">Linux</li>
      <li class="python">Python</li>
      <li class="teknoloji">Teknoloji</li>
      </ul>
    </div>
    
    <div class="col">
      <p>
      find_element_by_css_selector
      </p>
       <fieldset>
          <label>Hakkımda</label>
          <label>İletişim</label>
       </fieldset>
    </div>
  </div>
  
  <div class="row">
   <div class="col">
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
    <div class="col">
      <p>
      find_element_by_name
      </p>
       <span name="text1">Selenium modülü öğreniyorum.</span>
       <span name="text2">Python lover.</span>
       
    </div>
    <div class="col">
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
<footer> Tüm hakları saklıdır. sinanerdinc.com  - <a href="http://www.sinanerdinc.com" title="sinan erdinç websitesi">Anasayfa</a> </footer>
