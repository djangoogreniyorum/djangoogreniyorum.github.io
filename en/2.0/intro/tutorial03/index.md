---
layout: general
title: Öğretici 3 - Django Öğreniyorum
---
# İlk Django Uygulamanızı Yazma, Bölüm 3

Bu sayfa [Öğretici 2](/en/20/intro/tutorial02/)'nin kaldığı yerden devam ediyor. Ağ siteleri uygulamasına devam ediyoruz. Ortak bir arayüz oluşturmaya odaklanacağız. Konumuz "görünümler" (views).

## Önbakış

Görünüm, Django uygulamanızda genellikle belirli bir işleve hizmet eden ve belirli bir şablona sahip olan ağ sayfası türüdür. Örneğin, bir blog uygulamasında aşağıdaki görüntülemelere sahip olabilirsiniz:

- Blog ana sayfası: en yeni içerikleri görüntüler
- İçerik "ayrıntı" sayfası: tek bir içeriğin sayfasını görüntüler
- Yıllık arşiv sayfası: bir yıl boyunca yayımlanmış içerikleri görüntüler
- Aylık arşiv sayfası: bir ay boyunca yayımlanmış içerikleri görüntüler
- Günlük arşiv sayfası: bir gün boyunca yayımlanmış içerikleri görüntüler
- Yorum akışı: bir içeriğe gönderilmiş yorumları görüntüler

Anket uygulamamızda aşağıdaki 4 görünüme sahip olacağız

- Soru "indeks" sayfası: en son soruları görüntüleyecek
- Soru "ayrıntı" sayfası: bir sorunun metnini oylar sonucu olmadan gösterecek fakat oy verme formu görünecek.
- Soru "oy sonuçları" sayfası: bir soruya verilen oy sonuçlarını gösterecek
- Oy akışı: belirli bir sorunun belirli bir seçimi için oy verme gösterilecek

