---
layout: general
title: Bir Bakışta Django - Django Öğreniyorum
---
# Bir Bakışta Django

Django, hızlı bir haber odası ortamında geliştirildiğinden, ortak ağ geliştirme görevlerinih ızlı ve kolay kapmak için tasarlandı. Django ile veritabanı odaklı bir ağ uygulaması yazmanın nasıl yapıldığına dair gayri resmi bir genel bakış atalım.

Bu belgenin amacı, Django'nun nasıl çalıştığını anlamak için yeterli teknik ayrıntıları vermektir. Ancak bu bir ders veya kaynak olarak tasarlanmamıştır. Bir projeye başlamaya hazır olduğunuzda, [öğreticiyle](/en/2.0/intro/tutorial01/) başlayabilir veya [daha ayrıntılı belgelere](/en/2.0/topics/) dalabilirsiniz.

## Kalıbınızı Tasarlayın

Django'yu bir veritabanı olmadan kullanabilmenize rağman, veritabanı düzeninizi Python kodunda tanımladığınız bir nesne-ilişkisel eşlemeyle birlikte gelir.

[Veri-kalıbı sözdizimi](/en/2.0/topics/db/models/), kalıplarınızı göstermek için birçok zengin yol sunar. Şu ana kadar, yıllarca süren veritabanı şeması sorunlarını çözmüştür. İşte hızlı bir örnek:

benimsite/haberler/models.py
<pre data-gnl="1 1p"><code class="language-python">
from django.db import models

class Haberci(models.Model):
  tam_adi = models.CharField(max_length=70)

  def __str__(self):
      return self.tam_adi

class Makale(models.Model):
  yayim_tarihi = models.DateField()
  baslik = models.CharField(max_length=200)
  icerik = models.TextField()
  haberci = models.ForeignKey(Haberci, on_delete=models.CASCADE)

  def __str__(self):
      return self.baslik
  </code></pre>
  <hr>
## Kurulumu Yap
  Şimdi, veritabanı tablolarını doğal olarak oluşturmak için Django komut satırı yardımcı programını çalıştırın:
  <pre data-gnl="1 1p"><code class="language-python">
$ python manage.py migrate

</code></pre>

Göç komutu (migrate) mevcut tüm modellerinize bakar ve halihazırda var olmayan tablolar için veritabanınızda tablolar oluşturur ve isteğe bağlı olarak daha [zengin şema kontrolü](/en/2.0/topics/migrations/) sağlanır.

<hr>

## Ücretsiz API'nin tadını çıkarın

Bununla, verilerinize erişmek için özgür ve zengin bir [Python API](/en/2.0/topics/db/queries/)'si var. API anında oluşturulur, kod üretimi gerekli değildir:

