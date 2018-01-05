---
layout: general
title: ooo - Django Öğreniyorum
---
<div data-gnl="kaplama" markdown="1">
# İlk Django Uygulamanızı Yazma, Bölüm 6

  Bu eğitim [Öğretici 5]({{site.belgeler_ogretici5}}) 'in kaldığı yerden devam ediyor. Sınanmış bir anket uygulaması yaptık ve şimdi bir biçem düzeniyle resim ekleyeceğiz.

  Sunucu tarafıdnan üretilen HTML dışında, ağ uygulamalarının genellikle, eksiksiz ağ sayfasını oluşturmak için gerekli olan resimler, JavaScript veya CSS gibi ek dosyalar sunmaları gerekir. Django'da, bu dosyalara durgun dosyalar diyoruz. Teknik olarak statik dosya diyeceğiz.

  Küçük projeler için bu önemli bir mesele değildir. Çünkü durgun dosyaları ağ sunucunuzun bulabileceği bir yerde tutabilirsiniz. Bununla birlikte, daha büyük projelerde, özellikle de birden fazla uygulamadan oluşan her uygulama tarafından sağlanan birden fazla durgun dosyayı ele almak karmaşıklaşmayı başlatır.

  Django.contrib.staticfiles'in amacı şudur: her uygulamanızdan (ve belirttiğiniz diğer yerlerden) durgun dosyaları, üretimde kolayca sunabilecek tek bir yere toplar.

## Uygulamanızın görünümünü özelleştirin

  Önce, anketler dizininizde "static" adında bir dizin oluşturun. Django, durgun dosyaları Django'nun şablonları anketler/templates/ içinde araması gibi bu dizinde arayacaktır.

  Django'nun STATICFILES_FINDERS ayarı, çeşitli kaynaklardan durgun dosyaları nasıl bulacağını bilen bir bulucu listesi içerir. Varsayılanlarından biri, sadece oluşturduğumuz anketlerde olduğu gibi INSTALLED_APPS'in her birinde "static" bir alt dizin arayan AppDirectoriesFinder vardır. Yönetici sitesi, durgun dosyalar için aynı dizin yapısını kullanır.

  Yeni oluşturduğunuz durgun dizininde anketler adlı başka bir dizin oluşturun ve bunun içinde style.css adlı bir dosya oluşturun. Başka bir deyişle, biçem sayfanız anketler/static/style.css olmalıdır. AppDirectoriesFinder durgun dosya bulucu nasıl çalıştığından dolayı, Django'daki bu durgun doyası, şablonların yolunu kaynakça alanına benzer şekilde, anketler/style.css olarak atabilirsiniz.

<div data-bilget="genel" markdown="1">

### Durgun dosya adları yerleştirme

  Topkı şablonlar gibi, durgun dozyalarınızı doğrudan anketler/static (başka bir anket alt dizini oluşturmaktan ziyade) koyarak kurtulabiliriz, ancak aslında kötö bir fikir olacaktır. Django, bulduğu ilk durgun dosyayı adıyla eşleştirecek ve farklı bir uygulamada aynı ada sahip durgun bir dosyanız olsaydı onları ayırt edemeyecekti. Django'yu doğru olana yönlendirebilmemiz lazım ve bunları sağlamak için en kolay yol onları isimlendirmektir. Yani, bu durgun dosyaları uygulamanın kendisi için adlandırılan başka bir dizine koymaktır.

</div>

  Biçem sayfasında aşağıdaki kodu ekleyin:

  anketler/static/anketler/style.css
  ```css
    li a {
      color: green;
    }
  ```

  Sonra, anketler/templates/anketler/index.html'in başına aşağıdakileri ekleyin:

  anketler/templates/anketler/index.html
  ```html
    {% load static %}

    <link rel="stylesheet" type="text/css" href="{% static 'anketler/style.css' %}" />
  ```

  {% static %} şablon etiketi, durgun dosyaların mutlak URL'lerini üretir.

  Geliştirme için yapmanız gereken tek şey bu. Yeniden yükle http://localhost:8000/anketler/ ve soru bağlantılarının yeşil (Django biçemi!) olduğunu görmelisiniz. Bu da biçem sayfanızın düzgün yüklendiği anlamına gelir.

  <hr>

## Arka plan resmi ekleme

  Ardından, resimler için bir alt dizin oluşturacağız. anketler/static/anketler/ dizininde bir görseller alt dizini oluşturun. Bu dizin içine arkaplan.png adlı bir resim koyun. Başka bir deyişle, resminizi anketler/static/anketler/images/arkaplan.png dosyasına koyun.

  Ardından, biçem sayfanıza ekleyin:

  anketler/static/anketler/style.css
  ```css
    body {
      background: white url("images/arkaplan.png") no-repeat right bottom;
    }
  ```
  http://localhost:8000/anketler/ tarayıcıda yeniden yükleyin. Sağ alt kısmında yüklü arkaplan resmini görmelisiniz.

  <div data-bilget="uyarı">

### Uyarı

  Elbette {% static %} şablon etiketi, biçem sayfanız gibi Django tarafından üretilmeyen durgun dosyalarda kullanılamaz. Durgun dosyalarınızı birbirine bağlamak için her an göreli yolları kullanmalısınız, zira static dosyalarınızdaki bir sürü yolu değiştirmeden STATIC_URL'yi (URL'lerini oluşturmak için durgun şablon etiketi tarafından kullanılır.) değiştirebilirsiniz.

  </div>

  Bunların temelleri, Ayarlarla ve çerçeveyle birlikte gelen diğer bitlerle ilgili daha fazla bilgi için [durgun dosyalar](#)a ve [staticfiles kaynakçası](#)na bakın. [Durgun dosyaları dağıtmak](#), durgun dosyaların gerçek bir sunucuda nasıl kullanılacağını anlatır.

  Durgun dosyalardan memnun olduğunuzda, Django'nun doğal oalrak oluşturulan yönetici sitesini nasıl özelleştireceğinizi öğrenmek için [Öğretici 7]({{site.belgeler_ogretici7}}) bölümünü okuyun.

</div>
