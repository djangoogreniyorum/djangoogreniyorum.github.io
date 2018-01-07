---
layout: general
title: Öğretici 1 - Django Öğreniyorum
---
# İlk Django Uygulamanızı Yazma, Bölüm 1

Örnekle öğrenelim.
Temel bir anket uygulamasının oluşturulması konusunu bu ders boyunca işleyeceğiz.
İki kısımdan oluşacak:

- Herkesin anketlere katılmasını ve oy vermesini sağlayan bir site
- Anketler eklemenize, değiştirmenize ve silmenize izin veren bir yönetici sitesi.

Zaten [Django'nun kurulu]({{site.belgeler_kurulum}}) olduğunu varsayıyoruz. Django'nun kurulu olduğunu söyleyebilirsiniz ve kabuk isteminde hangi sürümün çalıştırıldığını $ önekiyle belirtilen yere aşağıdaki komutu yazarak görebilirsiniz.

<pre data-gnl="1 1p"><code class="language-python">
$ python -m django --version

</code></pre>

Django yüklüyse, kurulumunuzun sürümünü görmelisiniz. Değilse, "No module named django" diye bir hata mesajı alırsınız.

Bu öğretici Python 3.4 ve sonrasını destekleyen Django 2.0 için yazılmıştır. Django sürümü eşleşmiyorsa, bu sayfanın sağ alt köşesindeki sürüm değiştiriciyi kullanarak Django sürümüne ilişkin öğreticiye gidebilir veya Django'yu en yeni sürüme güncelleyebilirsiniz. Python'un daha eski bir sürümünü kullanıyosanız, "<a href="#">Django ile hangi Python sürümünü kullanabilirim?</a>" konusuna bakarak uyumlu sürümü bulabilirsiniz.

Django'nun eski sürümlerini kaldırma ve yeni bir tane kurma konusunda <a href="{{site.belgeler_kurulum}}">Django'nun nasıl kurulacağı</a> konusuna bakın.

<div data-bilget="genel" markdown="1">
Yardım alınabilecek yer:

Bu öğreticide sorun yaşıyorsanız, lütfen [Django Öğreniyorum grubuna](https://www.facebook.com/groups/1574377365924729/) mesaj gönderin veya yardımcı olabilecek diğer Django kullanıcılarıyla iletişime geçmek için [#django imi ile irc.freenode.net](irc://irc.freenode.net/django) adresine gidin.

</div>

## Proje oluşturma

Django'yu ilk defa kullanıyorsanız, bazı ilk kurulumlara dikkat etmeni gerekir. Yani, bir Django projesi veritabanı yapılandırması, Django'ya özgü seçenekler ve uygulamaya özel ayarlar dahil olmak üzere Django örneği için bir ayarlar kümesi oluşturan bazı kodları doğallıkla üretmeniz gerekecek.

Komut satırından cd emrini proje dosyalarınızın bulunmasını istediğiniz dizin için kullanın ve aşağıdaki komutu çalıştırın.

<pre data-gnl="1 1p"><code class="language-python">
$ django-admin startproject benimsite

</code></pre>

Geçerli dizinde "benimsite" dizini oluşturacaktır. İşe yaramazsa, bkz. [Django-admin sorunlu çalışmalar](#)
<div data-bilget="genel" markdown="1">
### Not

Yerleşik Python veya Django bileşenleri sonrasında projeleri adlandırmaktan kaçınmanız gerekir. Özellikle, django veya test gibi yerleşik bir Python paketiyle çakışabilecek isimleri kullanmamanız gerektiği anlamına gelir.
</div>
<div data-bilget="genel" markdown="1">
###  Bu kod nerede yaşayacak?

Arka planınız düz eski PHP (günümüz çatıları kullanmadan) ise, büyük olasılıkla ağ sunucusunun belge kökü altına (/var/www gibi bir yere) kod koymak için kullanırsınız. Django ile ise bunu yapamazsın. Bu Python kodundan herhangi birini ağ sunucunuzun belge kökü içine koymak iyi bir fikir değil. Çünkü insanların kodunuzu ağ üzerinden görüntüleyebilecekleri riskini taşıyor. Bu güvenlik açısından iyi değil.

Bu nedenle kodunuzu (/home/benimkodum gibi) belge kökü dışında bir dizine yerleştirin.
</div>

startproject komutunun oluşturduğu şeylere bir göz atalım:
<pre data-gnl="1 1p"><code class="language-python">
benimsite/
    manage.py
    benimsite/
        __init__.py
        settings.py
        urls.py
        wsgi.py

</code></pre>

Bu dosyalar:

- benimsite/: kök dizinidir. Yalnızca projeniz için kapsayıcı dizindir. Adının ne olduğu Django için önemli değildir; yeniden adlandırmada sorun yaşamazsınız.
- benimsite/manage.py: Django projenizle çeşitli şekillerde etkileşimde bulunmanıza izin veren bir komut satırı yardımcı programıdır. Manage.py ile ilgili tüm ayrıntıları [django-admin ve manage.py](#) bölümlerinde okuyabilirsiniz.
- benimsite/benimsite/: projenizin öz Python paketidir. Adı, içindeki herhangi bir içeriği içe aktarmak için kullanmanız gereken Python paketinin adıdır. Örnek kullanım: benimsite.urls
- benimsite/benimsite/__init__.py: Bu dizinin bir Python paketi olduğu düşüncesiyle Python'a bildirilen boş bir dosya. Bir Python acemi kullanıcısıysanız, [resmi Python belgelerindeki paketler hakkında](https://docs.python.org/3/tutorial/modules.html#tut-packages) daha fazla bilgi edinin.
- benimsite/benimsite/settings.py: Django projesi için ayarlar yapılandırması yapılır. [Django ayarları](#) belgesi ayarların size nasıl çalıştığını gösterecektir.
- benimsite/benimsite/urls.py: Django projesinin URL bildirimleridir; Django destekli sitenizin "içindekiler tablosu". [URL gönderim](#) programındaki URL'lerle ilgili daha fazla bilgiyi okuyabilirsiniz.
- benimsite/benimsite/wsgi.py: WSGI uyumlu ağ sunucuları için projenize hizmet edecek bir giriş noktası. Daha fazla bilgi için [WSGI](#) ile nasıl dağıtılacağına bakın.

## Geliştirme sunucusu

Django projenizin çalıştığını doğrulamamız gerekmekte. O halde doğrulayalım. Henüz yapmadıysanız benimsite kök dizinine geçin ve aşağıdaki komutları çalıştırın:

<pre data-gnl="1 1p"><code class="language-python">
$ python manage.py runserver

</code></pre>

Süreç devamında aşağıdaki çıktıyı komut satırında göreceksiniz
<pre data-gnl="1 1p"><code class="language-python">
  Performing system checks...

  System check identified no issues (0 silenced).

  You have unapplied migrations; your app may not work properly until they are applied.
  Run 'python manage.py migrate' to apply them.

  December 27, 2017 - 15:50:53
  Django version 2.0, using settings 'mysite.settings'
  Starting development server at http://127.0.0.1:8000/
  Quit the server with CONTROL-C.
</code></pre>

<div data-bilget="genel" markdown="1">
### Not

Şu an için uygulanmayan veritabanına taşıma işlemleri ile ilgili uyarıyı görmezden gelin; kısa süre içerisinde o uyarıyı düzeltmek için veritabanı işlemlerini yapacağız.
</div>

Yalnızca Python'da yazılmış olan hafif bir ağ sunucu olan Django geliştirme sunucusunu başlattınız. Bunu üretime hazır oluncaya kadar Apache gibi bir üretim sunucusunun yapılandırılmasıyla uğraşmak zorunda kalmadan hızla geliştirebilmeniz için Django'ya ekledik.

Şimdi dikkat edilmesi gereken güzel bir an: Bu sunucuya bir üretim ortamına benzeyen herhangi bir şeyde kullanmayın. Geliştirilmesi sırasında sadece kullanım için tasarlanmıştır. Amacımız sunucu yapmak değil, çatı yapmak.

Artık sunucu çalışıyor, ağ tarayıcınızla (firefox, chrome, edge, opera vb...) http://127.0.0.1:8000/ adresini ziyaret edin. Karşınıza "Tebrikler!" diye çıkan karşılama sayfasını göreceksiniz.

<div data-bilget="genel" markdown="1">
### Bağlantı noktasını değiştirme
Varsayılan olarak runserver komutu 8000 numaralı bağlantı noktasındaki dahili IP adresinde geliştirme sunucusunu başlatır.

Sunucunun bağlantı noktasını değiştirmek istiyorsanız bunu bir komut satırı değişkeni olarak iletin. Örneğin bu komut sunucuyu 8080 numaralı bağlantı noktasından başlatır:

  <pre data-gnl="1 1p"><code class="language-python">
$ python manage.py runserver 8080

  </code></pre>

Sunucun IP'sini değiştirmek isterseniz, geçit noktasının yanında belirtin. Örneğin, tüm mevcut ortak IP'leri dinlemek için (Vagrant çalıştırıyorsanız veya işinizi ağdaki diğer bilgisayarlarda göstermek istiyorsanız kullanışlıdır) kullanmak için için şunları yazın:

  <pre data-gnl="1 1p"><code class="language-python">
python manage.py runserver 0:8000

  </code></pre>

0 değeri 0.0.0.0 için kısayoldur. Geliştirme sunucusu için tam belgeler <a href="#">runserver</a> kaynakçasında bulunabilir.
</div>

<div data-bilget="genel" markdown="1">
### runserver'ın doğal olarak yeniden yüklenmesi

Geliştirme sunucusu, her istekte Python kodunu otomatik olarak yeniden yükler. Kod değişikliklerinin etkili olması için sunucuyu yeniden başlatmanız gerekemz. Bununla birlikte, dosya ekleme gibi bazı işlemler yeniden başlatma tetiklemez, bu nedenle bu durumlarda sunucuyu yeniden elle başlatmanız gerekir. Çünkü doğal olarak yenilenmeyecektir.
</div>

## Anket uygulamasını oluşturma

Artık bir "proje" olan kapsayıcınız kuruldu, işinizi yapmaya başladınız demektir.

Django'da yazdığınız her uygulama, belirli bir kurala uyan bir Python paketinden oluşur. Django, bir uygulamanın temel dizin yapısını doğal olarak üreten bir yardımcı programla birlikte gelir. Böylece dizinleri oluşturmak yerine kod yazmaya odaklanabilirsiniz.

<div data-bilget="genel" markdown="1">
### Proje ks uygulama

Bir proje ile bir uygulama arasındaki fark nedir? Bir uygulama, bir şey yapan ağ uygulamasıdır. Örneğin, ağ günlüğü örgüsü, genel kayıtların bir veritabanı veya basit bir anket uygulaması. Bir proje ise belirli bir ağ sitesi için yapılandırma ve uygulama topluluğunu kapsamaktır. Bir proje birden fazla uygulama içerebilir. Bir uygulama birden fazla projede olabilir.
</div>

Uygulamalarınız Python yolunuzun herhangi bir yerinde yaşayabilir. Bu yazıda, anket uygulamamızı manage.py dosyanızın hemen yanında oluşturacağız, böylece benimsite'nin bir alt eklentisi yerine kendi üst düzey eklentisi olarak içe aktarılabilecek.

Uygulamanızı oluşturmak için manage.py ile aynı dizinde olduğunuzdan emin olun ve şu komutu yazın:
<pre data-gnl="1 1p"><code class="language-python">
$ python manage.py startapp anketler
</code></pre>

Bu, şu şekilde düzenlenmiş bir dizin anketleri oluşturacaktır:
<pre data-gnl="1 1p"><code class="language-python">
  anketler/
      __init__.py
      admin.py
      apps.py
      migrations/
          __init__.py
      models.py
      tests.py
      views.py

</code></pre>

Bu dizin yapısı anket uygulamasını barındıracaktır.

<hr>

## İlk görünümü yazın (views.py)

anketler/views.py dosyasını açın ve aşağıdaki Python kodunu buraya yerleştirin:
benimsite/anketler/views.py

<pre data-gnl="1 1p"><code class="language-python">
from django.http import HttpResponse


def index(request):
    return HttpResponse("Merhaba dünya. Anket sayfasındasın.")

</code></pre>

Bu, Django'da mümkün olan en basit görünümdür. Görünümü çağırmak için onu bir URL'ye eşlemeliyiz ve bunun için bir URLconf'a ihtiyacımız var.

Yoklamalar dizininde bir URLconf oluşturmak için urls.py adlı bir dosya oluşturun.

Uygulama dizininiz şu şekilde görünmelidir.
<pre data-gnl="1 1p"><code class="language-python">
  anketler/
      __init__.py
      admin.py
      apps.py
      migrations/
          __init__.py
      models.py
      tests.py
      urls.py
      views.py

</code></pre>

Şimdi anketler/urls.py dosyasına aşağıdaki kodları dahil edin:

benimsite/anketler/urls.py
<pre data-gnl="1 1p"><code class="language-python">
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
</code></pre>

Bir sonraki adım, anketler.urls eklentisindeki kök URLconf'u göstermektir. benimsite/urls.py dosyasında django.urls.include için bir içe aktarma ekleyin ve urlpatterns listesine bir include() ekleyin, böylece şunları yapabilirsiniz:

benimsite/benimsite/urls.py
<pre data-gnl="1 1p"><code class="language-python">
from django.urls import include, path
from django.contrib import admin

urlpatterns = [
    path('anketler/', include('anketler.urls')),
    path('admin/', admin.site.urls),
]

</code></pre>

include() işlevi, diğer URLconf'ların kaynakça edilmesini sağlar. Django, include() ile karşılaştığında, o noktaya kadar eşleşen URL'nin herhangi bir bölümünü keser ve kalan dizeyi daha sonraki işlemler için URLconf'a gönderir.

inclue() ögesinin arkasındaki fikir tak ve çalıştırdır. URL'leri kolaylaştırmaktır. Anketler kendi URLconf'larında (anketler/urls.py) bulunduklarından "/anketler/" ya da "/eglenceli_anketler/" ya da "/icerik/anketler/" ya da diğer herhangi bir yol kökü altına yerleştirilebilir. Uygulama sorunsuz çalışmaya devam eder.

<div data-bilget="genel" markdown="1">
### include() ne zaman kullanılır?

Diğer URL kalıplarını eklerken include() işlevini kullanmalısınız. Bunun tek istisnası admin.site.urls'dir.
</div>

URLconf'a bir dizin görüntüsü bağladınız. Çalıştığını doğrulamasına izin verin, aşağıdaki komutu çalıştırın:

<pre data-gnl="1 1p"><code class="language-python">
$ python manage.py runserver

</code></pre>

Tarayıcınızda http://localhost:8000/anketler/ adresine gidin ve "Merhaba dünya. Anket sayfasındasınız." görünümde tanımladığınız metni görün.

[path()](#) işlevi dört bağımsız değişkenden geçirilir. İki tane gereklidir: rota ve görünüm (route ve view). Diğer ikisi ise isteğe bağlıdır: kwargs ve name. Bu noktada, bu değiştirgelerin ne için olduğunu incelemek gerekir.

<hr>

## [path()](#) konusu: rota (route)

route, bir URL kalıbı içeren dizedir. Bir isteği işlerken, Django urlpatterns'deki ilk desenden başlar ve istenilen URL'yi her desene eşleyene kadar karşılaştırarak listeden aşğı doğru yol alır.

Desenler GET ve POST değiştirgelerini veya etki alanı adını aramaz. Örneğin, https://www.örnek.com/benimuygulama/ adresine yapılan bir istekte, URLconf benimuygulama/ ögesini arar. https://www.örnek.com/benimuygulama/?sayfa=3 adresine bir istekte bulunursa, URLconf yine benimuygulama/ adresini arar.

<hr>

## [path()](#) konusu: görünüm (view)

Django eşleşen bir desen bulduğu zaman, belirtilen görüntüleme işlevini bir HttpRequest (Httpİstek) nesenesi ile ilk konu olarak çağırır ve anahtar sözcükleri olarak route (rota) daki yakalanmış tüm değerleri de. Buna biraz örnek vereceğiz.

<hr>

## [path()](#) konusu: kwargs

Keyifli anahtar sözcük konuları bir sözlükte hedef görünümde geçirilebilir. Öğreticide Django'nun bu özelliğini kullanmayacağız.

<hr>

## [path()](#) konusu: isim (name)

URL'nizi adlandırmak, Django'nun başka yerlerinden, özellikle de şablonlardan ayırt edici bir şekilde başvurmanıza izin verir. Bu güçlü özellik, yalnızca tek bir doyaya dokunurken projenizin URL kalıplarında genel değişiklikler yapmanıza olanak tanır.

Temek istek ve yanıt akışından memnun kaldığınızda, veritabanıyla çalışmaya başlamak için bu [öğreticinin 2. bölümü](/en/20/intro/tutorial02/)nü okuyun.