Django'da ağ sayfaları ve diğer içerikler görüntüleme yöntemiyle sunulmuştur. Her görünüm basit bir Python işleviyle (veya sınıf tabanlı görünümler durumunda yöntemle) temsil edilir. Django istenen URL'yi inceleyerek (kesin olmak gerekirse URL'nin alan adının ardındaki kısmını) bir görünüm seçecektir.

Artık ağdaki zamanınızda 'ME2/Sites/dirmod.asp?sid=&amp;type=gen&amp;mod=Core+Pages&amp;gid=A6CD4967199A42D9B65B1B' gibi güzelliklere rastlamış olabilirsiniz. Django'nun bize göre çok daha zarif URL kalıpları sağladığını bilmekten mutluluk duyacaksınız.

Bir URL kalıbı, basitçe bir URL'nin genel biçimidir. Örneğin: /haberarsivi/&lt; yil &gt;/&lt; ay &gt;

Bir URL'den bir görünüm elde etmek için Django, 'URLconf' olarak bilinenleri kullanır. Bir URLconf, URL kalıplarını görünümlere eşler

Bu eğitimde URLconf'ların kullanımı konusunda temel yönergeler verilmektedir ve daha fazla bilgi için [URL yönlendiricisine](#) başvurabilirsiniz.

<hr>

## Görünümler Yazıyoruz
Şimdi anketler/views.py için birkaç görünüm daha ekleyelim. Bu görünümler biraz farklıdır çünkü bir öğe almaktadırlar:

anketler/views.py

  <pre data-gnl="1 1p"><code class="language-python">
    def ayrinti(request, soru_id):
        return HttpResponse("%s kimlikli soruya bakıyorsun." % soru_id)

    def sonuclar(request, soru_id):
        response = "%s kimlikli sorunun sonuçlarına bakıyorsun."
        return HttpResponse(response % soru_id)

    def oy(request, soru_id):
        return HttpResponse("%s kimlikli soruya oy veriyorsun." % soru_id)
  </code></pre>

Aşağıdaki yeni path() çağrılarını ekleyerek bu yeni görünümleri anketler.urls modülüne bağlayın:

anketler/urls.py

  <pre data-gnl="1 1p"><code class="language-python">
    from django.urls import path

    from . import views

    urlpatterns = [
        # örnek: /anketler/
        path('', views.index, name='index'),
        # örnek: /anketler/5/
        path('&lt;int:soru_id&gt;/', views.ayrinti, name='ayrinti'),
        # örnek: /anketler/5/sonuclar/
        path('&lt;int:soru_id&gt;/results/', views.sonuclar, name='sonuclar'),
        # örnek: /anketler/5/oy/
        path('&lt;int:soru_id&gt;/vote/', views.oy, name='oy'),
    ]
  </code></pre>

Tarayıcınızda "/anketler/34/" adresinden bir göz atın. ayrinti() yöntemini çalıştırır ve URL'de sağladığınız kimliği görüntüler. "/anketler/34/sonuclar/" ve "/anketler/34/oy/" adreslerini de deneyin. Bunlar yer tutucu sonuçlarını ve oylama sayfalarını görüntüler.

Birisi ağ sitenizden bir sayfa istediğinde "/anketler/34/" deneyin. Django benimsite.urls Python modülünü yükleyecektir. Çünkü ROOT_URLCONF ayarı öyle tanımlanmıştır. urlparrents adlı değişkeni bulur ve desenleri sırasıyla işleme geçirir. "anketler/" eşleşmesini bulduktan sonra bir sonraki "34/" değiştirgesini eşleştirir. Sonraki işleme için 'anketler.urls' URLconf'a gönderir. Orada '&lt;int: soru_id&gt;/' ile eşleşir ve ayrıntı() görünümüne şöyle bir çağrı gelir:

  <pre data-gnl="1 1p"><code class="language-python">
    ayrinti(request=&lt;HttpRequest object&gt;, soru_id=34)
  </code></pre>

soru_id = 34 bölümü &lt;int:soru_id&gt;'den gelir. Köşeli parantezler kullanılarak URL'nin bir kısmı "yakalanır" ve onu görünüm işlevine anahtar sözcük ögesi olarak gönderir. :soru_id&gt; dizesinin bir kısmı, eşleşen desen tanımlamak için kullanılacak adı ve &lt;int: bölümü, URL yolunun bu bölümüyle hangi kalıpların eşleşmesi gerektiğini belirleyen dönüştürücüdür

.html gibi URL cruft eklemenize gerek yoktur. İstemediğiniz durumda aşağıdakileri yapabilirsiniz.

  <pre data-gnl="1 1p"><code class="language-python">
    path('anketler/enyeniler.html', views.index),
  </code></pre>

fakat böyle bir şey yapmak gereksizdir.

<hr>

## Gerçekten bir şeyler yapan görünümler yazmak
Her görünüm iki şeyden birini yapmaktan sorumludur: istenen sayfanın içeriğini içeren bir HttpResponse nesenesinin döndürülmesi veya Http404 gibi bir istisna yükseltilmesi. Gerisi size kalmış.

Görünüm bir veritabanındaki kayıtları okuyabilir veya okyuamaz. Django'nun veya üçüncü parti bir Python şablon örgüsü gibi şablon örgüsü kullanabilir veya kullanamaz. İstediğiniz Python kütüphanelerini kullanarak, bir PDF dosyası, çıktı XML, anında bir ZIP dosyası oluşturma, istediğini herhangi bir şey oluşturabilir.

Django'nun istediği HttpResponse olmasıdır. Ya da bir istisna.

Çünkü kullanışlıdır, Django'nun [Öğretici 2](/en/20/intro/tutorial02/)'de ele aldığı kendi veritabanı API'sini kullanalım. İşte yeni bir index() görünümde, yayın tarihine göre, sistemdeki en son 5 anket sorusunu virgüllerle ayırarak gösteren bir tutsak var:

anketler/views.py

<pre data-gnl="1 1p"><code class="language-python">
from django.http import HttpResponse

from .models import Soru


def index(request):
    son_sorular_listesi = Soru.objects.order_by('-yayim_tarihi')[:5]
    cikti = ', '.join([q.soru_metni for q in son_sorular_listesi])
    return HttpResponse(cikti)

# Kalan görünüşleri (ayrinti, sonuclar, oy) olduğu gibi bırakın.
</code></pre>

Yine de burada bir sorun var: sayfanın tasarımı görünümde kodlanmış. Sayfanın görünümü değiştirmek isterseniz bu Python kodunu düzenlemeni gerekir. Django2nun şablon örgüsünü, görünüm kullanabileceği bir şablon oluşturarak Python'dan ayırmak için kullanalım.

İlk olarak, anketler dizininde "templates" adı verilen bir dizin oluşturun. Django şablonları arar.

Projenizin TEMPLATES ayarı, Django'nun şablonları nasıl yükleyeceğini ve işleyeceğini açıklıyor. Varsayılan ayarlar dosyası, APP_DIRS seçeneği True olarak ayarlanmış bir DjangoTemplates arka ucunu yapılandırır. Sözleşmeye göre, DjangoTemplates, INSTALLED_APPS'in her birinde bir "şablon" alt dizini arar.

Yeni olşturduğunuz şablon (templates) dizininde, anketler adlı başka bir dizin oluşturun ve bunun içinde index.html adlı bir dosya oluşturun. Başka bir değişle, şablonunuz anketler/templates/anketler/index.html olmalıdır. app_directoreis şablon yükleyicisinin yukarıda açıklanan şekilde çalışması nedeniyle Django içindeki bu şablona anketler/index.html gibi bakabilirsiniz.

<div data-bilget="genel" markdown="1">
### Şablon ad alanları

Şimdi şablonlarımızı doğrudan anketler/templates dizinine koyarak ilerleyebiliriz (Başka bir anket alt dizini oluşturmak yerine), ama aslına bakarsak kötü bir fikir olurdu. Django, bulduğu ilk şablonu seçecek ve adı aynı olan bir şablonun farklı bir uygulamada bulunması durumunda aralarında ayrım yapamayan Django olurdu. Django'yu doğru olana yönlendirebilmemiz lazım ve bunları sağlamak için en kolay yol onları isimlendirmektir. Yani, bu şablonları uygulamanın kendisi için adlandırılan başka bir dizine yerleştirerek...

</div>

Şablona aşağıdaki kodu koyun:

anketler/templates/anketler/index.html

  <pre data-gnl="1 1p"><code class="language-python">
    {&#37; if son_sorular_listesi &#37;}
        &lt;ul&gt;
        {&#37; for soru in son_sorular_listesi &#37;}
            &lt;li&gt;&lt;a href=&quot;/anketler/{&#123; soru.id &#125;}/&quot;&gt;{&#123; soru.soru_metni &#125;}&lt;/a&gt;&lt;/li&gt;
        {&#37; endfor &#37;}
        &lt;/ul&gt;
    {&#37; else &#37;}
        &lt;p&gt;Anket bulunamadı.&lt;/p&gt;
    {&#37; endif &#37;}
  </code></pre>

Şablonu kullanmak için dizin görünümünü anketler/views.py dosyasında güncelleyelim.

anketler/views.py

<pre data-gnl="1 1p"><code class="language-python">
  from django.http import HttpResponse
  from django.template import loader

  from .models import Soru


  def index(request):
      son_sorular_listesi = Soru.objects.order_by('-yayim_tarihi')[:5]
      template = loader.get_template('anketler/index.html')
      context = {
          'son_sorular_listesi': son_sorular_listesi,
      }
      return HttpResponse(template.render(context, request))
</code></pre>

Bu kod, anketler/index.html adlı şablonu yükler ve bir bağlamı iletir. Bağlam, Python nesnelerine bir sözlük eşleme şablon adlarıdır.

Tarayıcınısı "/anketler/" adresine getirerek sayfayı yükleyin ve [Öğretici 2](/en/20/intro/tutorial02/)'nin "Yenilikler ne?" sorusunu içeren madde işaretli bir liste görmelisiniz. Bağlantı, sorununun ayrıntı sayfasına işaret eder.

### Kısayol: render()

Bir şablon yüklemek, bir bağlamı doldurmak ve işlenmiş şablonun sonucuyla bir HttpResponse nesnesi döndürmek çok yaygın bir deyimdir. Django bir kısayol sağlar. İşte tam index() görünümü, yeniden yazıldı:

anketler/views.py

<pre data-gnl="1 1p"><code class="language-python">
  from django.shortcuts import render

  from .models import Soru


  def index(request):
      son_sorular_listesi = Soru.objects.order_by('-yayim_tarihi')[:5]
      context = {'son_sorular_listesi': son_sorular_listesi}
      return render(request, 'anketler/index.html', context)
</code></pre>

Bunu tüm bu görünümlerde yaptıktan sonra yükleyici ve HttpRepsonse'u artık içe aktarmamız gerekmediğini unutmayın (ayrıntılar, sonuçlar ve oy için saplama yöntemleri hala varsa HttpRepsonse'u saklamak istersiniz).

render() işlevi, istek nesenesini ilk bağımsız değişkeni, bir şablon adını ikinci bağımsız değişkeni ve bir sözlüğü isteğe bağlı üçüncü bağımsız değişkeni alır. Verilen bağlamla oluşturulmuş verilen şablonun bir HttpResponse nesnesi döndürür.

<hr>

## 404 hatası yükseltme

Şimdi, soru ayrıntısı görünümüyle uğraşalım. Belirli bir anket için soru metnini görüntüleyen sayfa. İşte görünüm:

anketler/views.py

<pre data-gnl="1 1p"><code class="language-python">
  from django.http import Http404
  from django.shortcuts import render

  from .models import Soru
  # ...
  def ayrinti(request, soru_id):
      try:
          soru = Soru.objects.get(pk=soru_id)
      except Soru.DoesNotExist:
          raise Http404("Soru bulunamadı")
      return render(request, 'anketler/ayrinti.html', {'soru': soru})
</code></pre>

Buradaki yeni kavram: İstek, istenen kimliği olan bir soru bulunmaması halinde Http404 istisnasını yükseltir.

anketler/ayrinti.html şablonuna neyin koyabileceğini tartışacağız, ancak yukarıdaki örneği çabucak kullanmak istiyorsanız, sadece aşağırdakileri içeren bir dosya oluşturun:

anketler/templates/anketler/ayrinti.html

<pre data-gnl="1 1p"><code class="language-python">
&#123;{ question }&#125;
</code></pre>

Şimdi başlayacaksın

## Kısayol: get_object_or_404()
get() işlevini kullanmak ve nesne yoksa Http404'ü yükseltmek çok yaygın bir deyimdir. Djano buna bir kısayol sağlar. ayrinti() görünümü aşağıda yeniden yazılmıştır:

anketler/views.py

<pre data-gnl="1 1p"><code class="language-python">
from django.shortcuts import get_object_or_404, render

from .models import Soru
# ...
def ayrinti(request, soru_id):
    soru = get_object_or_404(Soru, pk=soru_id)
    return render(request, 'anketler/ayrinti.html', {'soru': soru})
</code></pre>

get_object_or_404() işlevi, ilk öğe olarak bir Django modeli ve model yöneticisinin get() işlevine ilettiği rasgele sayıda anahtar sözcük bağımsız değişkeni alır. Nesne yoksa Http404'ü yükseltir.

<div data-bilget="genel" markdown="1">
### Felsefesi

Neden daha yüksek bir seviyede ObjectDoesNotExist istisnalarını doğal olarak yakalamak yerine bir yardımcı işlev get_object_or_404() kullanılıyor veya model API'si ObjectDoesNotExist yerine Http404 yükseltiliyor?

Çünkü model katmanı görüntüleme katıyla eşleştirir. Django'nun en önemli tasarım amaçlarından biri, gevşek bağlamayı korumaktır. Bazı kontrollü bağlantılar django.shortcuts modülünde tanıtıldı.

</div>

get_object_or_404() gibi çalışan, ancak get() yerine filter() işlevini kullanan bir get_list_or_404() işlevi de vardır. Liste boşsa, Http404'ü yükseltir.

<hr>

## Şablon sistemini kullan

Anket uygulaması için ayrinti() görünümüne geri dönün. Bağlam değişkeni sorusu göz önüne alındığında, anketler/ayrinti.html şablonunun nasıl görünebileceğini burada görebilirsiniz:

anketler/templates/anketler/ayrinti.html

<pre data-gnl="1 1p"><code class="language-python">
  &lt;h1&gt;{&#123; question.question_text &#125;}&lt;/h1&gt;
  &lt;ul&gt;
  {&#37; for choice in question.choice_set.all &#37;}
      &lt;li&gt;{&#123; choice.choice_text &#125;}&lt;/li&gt;
  {&#37; endfor &#37;}
  &lt;/ul&gt;
</code></pre>

Şablon örgüsü, değişken niteliklere erişmek için nokta-gözatma sözdizimini kullanır. &#123;{soru.soru_metni}&#125; örneğinde, ilk Django nesne sorusunda sözlükte arama yapar. Başarısız olursa, bir öznitelik araştırması dener ki bu da işe yarar. Öznitelik araması başarısız olursa, bir liste dizini araştırması denemiş olurdu.

Yöntem çağrısı, {&#37; for &#37;} döngüsünde olur: soru.secim_set.all, Secim nesnelerinin yinelenebilirliğini döndüren ve {&#37; for &#37;} etiketinde kullanılacak kodu soru.secim_set.all() olarak yorumlanır.

Şablonlar hakkında daha fazla bilgi için [şablon rehberi](#)ne bakın.

<hr>

## Şablonlarda kodlanmış URL'leri kaldırma

anketler/index.html şablonundaki bir sorunun bağlantısını yazdığımızda hattın kusmen sabit kod olduğunu untumayın:

<pre data-gnl="1 1p"><code class="language-python">
&lt;li&gt;&lt;a href=&quot;/anketler/{&#123; soru.id &#125;}/&quot;&gt;{&#123; soru.soru_metni &#125;}&lt;/a&gt;&lt;/li&gt;
</code></pre>

Bu kodlanmış, sıkı bağlanmış yaklaşımla ilgili sorun, birçok şablonla projelerde URL'leri değiştirmek zor oluyor. Bununla birlikte, anketler.urls modülünde path() işlevinde name öğesi tanımladığınızdan, {&#37; for &#37;} şablon etiketini kullanarak yapılandırmalarınızda tanımlanan belirli URL yollarına güvenebilirsiniz:

<pre data-gnl="1 1p"><code class="language-python">
&lt;li&gt;&lt;a href=&quot;{&#37; url 'ayrinti' soru.id &#37;}&quot;&gt;{&#123; soru.soru_metni &#125;}&lt;/a&gt;&lt;/li&gt;
</code></pre>

Bunun çalışması, anketler.urls modülünde belirtilen URL tanımına bakmaktır. 'ayrinti' için URL adının tam olarak nerede tanımlandığını görebilirsiniz.

<pre data-gnl="1 1p"><code class="language-python">
  ...
  # 'name' değeri {&#37; url &#37;} şablon etiketi tarafından çağrıldığı gibi
  path('&lt;int:soru_id&gt;/', views.ayrinti, name='ayrinti'),
  ...
</code></pre>

Anketlerin ayrıntı görünümünün URL'sini şablonda (veya şablonlarda) yapmak yerine, anketler/ozellikler/12/ gibi bir şeye değiştirmek isterseniz, bunu anketler/urls.py dosyasında değiştireceksiniz:

<pre data-gnl="1 1p"><code class="language-python">
  ...
  # 'name' değeri {&#37; url &#37;} şablon etiketi tarafından çağrıldığı gibi
  path('ozellikler/&lt;int:soru_id&gt;/', views.ayrinti, name='ayrinti'),
  ...
</code></pre>

<hr>

## URL isimleri isimlendirme

Öğretici projede sadece bir uygulama var, anketler. Gerçek Django projelerinde beş, on, yirmi veya daha fazla uygulama olabilir. Django, aralarında bulunan URL adlarını nasıl ayırt eder? Örneğin, anketler uygulaması bir "ayrinti" görünümüne sahiptir ve bu nedenle bir blog için olan aynı projedeki bir uygulama olabilir. Django {&#37; url &#37;} şablon etkitekini kullanırken bir url için hangi uygulama görüntülemesinin oluşturulacağını bilmesi için bunu nasıl yapar?

Cevap. URLconf'unuzda ad alanları eklemektir. anketler/urls.py dosyasında, devam edin ve uygulama ad alanını ayarlamak için bir app_name ekleyin:

anketler/urls.py

<pre data-gnl="1 1p"><code class="language-python">
  from django.urls import path

  from . import views

  app_name = 'anketler'
  urlpatterns = [
      path('', views.index, name='index'),
      path('&lt;int:soru_id&gt;/', views.ayrinti, name='ayrinti'),
      path('&lt;int:soru_id&gt;/sonuclar/', views.sonuclar, name='sonuclar'),
      path('&lt;int:soru_id&gt;/oy/', views.oy, name='oy'),
  ]
</code></pre>

anketler/index.html şablonunuzu şu adresten değiştirin:

anketler/templates/anketler/index.html

<pre data-gnl="1 1p"><code class="language-python">
&lt;li&gt;&lt;a href=&quot;{&#37; url 'ayrinti' soru.id &#37;}&quot;&gt;{&#123; soru.soru_metni &#125;}&lt;/a&gt;&lt;/li&gt;
</code></pre>

ad alanı 'ayrinti' görünümünü işaret etmek için:

<pre data-gnl="1 1p"><code class="language-python">
&lt;li&gt;&lt;a href=&quot;{&#37; url 'anketler:ayrinti' soru.id &#37;}&quot;&gt;{&#123; soru.soru_metni &#125;}&lt;/a&gt;&lt;/li&gt;
</code></pre>

Görünümler yazarken rahat edersiniz, basit biçim işleme ve genel görünümler hakkında bilgi edinmek için [öğretici 4](/en/20/intro/tutorial04/) gölümüne geçin.