<pre data-gnl="1 1p"><code class="language-python">
  # Oluşturduğumuz modelleri &quot;haber&quot; uygulamamızdan i&ccedil;e aktarın
  &gt;&gt;&gt; from news.models import Haberci, Makale

  # Haberciler hen&uuml;z &ouml;rg&uuml;de değiller.
  &gt;&gt;&gt; Haberci.objects.all()
  &lt;QuerySet []&gt;

  # Yeni bir Haberci oluştur
  &gt;&gt;&gt; r = Haberci(tam_adi='&Ccedil;apanoğlu Ag&acirc;h Efendi')

  # Nesneyi veritabanına kaydedin. save() &ouml;gesini eklem olarak &ccedil;ağırmalısınız.
  &gt;&gt;&gt; r.save()

  # Şimdi bir kimliği (ID) var. id &ouml;gesini eklem olarak &ccedil;ağırıp g&ouml;rebilirsiniz.
  &gt;&gt;&gt; r.id
  1

  # Şimdi yeni haberci veritabanında.
  &gt;&gt;&gt; Haberci.objects.all()
  &lt;QuerySet [&lt;Haberci: &Ccedil;apanoğlu Ag&acirc;h Efendi&gt;]&gt;

  # Alanlar, Python nesnesinde &ouml;znitelikler olarak temsil edilir. tam_adi &ouml;zniteliği &ouml;gesini eklem olarak g&ouml;sterek &ccedil;ağırın.
  &gt;&gt;&gt; r.tam_adi
  '&Ccedil;apanoğlu Ag&acirc;h Efendi'

  # Django zengin bir veritabanı arama API'si sağlar.
  &gt;&gt;&gt; Haberci.objects.get(id=1)
  &lt;Haberci: &Ccedil;apanoğlu Ag&acirc;h Efendi&gt;
  &gt;&gt;&gt; Haberci.objects.get(tam_adi__startswith='&Ccedil;apanoğlu')
  &lt;Haberci: &Ccedil;apanoğlu Ag&acirc;h Efendi&gt;
  &gt;&gt;&gt; Haberci.objects.get(tam_adi__contains='pano')
  &lt;Haberci: &Ccedil;apanoğlu Ag&acirc;h Efendi&gt;
  &gt;&gt;&gt; Haberci.objects.get(id=2)
  Traceback (most recent call last):
      ...
  DoesNotExist: Reporter matching query does not exist.

  # Makale oluştur.
  &gt;&gt;&gt; from datetime import date
  &gt;&gt;&gt; a = Makale(yayim_tarihi=date.today(), baslik='Django harika',
  ...     icerik='Ivır zıvır.', haberci=r)
  &gt;&gt;&gt; a.save()

  # Şimdi makale veritabanında.
  &gt;&gt;&gt; Makale.objects.all()
  &lt;QuerySet [&lt;Makale: Django harika&gt;]&gt;

  # Makale nesneleri, ilgili Haberci nesnelerine API erişimi sağlar.
  &gt;&gt;&gt; r = a.haberci
  &gt;&gt;&gt; r.tam_adi
  '&Ccedil;apanoğlu Ag&acirc;h Efendi'

  # Ve tam tersi: Haberci nesneleri Makale nesnelerine API erişimi sağlar.
  &gt;&gt;&gt; r.makale_set.all()
  &lt;QuerySet [&lt;Makale: Django harika&gt;]&gt;

  # API, ilişkileri ihtiyacınız olan kadarıyla izler ve verimli bir şekilde ger&ccedil;ekleştirir.
  # Sahnelerin arkasında sizin i&ccedil;in birleşir.
  # Bu, &quot;&Ccedil;apanoğlu&quot; ile başlayan bir haberci tanımlı t&uuml;m makaleleri bulur.
  &gt;&gt;&gt; Makale.objects.filter(haberci__tam_adi__startswith='&Ccedil;apanoğlu')
  &lt;QuerySet [&lt;Makale: Django harika&gt;]&gt;

  # Bir nesneyi niteliklerini değiştirerek ve save() y&ouml;ntemini kullanarak değiştirin.
  &gt;&gt;&gt; r.tam_adi = 'Şinasi'
  &gt;&gt;&gt; r.save()

  # Bir nesneyi delete() eklemi ile silebilirsiniz.
  &gt;&gt;&gt; r.delete()
</code></pre>

## Dinamik bir yönetici arayüzü: sadece iskele değil, tüm ev var.

Django, kalıplarınızı tanımlandıktan sonra kimliği doğrulanmış kullanıcıların nesneleri ekleme, değiştirme ve silme olanağı veren bir ağ sitesi olan, usta, üretime hazır bir [yönetici arabirimi](/en/2.0/ref/contrib/admin/) doğal olarak oluşturabilir. Kalıbınızı yönetici sitesine kaydetmek kadar kolaydır:

benimsite/haberler/models.py
<pre data-gnl="1 1p"><code class="language-python">
from django.db import models

class Makale(models.Model):
    yayim_tarihi = models.DateField()
    baslik = models.CharField(max_length=200)
    icerik = models.TextField()
    haberci = models.ForeignKey(Haberci, on_delete=models.CASCADE)
  </code></pre>
  benimsite/haberler/admin.py
  <pre data-gnl="1 1p"><code class="language-python">
from django.contrib import admin

from . import models

admin.site.register(models.Makale)
</code></pre>

Buradaki felsefe, sitenizin bir görevli, bir müşteri veya belki de sadece sizin tarafından düzenlenmesidir ve yalnızca içeriği yönetmek için arka uç arabirimleri oluşturmakla uğraşma zorunda kalmak istemezsiniz.

Django uygulamalarını oluştururken kullanılan tipik bir iş akışı, modeller oluşturmak ve yönetici sitelerini olabildiğince hızlı çalıştırmaktır; böylece görevlileriniz (veya müşterileriniz, ziyaretçileriniz, üyeleriniz) veri toplamaya başlayabilir. Ardından, verilerin halka sunulma biçimini geliştirin.

<hr>

## URLleri tasarlayın

Temiz, şık bir URL şeması, yüksek kaliteli bir ağ uygulamasında önemli bir ayrıntıdır. Django güzel bir URL tasarımını teşvik eder ve .php veya .asp gibi URL'lerde herhangi bir hata yapmaz.

