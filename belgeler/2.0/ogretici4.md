---
layout: general
title: Öğretici 4 - Django Öğreniyorum
---
# İlk Django Uygulamanızı Yazma, Bölüm 4

Bu sayfa <a href="{{site.belgeler_ogretici3}}">Öğretici 3</a>'ün kaldığı yerden devam ediyor. Anket uygulamasına devam ediyoruz ve basit biçim işleme ve kodumuzu kesme üzerine odaklanacağız.

## Basit bir biçim yazın

Şablonun bir HTML **&lt;form&gt;** öğesi içerdiği şekilde, son öğreticide geldiğimiz aşamadaki anket ayrıntısı şablonunu güncelleyelim. ('anketler/ayrinti.html'):

anketler/templates/anketler/ayrinti.html

<pre data-gnl="1 1p"><code class="language-python">
  &lt;h1&gt;{&#123; soru.soru_metni 	&#125;}&lt;/h1&gt;

  {&#37; if hata_mesaji &#37;}&lt;p&gt;&lt;strong&gt;{&#123; hata_mesaji 	&#125;}&lt;/strong&gt;&lt;/p&gt;{&#37; endif &#37;}

  &lt;form action=&quot;{&#37; url 'anketler:oy' soru.id &#37;}&quot; method=&quot;post&quot;&gt;
  {&#37; csrf_token &#37;}
  {&#37; for secim in soru.secim_set.all &#37;}
      &lt;input type=&quot;radio&quot; name=&quot;secim&quot; id=&quot;secim{&#123; forloop.counter 	&#125;}&quot; value=&quot;{&#123; choice.id 	&#125;}&quot; /&gt;
      &lt;label for=&quot;secim{&#123; forloop.counter 	&#125;}&quot;&gt;{&#123; secim.secim_metni 	&#125;}&lt;/label&gt;&lt;br /&gt;
  {&#37; endfor &#37;}
  &lt;input type=&quot;submit&quot; value=&quot;Vote&quot; /&gt;
  &lt;/form&gt;
</code></pre>

Hızlı bir özet:


- Yukarıdaki şablon, her soru seçimi için bir radyo düğmesi görüntüler. Her radyo düğmesinin değeri ilişkili soru seçimi kimliğidir. Her radyo düğmesinin adı "secim" dir. Yani birisi radyo düğmelerinden birini seçip biçimi gönderdiğinde, secim=# nerede # POST verisini gönderir demektir. HTML biçimlerinin yani form etiketinin temel kavramı budur.
- HTML form etiketinin eylemini {&#37; url'anketler:oy' soru.id &#37;} olarak ayarladık ve yöntemide method="post" olarak ayarladık. method="post" (method="get" yerine) kullanılması çok önemlidir. Çünkü bu biçimi gönderme işlemi veri sunucusunda değişiklik yapacaktır. Veri sunucusu tarafında değişiklik yapan bir biçim oluşturduğunuzda, method="post" kullanın. Bu ipucu, Django'ya özgü değildir. Daha iyi bir ağ uygulaması geliştirmek amacıyla yapılır.
- forloop.counter, for etiketinin kaç kez döngüden geçtiğini gösterir.
- Bir POST biçimi oluşturduğumuzdan (veri değiştirme etkisine sahip olabilir), Siteler Arası İstek Hırsızlığı konusunda olağan şartlarda endişelenmeliyiz. Neyse ki endişenmenize gerek yok çünkü Django buna karşı koruma için çok kolay bir örgüyle geliyor. Kısacası, iç URL'leri hedefleyen tüm POST biçimlerinde {&#37; csrf_token &#37;} şablon etiketi kullanılmalıdır.

Şimdi, gönderilen verileri işleyen ve onunla birlikte bir şey yapan bir Django görünümü oluşturalım. [Öğretici 3]({{site.belgeler_ogretici3}})'te, bu satırı içeren yoklama uygulamaları için bir URLconf oluşturduk:

anketler/urls.py
<pre data-gnl="1 1p"><code class="language-python">
  path('&lt;int:soru_id&gt;/oy/', views.oy, name='oy'),

</code></pre>

Ayrıca, oy() işlevinin kukla bir uygulamasını oluşturduk. Hay de, gerçek bir sürüm oluşturalım. anketler/views.py dosyasına aşağıdakileri ekleyin:

anketler/views.py
<pre data-gnl="1 1p"><code class="language-python">
  from django.shortcuts import get_object_or_404, render
  from django.http import HttpResponseRedirect, HttpResponse
  from django.urls import reverse

  from .models import Secim, Soru
  # ...
  def oy(request, soru_id):
      soru = get_object_or_404(Soru, pk=soru_id)
      try:
          secim_secildi = soru.secim_set.get(pk=request.POST['secim'])
      except (KeyError, Secim.DoesNotExist):
          # Soruyu oylama biçimini yeniden görüntüleme.
          return render(request, 'anketler/ayrinti.html', {
              'soru': soru,
              'hata_mesaji': "Herhangi bir seçim yapmadınız.",
          })
      else:
          secim_secildi.oylar += 1
          secim_secildi.save()
          # POST verileri her zmaan bir HttpResponseRedirect döndürür. Bu, bir kullanıcı "geri" düğmesine basarsa verilerin iki kez gönderilmesini önler.
          return HttpResponseRedirect(reverse('anketler:sonuclar', args=(soru.id,)))
          
</code></pre>

Bu kod, bu derste henüz ele almadığımız birkaç şeyi içermektedir:

- **request.POST**, anahtar adına göre gönderilen verilere erişmenizi sağlayan sözdizimi benzeri bir nesnedir. Bu durumda request.POST['secim'], seçilen tercihin kimliğini bir dize olarak döndürür. request.POST değerleri her zaman dizelerdir.

   Django'nun aynı şekilde GET verisine erişmek için request.GET de sağladığını unutmayın. Ancak verilerimizin yalnızca bir POST çağrısı aracılığıyla değiştirilmesini sağlamak için request.POST kodumuzda açıkça kullanıyoruz.

- request.POST['secim'] POST verilerinde seçim yapılmadığı takdirde KeyError değerini yükseltecektir. Yukarıdaki kod KeyError'u denetler ve seçim yapılmazsa soru biçimini bir hata mesajıyla tekrar görüntüler.

- Seçim sayısını arttırdıktan sonra kod, normal bir HttpResponse yerine bir HttpResponseRedirect döndürür. HttpResponseRedirect, tek bir bağımsız değişkeni alır: kullanıcının yönlendirileceği URL (bu durumda URL'yi nasıl oluşturduğumuz konusunda aşağıdaki noktaya bakın).

   Yukarıdaki Python yorumunda dikkat çekildiği gibi, her zaman bir POST verisi ile uğraştıktan sonra HttpResponseRedirect döndürmelidir. Bu ipucu, Django'ya özgü değildir; sadece iyi bir ağ geliştirme adetidir.

- Bu örnekte HttpResponseRedirect yapıcısında reverse() işlevini kullanıyoruz. Bu işlev, görünüm işlevinde bir URL'nin sabit kodlanması gerekmeden yardımcı olur. Kontrolden geçirmek istediğimiz görünümü ve bu görünümü imleyen URL kalıbının değişken bölümü verilir. Bu durumda, [Öğretici 3]({{site.belgeler_ogretici3}})'de kurduğumuz URLconf kullanılarak reverse() çağrısı aşağıdaki gibi bir dize döndürür:
   <pre data-gnl="1 1p"><code class="language-python">
'/anketler/3/sonuclar/'
</code></pre>

   burada soru.id'nin değeri 3'tür. Bu yönlendirilen URL daha sonra, sonuç sayfasını görüntülemek için 'sonuclar' görünümünü çağırır.

[Öğretici 3]({{site.belgeler_ogretici3}})'de belirtildiği gibi, request bir HttpRequest nesnesidir. HttpRequest nesneleri hakkında daha fazla bilgi için <a href="#">istek ve yanıt (request ve response) belgeleri</a>ne bakın.

Birisi bir soruyu oylamaya başladıktan sonra, oylama() görünümü sorunun sonuçlar sayfasına yönlendirilir. Bu görünümü yazalım:

anketler/views.py
<pre data-gnl="1 1p"><code class="language-python">
  from django.shortcuts import get_object_or_404, render


  def sonuclar(request, question_id):
      soru = get_object_or_404(Soru, pk=question_id)
      return render(request, 'anketler/sonuclar.html', {'soru': soru})
</code></pre>

Bu, [Öğretici 3]({{site.belgeler_ogretici3}})'deki ayrinti() görünümüyle hemen hemen aynıdır. Tek fark şablon adıdır. Bu fazlalığı sonra çözeceğiz.

Şimdi bir anketler/sonuclar.html şablonu oluşturun:

anketler/templates/anketler/sonuclar.html
<pre data-gnl="1 1p"><code class="language-python">
  &lt;h1&gt;{&#123; soru.soru_metni &#125;}&lt;/h1&gt;

  &lt;ul&gt;
  {&#37; for secim in soru.secim_set.all &#37;}
      &lt;li&gt;{&#123; secim.secim_metni &#125;} -- {&#123; secim.oylar &#125;} oy{&#123; secim.oylar|pluralize &#125;}&lt;/li&gt;
  {&#37; endfor &#37;}
  &lt;/ul&gt;

  &lt;a href=&quot;{&#37; url 'oylar:ayrinti' soru.id &#37;}&quot;&gt;Tekrar oyla?&lt;/a&gt;
</code></pre>

Şimdi, tarayıcınızda "/anketler/1/" adresine gidin ve soruyu oylayın. Her oy verdiğinizde güncellenen bir sonuç sayfası görmelisiniz. Bir seçeneği seçmeden biçimi gönderirseniz, hata iletisini görmelisiniz.

<div data-bilget="genel" markdown="1">
### Not

oy() görüntüsü kodumuz küçük bir sorun yaşıyor. Önce secim_secildi nesnesini veritabanından alır. Sonra oyların yeni değerini hesaplar ve daha sonra veritabanına geri kaydeder. Ağ sitenizin iki kullanıcısı aynı anda oy kullanmaya kalkarsa, bu yanlış olabilir: Aynı değeri her iki oy içinde alır. Ardında her iki kullanıcı için 43'ün yeni değeri hesaplanır ve kaydedilir. Ancak beklenen değerin sonucu 44 olur.

Buna yarış durumu denir. eğer ilgileniyorsanız bu sorunu nasıl çözebileceğinizi öğrenmek için <a href="#">yarış şartlarından kaçma F()</a> konusunu okuyabilirsiniz.
</div>

<hr>

## Genel görünümler kullanın: Az kod daha iyidir

ayrinti() (<a href="{{site.belgeler_ogretici3}}">Öğretici 3</a>'den) ve sonuclar() görünümleri çok basittir. Yukarıda belirtildiği gibi gereğinden fazla... Anketlerin bir listesini görüntüleyen index() görünümüne benzer.

Bu görünümler, temel ağ geliştirmesinin ortak bir örneğini temsil eder: URL'den geçirilen bir değiştirgeye göre veritabanından veri alma, bir şablon yükleme ve işlenmiş şablonu iade etme. Bu çok yaygın olduğu için Django 'genel görünümler' örgüsü olarak adlandırılan bir kısayol sağlar.

Genel görünümler, yaygın kalıpları bir uygulama yazmak için Python kodunu yazmak zorunda kalmadığınız noktada soyutlar.

Anket uygulamamızı genel görünüm örgüsünü kullanacak şekilde dönüştürelim. Böylece kendi kodumuzdan bir demek silebiliriz. Dönüşümü yapmak için birkaç adım atmamız gerekecek. Yapacağımız şeyler:

- URLconf dönüştürme
- Gereksiz görünümlerin bir kısmını silme
- Django'nun genel görünümlerine dayalı yeni görünümler alma
{.liste}

Ayrıntılar için okumaya devam edin.

<div data-bilget="genel" markdown="1">
### Kod karışıklığı neden olur?

Genel olarak, bir Django uygulaması yazarken, jenerik görünümlerin sorununuza iyi uyum sağlayıp sağlamadığını değerlendireceksiniz ve kodunuzu ara ara yeniden biçimlendirmek yerine baştan kullanacaksınız. Ancak bu öğretici kısıtlı olarak, çekirdek kavramlara odaklanmak için şu ana kadar "uzun yoldan" görünümleri anlattı.

Bir hesap makinesini kullanmaya başlamadan önce temel matematiği öğrenmelisin meselesi...
</div>

## URLconf değiştirin

İlk olarak, anketler/urls.py URLconf'i açın ve şu şekilde değiştirin:

anketler/urls.py
<pre data-gnl="1 1p"><code class="language-python">
from django.urls import path

from . import views

app_name = 'anketler'
urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
    path('&lt;int:pk&gt;/', views.AyrintiView.as_view(), name='ayrinti'),
    path('&lt;int:pk&gt;/sonuclar/', views.SonuclarView.as_view(), name='sonuclar'),
    path('&lt;int:soru_id&gt;/oy/', views.oy, name='oy'),
]
</code></pre>

İkinci ve üçüncü desenlerin yol dizelerinde eşleşen desenin adı soru_id'den pk'ya geçtine dikkat edin.

<hr>

## Görünümleri değiştirin

Sırada... Eski dizin, ayrıntı ve sonuç görünümlerimizi kaldıracağız. Bunun yerine Django'nun genel görünümlerini kullanacağız. Bunu yapmak için anketler/views.py dosyasını açın ve şu şekilde değiştirin:

anketler/views.py
<pre data-gnl="1 1p"><code class="language-python">
  from django.shortcuts import get_object_or_404, render
  from django.http import HttpResponseRedirect
  from django.urls import reverse
  from django.views import generic

  from .models import Secim, Soru


  class IndexView(generic.ListView):
      tema_adi = 'anketler/index.html'
      baglam_nesnesinin_adi = 'son_sorular_listesi'

      def get_queryset(self):
          """En son yayınlanan 5 soruyu getir."""
          return Soru.objects.order_by('-yayim_tarihi')[:5]


  class AyrintiView(generic.DetailView):
      model = Soru
      tema_adi = 'anketler/ayrinti.html'


  class SonuclarView(generic.DetailView):
      model = Soru
      tema_adi = 'anketler/sonuclar.html'


  def oy(request, soru_id):
      ... # bundan sonra değişiklik gerekmiyor.
</code></pre>

Burada iki genel görünüm kullanıyoruz: ListeView ve AyrintiView. Her iki görünümde de "nesnelerin listesi gösteriliyor". Belirli bir nesne türü için ayrıntı sayfası görüntüleme kavramlarını özetlemek gerekir.


- Her jenerik görüşün hangi model üzerinde etkili olacağını bilmesi gerekir. Bu, model özniteliğini kullanarak sağlanmaktadır.
- AyrintiView genel görünümü, URL'den alınan birincil anahtar değerinin pk olarak çağrılmasını beklediğinden soru_id genel görünümler için pk olarak değiştirdik.
{.liste}

Varsayılan olarak, AyrintiView genel görünümü /<ugulama adı>/<model adı> _ayrinti.html adlı bir şablon kullanır. Bizim durumumuzda "anketler/soru_ayrinti.html" şablonu kullanacaktır. tema_adi özniteliği, doğal oalrak oluşturulmuş varsayılan şablon adı yerine belirli bir şablon adı kullanmasını Django'ya söylemek için kullanır. Ayrıca, sonuç listesi görünümü için tema_adi da belirtilir. Bu, sonuç görünümün ve ayrıntı görünümün arkasında bir AyrintiView olmasına rağmen işlendiğinde farklı bir görünüme sahip olmasını sağlar.

Benzer şekide, ListeView genel görünümü /<uygulama adı>/<model adı> _liste.html olarak adlandırılan varsayılan şablonu kullanır. ListeView'a mevcut "anketler/index.html" şablonumuzu kullanmasını söylemek için tema_adi kullanıyoruz.

Öğreticinin önceki bölümlerinde şablonlara soru ve son_sorular_listesi bağlam değişkenlerini içeren bir bağlam verilmiştir. AyrintiView için soru değişkeni doğal olarak sağlanır. Çünkü Django modeli (Soru) kullanıyoruz. Django bağlam değişkeni için uygun bir ad belirleyebiliyor. Bununla birlikte, ListeView iin doğal olarak oluşturulan bağlam değişkeni soru_listesi'dir. Bunu geçersiz kulmak için bunun yerine son_sorular_listesi'i kullanmak istediğimizi belirten baglam_nesnesinin_adi niteliğini sağlıyoruz. Seçeneksel yaklaşım olarak şablonlarınızı yeni varsayılan içerik değişkenleriyle eşleşecek şekilde değiştirebilirsiniz. Ancak Django'ya istediğiniz değişkeni kullanmasını söylemek çok daha kolaydır.

Sunucuyu çalıştırın ve genel görünümlere dayalı yeni anket uygulamanızı kullanın.

Genel görünümler hakkında ayrıntılı bilgi için [genel görünüm belgelerine](#) bakın.

Biçimlere ve genel görünümlere uygun olduğunda, anket uygulamalarımızı denemek hakkında bilgi edinebilirsiniz. Bunun için [Öğretici 5]({{site.belgeler_ogretici5}}) bölümünü okuyun.