Bir uygulama için URL'ler tasarlamak için bir [URLconf](/en/2.0/topics/http/urls/) adlı Python eklentisi oluşturursunuz. Uygulamanız için içindekiler tablosu, URL kalıpları ile Python geri arama işlevleri arasında basit bir imgeleme içerir. URLconf'lar, URL'leri Python kodundan ayırmak için de kullanılır. Yukarıdaki Haberci / Makale örneği için URLconf nasıl olacağını aşağıda görebilirsiniz:

benimsite/haberler/urls.py
<pre data-gnl="1 1p"><code class="language-python">
from django.urls import path

from . import views

urlpatterns = [
    path('makaleler/&lt;int:yil&gt;/', views.yillik_arsiv),
    path('makaleler/&lt;int:yil&gt;/&lt;int:ay&gt;/', views.aylik_arsiv),
    path('makaleler/&lt;int:yil&gt;/&lt;int:ay&gt;/&lt;int:pk&gt;/', views.makale_ayrintisi),
]
  </code></pre>

Yukarıdaki kod, URL yollarını Python geri arama işlevlerine ("views") eşleştirir. Yol dizeleri, URL'lerden değerleri "yakalama" için değiştirge etiketleri kullanır. Bir kullanıcı bir sayfayı istediğinde, Django her yoldan sırayla geçer ve istenen URL ile eşleşen ilk satırda durur. (Hiçbiri eşleşmezse, Django özel durum 404 görünümü çağırır.) Yollar, yükleme zamanında normal ifadeler halinde derlendiğinden, çok hızlıdır.

URL kalıpları eşleştiğinde, Django, Python işlevi olan belirli görünümü çağırır. Her görünüm ('views') bir istek nesnesi tarafından iletilir - talep meta verileri içerir - ve desende yaklanan değerler de.

Örneğin, bir kullanıcı "/makaleler/2005/05/39323" URL'sini isterse, Django haberler.views.makale_ayrintisi(request, year=2005, ay=5, pk=39323) işlevini çağırır.

<hr>

## Görünümleri yaz (views)

Her görünüm, iki şeyden birini yapmaktan sorumludur: İstenen sayfa için içeriği içeren bir [HttpResonse](/en/2.0/ref/request-response/#django.http.HttpResponse) nesenesini döndürme veya [Http404](/en/2.0/topics/http/views/#django.http.Http404) gibi bir istisna yükseltere. Gerisi size kalmış.

Genellikle, bir görünüm değiştirgelere göre veri alır, bir şablon yükler ve şablonu alınan verilerle işler. Yukarıdaki yillik_arsiv için bir örnek görüntü var:

benimsite/haberler/views.py
<pre data-gnl="1 1p"><code class="language-python">
from django.shortcuts import render

from .models import Makale

def yillik_arsiv(request, yil):
    a_listesi = Makale.objects.filter(yayim_tarihi__year=yil)
    baglam = {'yil': yil, 'makale_listesi': a_listesi}
    return render(request, 'haberler/yillik_arsiv.html', baglam)

</code></pre>

Bu örnek, birkaç güçlü özelliklere sahip olan Django [şablon örgüsünü](/en/2.0/topics/templates/) kullanmaktadır; ancak programcı olmayanların kullanması için yeterince basit kalmaya çabalamaktadır.

<hr>

## Şablonlarınızı tasarlayın

Yukarıdaki kod, haberler/yillik_arsiv.html şablonunu yükler.

Django, şablonlar arasında fazlalığı azaltmanızı sağlayan bir şablon arama yoluna sahiptir. Django ayarlarınızda, [DIRS](/en/2.0/ref/settings/#std:setting-TEMPLATES-DIRS) ile şablonları denetlemek için dizinlerin bir listesini belirtirsiniz. İlk dizinde bir şablon yoksa, ikinci dizini denetler, vb.

Diyelim ki haberler/yillik_arsiv.html şablon bulundu. İşte bunun gibi görünebilir:

benimsite/haberler/templates/haberler/yillik_arsiv.html

<pre data-gnl="1 1p"><code class="language-html">
{&#37; extends &quot;temel.html&quot; &#37;}

{&#37; block title &#37;}&#123;&#123; yil &#125;&#125; i&ccedil;in makaleler{&#37; endblock &#37;}

{&#37; block content &#37;}
&lt;h1&gt;&#123;&#123; yil &#125;&#125; i&ccedil;in makaleler&lt;/h1&gt;

{&#37; for makale in makale_listesi &#37;}
    &lt;p&gt;&#123;&#123; makale.baslik &#125;&#125;&lt;/p&gt;
    &lt;p&gt;&#123;&#123; makale.haberci.tam_adi &#125;&#125; tarafından&lt;/p&gt;
    &lt;p&gt;Yayımlanma &#123;&#123; makale.yayim_tarihi|date:&quot;F j, Y&quot; &#125;&#125;&lt;/p&gt;
{&#37; endfor &#37;}
{&#37; endblock &#37;}

</code></pre>

Değişkenler çift kıvrımlı parantezlerle çevrilidir. **{{makale.baslik}}**, "Makale başlığı özniteliğinin değerini verin" anlamına gelir. Ancak noktalar yalnızca özellik taramasıiçin kullanılmaz. Ayrıca sözlük anahtarlı arama, dizin arama ve işlev çağrıları yapabilirler.

Not: **{{makale.yayim_tarihi | date:"F j, Y"}}** bir Unix tarzı "boru" yani "|" karakterini kullanır. Buna şablon süzgeci denir ve bir değişkenin değerini süzmek için bir yöntemdir. Bu durumda tarih süzgeci bir Python datetime nesnesini biçim olarak biçimler (PHP date işlevinde olduğu gibi)

Birlikte istediğiniz kadar süzgeç zinciri yapabilirsiniz. [Özel şablon süzgeçleri](/en/2.0/howto/custom-template-tags/#howto-writing-custom-template-filters) yazabilirsiniz. Sahnelerin arkasında özel Python kodunu çalıştıran [özel şablon etiketleri](/en/2.0/howto/custom-template-tags/) yazabilirsiniz.

Son olarak, Django "şablon kalıtımı" kavramını kullanmaktadır. **{&#37; Extends "temel.html" &#37;}** ne işe yarar? İlk önce bir blok yığını tanımlayan "temel" adındaki şablonu yükleyin ve blogkları aşağıdaki bloklarla doldurun anlamına gelir. Kısacası, şablonlarda fazlalıkları önemli ölçüde azaltabilirsiniz. Her şablon yalnızca ilgili şablon için benzersi olanları tanımlamalıdır.

İşe sabit dosyaların kullanımı da dahil olmak üzere "temel.html" şablonu şöyle görünebilir.

<pre data-gnl="1 1p"><code class="language-html">
{&#37;load static &#37;}
&lt;html&gt;
&lt;head&gt;
    &lt;title&gt;{&#37;block title &#37;}{&#37;endblock &#37;}&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;img src=&quot;{&#37;static &quot;images/sitelogo.png&quot; &#37;}&quot; alt=&quot;Logo&quot; /&gt;
    {&#37;block content &#37;}{&#37;endblock &#37;}
&lt;/body&gt;
&lt;/html&gt;

</code></pre>

Basitçe, sitenin görünümünü ve şeklini (sitenin logosuyla birlikte) tanımlar ve doldurulması için iç şablonlara "yuvalar" sağlar. Bu, bir siteyi tek bir dosyayı değiştirme kadar kolaylıkla yeniden tasarlamaktır.

Ayrıca, iç şablonlarını yeniden kullanırken, farklı temel şablonlarla bir sitenin birden çok sürümünü oluşturmanıza olanak tanır. Django'nun yaratıcıları, sitenin çarpıcı biçimde farklı telefon sürümlerini oluşturmak için tekniği basitçe yeni bir temel şablon oluşturarak kullandı.

Başka bir örgüyü tercih ederseniz, Django'nun şablon görügüsünü kullanmak zorunda kalmadığınızı unutmayın. Django'nun şablon örgüsü Django'nun model katmanı ile özellikle iyi gömülü olsa da, hiçbir şey onu kullanmaya zorlamaz. Bu konuda Django'nun veritabanı API'sini kullanmak zorunda değilsiniz. Başka bir veritabanı soyutlama katmanı kullanabilirsiniz, XML dosyalarını okuyabilir, dosyaları diskten okuyabilir veya istediğiniz herhangi bir şey okuyabilirsiniz. Django'nun her bir parçası (modeller, görünümler, şablonlar) diğerlerinden ayrılır.

<hr>

## Bu sadece yüzey

Bu sadece Django'nun işlevselliği hakkında hızlı bir genel bakış olmuştur. Bazı kullanışlı özellikler şöyle:

- Memcached veya diğer arka uçlarla bütünleşen bir [önbellekleme çerçevesi](/en/2.0/topics/cache/).
- RSS ve Atom oluşturma işlemini küçük bir Python sınıfı yazmak kadar kolaylaştıran bir [sendikasyon çerçevesi](/en/2.0/ref/contrib/syndication/).
- Doğal olarak oluşturulan daha çekici doğal yönetici özellikleri

Bir sonraki belirgin adımlar [Django'yu indirmek](/download/), [dersleri okumak](/en/2.0/intro/tutorial01/), [topluluğa katılmak](/community/) içindir. İlginiz için teşekkürler.
