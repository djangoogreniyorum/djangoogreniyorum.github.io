---
layout: general
title: Öğretici 7 - Django Öğreniyorum
---
# İlk Django Uygulamanızı Yazma, Bölüm 7

Bu sayfa [Öğretici 6]({{site.belgeler_ogretici6}})'nın kaldığı yerden devam ediyor. Anket uygulamasına devam ediyoruz. Önce [Öğretici 2]({{site.belgeler_ogretici2}})'de keşfettiğimiz Django'nun doğal olarak oluşturulan yönetici sayfalarını özelleştirme üzerine odaklanacağız.

## Yönetci biçimini özelleştirin

Soru kalıplarını admin.site.register(Soru) ile kaydettirerek, Django varsayılan biçim gösterimi oluşturmayı başarır. Büyük olasılıkla, yönetici biçiminin nasıl göründüğünü ve nasıl çalıştığını özelleştirmek isteceksiniz. Bunu, nesneyi kaydettiğinizde istediğiniz seçenekleri Django'ya söyleyerek yapacaksınız.

Düzenleme biçimindeki alanları yeniden sıralayarak nasıl çalıştığını basitçe görelim. admin.site.register(Soru) satırını şu şekilde değiştirin:

anketler/admin.py
<pre data-gnl="1 1p"><code class="language-python">
  from django.contrib import admin

  from .models import Soru


  class SoruAdmin(admin.ModelAdmin):
      fields = ['yayim_tarihi', 'soru_metni']

  admin.site.register(Soru, SoruAdmin)
</code></pre>

Bu deseni takip edeceksiniz. Bir kalıp (model) yönetici sınıfı oluşturun, ardından bir kalıp için yönetici seçeneklerini değiştirmeniz gerektiğinde admin.site.register() için ikinci öğe olarak değiştirin.

Yukarıdaki bu özel değişiklik, "Yayımlama tarihi"nin "Soru" alanından önce gelmesini sağlar. Benzer bir görüntüyü alttaki resimde görebilirsiniz.

<img src="https://docs.djangoproject.com/en/2.0/_images/admin07.png" alt="">

Bu, yalnızca iki alanla etkileyici değildir, ancak düzinelerce alan içeren yönetici biçimleri için, sezgisel bir sipariş seçmek, önemli bir kullanılabilirlik ayrıntısıdır.

Ve düzinelerce alanla biçimlerden bahsediyorsak, biçimi alan kümelerine ayırmak isteyebilirsiniz:

anketler/admin.py
<pre data-gnl="1 1p"><code class="language-python">
  from django.contrib import admin

  from .models import Soru


  class SoruAdmin(admin.ModelAdmin):
      fieldsets = [
          (None,               {'fields': ['soru_metni']}),
          ('Date information', {'fields': ['yayim_tarihi']}),
      ]

  admin.site.register(Soru, SoruAdmin)
</code></pre>

Her taraftaki ilk öğeler, alan kümesinin başlığıdır. İşte biçimimizin şimdiki şekli bu resimdeki gibi olacaktır:
<img src="https://docs.djangoproject.com/en/2.0/_images/admin08t.png" alt="">

<hr>

## İlgili nesneler ekleme

Tamam, bizim Soru yönetici sayfamız var. Ancak bir sorunun birden fazla seçimi var ve yönetici sayfası seçimleri göstermiyor.

Oysa..

Bu sorunu çözmein iki yolu vardır. Birincisi, soru ile yaptığımız gibi yönetici ile seçim kaydetmektir. Bu kolay:

anketler/admin.py

<pre data-gnl="1 1p"><code class="language-python">
  from django.contrib import admin

  from .models import Secim, Soru
  # ...
  admin.site.register(Secim)
</code></pre>

Şimdi "Seçim"ler Django yönetimde kullanılabilir bir seçenektir. "Seçim ekle" biçimi şu resimdeki duruma benzer
<img src="https://docs.djangoproject.com/en/2.0/_images/admin09.png" alt="">

Bu biçimde, "Soru" alanı, veritabanındaki her soruyu içeren bir seçim kutusu. Django, ForeignKey'in yöneticide bir html select kutusu olarak temsil edilmesi gerektiğini bilir. Bizim durumumuzda, bu noktada sadece bir soru var.

Ayrıca, "Soru"nun yanında "Başka Ekle" bağlantısına dikkat edin. Bir ForeignKey ilişikisine sahip her nesene bunları bedelsiz alır. "Başka Ekle"yi tıkladığınızda, "Soru ekle" biçimi ile bir açılır pencere görürsünüz. Bu pencereye bir soru ekledikten sonra "Kaydet" düğmesine tıklarsınız. Django soruyu veritabanına kaydeder ve baktığınız "Seçimi ekle" biçimindeki seçili seçenek değişmeli olarak ekleyecektir.

Ancak, gerçekten bu örgüye Secim nesneleri eklemenin verimsiz bir yoludur. Soru nesnesini oluştururken bir grup seçimler ekleyebilriseniz daha iyi olur. Hadi bunu yapalım.

Seçim kalıbı için register() çağrısını kaldırın. Daha sonra okumak için Soru kaydetme kodunu düzenleyin.


anketler/admin.py
<pre data-gnl="1 1p"><code class="language-python">
  from django.contrib import admin

  from .models import Secim, Soru


  class SecimInline(admin.StackedInline):
      model = Secim
      extra = 3


  class SoruAdmin(admin.ModelAdmin):
      fieldsets = [
          (None,               {'fields': ['soru_metni']}),
          ('Date information', {'fields': ['yayim_tarihi'], 'classes': ['collapse']}),
      ]
      inlines = [SecimInline]

  admin.site.register(Soru, SoruAdmin)
</code></pre>

Bu, Django'ya şunu bildirir: "Seçim nesneler, Soru yönetim sayfasında düzenlenir. Varsayılan olarak, 3 seçen için yeterli alan sağlayın."

Bunun nasıl görüneceğini görmek için "Soru ekle" sayfasını yükleyin: Benzer görüntü resimdedir

<img src="https://docs.djangoproject.com/en/2.0/_images/admin10t.png" alt="">

Bu şu şekilde çalışır: İlgili seçenekler için üç ekleme yeri vardır. Fazladan belirtildiği gibi ve önceden oluşturulmuş bir nesenein "Değiştir" sayfasına geri döndüğünüzde başka bir üç ek alan daha kazanırsınız.

Üç geçerli yuvanın sonunda "Bir başka seçim ekle" bağlantısı bulacaksınız. Üzerine tıklarsanız, yeni bir alan eklenecektir. Ekelenen yuvarı kaldırmak isterseniz, eklenen yuvanın sağ üst köşesindeki X işaretini tıklayabilirsiniz. Asıl olan üç yuvayı kaldırabileceğinizi unutmayın. Aşağıdaki resim eklenen bir alanı gösteren benzer bir resimdir:

<img src="https://docs.djangoproject.com/en/2.0/_images/admin14t.png" alt="">

Küçük bir sorun olsa da... İlgili seçim nesnelerine girmek için tüm alanları görüntülemeye çok fazla ekran yeri gerekecektir. Bu nedenle, Django satıriçi ilgili nesneleri tablolaştırma yöntemi ile sunar; yalnızca okumak için SecimInline bildirimini değiştirmeniz gerekir:

anketler/admin.py
<pre data-gnl="1 1p"><code class="language-python">
  class SecimInline(admin.TabularInline):
    #...
</code></pre>

Bu TabularInline (StackedInline yerine) ile ilgili nesneler daha toplu, tablo tabanlı biçemde görüntülenir.

<img src="https://docs.djangoproject.com/en/2.0/_images/admin11t.png" alt="">

"Başka bir seçim ekle" düğmesini ve daha önce kaydedilmiş satırları kullanarak eklenen satırların kaldırılmasını sağlayan ek bir "Sil" sütunu olduğunu unutmayın.

<hr>

## Yönetici değiştirme listesini özelleştirin

Şimdi, Soru yönetici sayfasının iyi görünmesi üzerine, "liste değiştir" sayfasında biraz değişiklik yapalım. Örgüdeki tüm soruları görüntüleyen sayfadan bahsediyoruz.

İşte bu noktada şu şekildedir:

<img src="https://docs.djangoproject.com/en/2.0/_images/admin04t.png" alt="">

Varsayılan olarak, Django her nesnenin str()'sini görüntüler. Fakat bazen tek tek alanları görüntüleyerebilirsek daha yararlı olurdu. Bunu yapmak için, nesnenin değişim listesi sayfasında sütunlar olarak görüntülenen alan adlarının bir satırı olan list_display admin seçeneğini kullanın:

anketler/admin.py
<pre data-gnl="1 1p"><code class="language-python">
  class SoruAdmin(admin.ModelAdmin):
      # ...
      list_display = ('soru_metni', 'yayim_tarihi')
</code></pre>

İyi bir önlem almak için, <a href="{{site.belgeler_ogretici2}}">Öğretici 2</a>'deki was_published_recently() yöntemini de dahil edelim:

anketler/admin.py
<pre data-gnl="1 1p"><code class="language-python">
  class SoruAdmin(admin.ModelAdmin):
      # ...
      list_display = ('soru_metni', 'yayim_tarihi', 'was_published_recently')
</code></pre>

Şimdi soru değiştirme listesi sayfası şuna benzeyecek:

<img src="https://docs.djangoproject.com/en/2.0/_images/admin12t.png" alt="">

Bu değerlere göre sıralamak için sütun başlıklarına tıklayabilirsiniz. Bunun neseni was_published_recently başlığının haricinde, keyfi bir yöntemin çıktısıyla sıralama desteklenmediğidir. Ayrıca, was_published_recently için sütun başlığının varsayılan olarak yönetim adı (alt çizgiler, boşluklarla değiştirilir) olduğunu ve her satırın çıktının dize gösterimini içerdiğini unutmayın.

Bu yöntemi (anketler/models.py dosyasında) birkaç özellik vererek geliştirebilirsiniz:

anketler/models.py
<pre data-gnl="1 1p"><code class="language-python">
  class Soru(models.Model):
      # ...
      def was_published_recently(self):
          now = timezone.now()
          return now - datetime.timedelta(days=1) <= self.yayim_tarihi <= now
      was_published_recently.admin_order_field = 'yayim_tarihi'
      was_published_recently.boolean = True
      was_published_recently.short_description = 'Yakında yayınlanan?'
</code></pre>

Bu yöntem özellikleri hakkında daha fazla bildi için <a href="#">list_display</a>

anketler/admin.py dosyanızı tekrar düzenleyin ve Soru değiştirme listesi sayfasına bir gelişme ekleyin: list_filter'i kullanan filtereler. SoruAdmin'e aşağıdaki satırı ekleyin:

<pre data-gnl="1 1p"><code class="language-python">
list_filter = ['yayim_tarihi']
</code></pre>

Bu, kullanıcıların yayim_tarihi alanına göre değişiklik listesini filtrelemesine olanak tanıyan bir "filtre" kenar çubuğu ekler:

<img src="https://docs.djangoproject.com/en/2.0/_images/admin13t.png" alt="">

Görüntülenen filtre türü, filtrelemekte olduğunuz alan türüne bağlıdır. yayim_tarihi bir DateTimeField olduğu için Django, "Herhangi bir tarih", "Bugün", "Son 7 gün", "Bu ay", "Bu yıl" gibi uygun filtre seçeneklerini biliyor.

Bu iyi şekilleniyor. Biraz arama özelliği ekleyelim:

<pre data-gnl="1 1p"><code class="language-python">
search_fields = ['soru_metni']
</code></pre>

Bu, değişiklik listesinin en üstünde bir arama kutusu ekler. Birisi arama terimlerini girdiğinde, Django soru_metni alanını arayacaktır. Sahne arkasında LIKE sorgusu kullandığı halde istediğiniz sayıda alanı kullanabilirsiniz. Ancak arama lanlarının sayısını makul bir sayayıya getirmek, veritabanınızın arama yapmasını kolaylaştıracaktır.

Artık değişiklik listelerinin size bedelsiz sayfalandırma sağladığını not etmek için de iyi bir zaman. Varsayılan olarak sayfa başına 100 öğe görüntülenmektedir. Değiştirme listesi sayfalandırma, arama kutuları, süzgeçler, tarih hiyerarşileri ve sütun başlığı siparişleri birlikte düşünülmüş gibi birlikte çalışıyor.

<hr>

## Yönetici görünümünü özelleştirin

Açıkçası, her yönetici sayfasının üst kısmında "Django yönetimi" bulundurmak gülünçtür. Sadece yer tutan metin...

Buna rağmen Django'nun şablon örgüsünü kullanarak değiştirmek kolaydır. Django yönetici kendisi Django tarafından desteklenmektedir ve arayüzleri Django'nun kendi şablon örgüsünü kullanmaktadır.

### Proje şablonlarını özelleştirme

Proje dizininizde (manage.py içeren dizin) bir şablon dizini oluşturun. Şablonlar, Django'nun erişebildiği dosya örgünüzdeki herhangi bir yerde yaşayabilir. (Django, dunucunuzun çalıştığı kullanıcı olarak çalışır.) Bununla birlikte, şablonlarınızı proje içinde tutmak, takip etmek için iyi bir kuraldır.

Ayar dosyanızı açın (benimsite/settings.py hatırlayın) ve TEMPLATES ayarında DIRS seçeneği ekleyin:

benimsite/settings.py
<pre data-gnl="1 1p"><code class="language-python">
  TEMPLATES = [
      {
          'BACKEND': 'django.template.backends.django.DjangoTemplates',
          'DIRS': [os.path.join(BASE_DIR, 'templates')],
          'APP_DIRS': True,
          'OPTIONS': {
              'context_processors': [
                  'django.template.context_processors.debug',
                  'django.template.context_processors.request',
                  'django.contrib.auth.context_processors.auth',
                  'django.contrib.messages.context_processors.messages',
              ],
          },
      },
  ]
</code></pre>

DIRS, Django şablonlarını yüklerken denetlenecek dosya örgüsünü dizinlerinin bir listesidir; bu bir arama yoludur.

<div data-bilget="genel">

### Şablonları düzenleme

Durgun dosyalar gibi tüm şablonlarımızı bir büyük şablon dizininde bir araya getirebiliriz ve bu da mükemmel bir şekilde işe yarayabilir. Bununla birlikte, belirli bir uygulamaya ait şablonlar, projenin yerine bu uygulamanın şablon dizinine (örneğin anketler/templates) yerleştirilmelidir. Bunun neden tekrar kullanıldığını öğretici uygulamada daha ayrıntılı olarak tartışacağız.

</div>

Şimdi şablonların içinde admin adlı bir dizin oluşturun ve Django'nun kaynak kodundaki (django/contrib/admin/templates) varsayılan Django yönetici şablon dizininden bu dizine admin/base_site.html şablonunu kopyalayın.

<div data-bilget="genel">
### Django kaynak dosyaları nerdedir?

Django kaynak dosyalarının işletim örgünüzde nerede bulunduğu konusunda zorluk çekiyorsanız, aşağıdaki komutu çalıştırın:
  <pre data-gnl="1 1p"><code class="language-python">
  $ python -c "import django; print(django.__path__)"
  </code></pre>
</div>

Ardından, dosyayı düzenleyin ve uygun gördüğünüz gibi &#123;{site_header | default:_('Django yönetimi')}&#125; (kıvırcık parantezi dahil) kendi sitenizin adıyla değiştirin. Aşağıdakine benzer bir bölümle sonuçlanacak.

<pre data-gnl="1 1p"><code class="language-html">
  &#123;% block branding %&#125;
  &lt;h1 id="site-name"&gt;&lt;a href="&#123;% url 'admin:index' %&#125;"&gt;Anketler Yönetimi&lt;/a&gt;&lt;/h1&gt;
  &#123;% endblock %&#125;
</code></pre>

Bu yaklaşımı şablonları geçersiz kılmayı öğretmek için kullanırız. Gerçek bir projede muhtemelen bu özelleştirmeyi daha kolay yapmak için django.contrib.admin.AdminSite.site_header özniteliğini kullanırsınız.

Bu şablon dosyası &#123;% block branding %&#125; ve &#123;{ title }&#125; gibi birçok betin içeriyor. &#123;% ve &#123;{ etiketleri, Django'nun şablon dilinin bir parçasıdır. Django admin/base_site.html'i işlerken, bu şablon dili, [Öğretici 3]({{site.belgeler_ogretici3}})'te gördüğümüz gibi son HTML sayfasını üretmek üzere değerlendirilecektir.

Django'nun varsayılan yönetici şablonlarından herhangi birinin geçersiz kılınabileceğini unutmayın. Bir şablonu geçersiz kılmak için, yalnızca base_site.html ile yaptığınız şeyi yapın. Varsayılan dizinden kendi özel dizininize kopyalayın ve dğişiklik yapın.

<hr>

### Uygulamanızın şablonlarını özelleştirme

Zeki okuyucular soracaktır: Cevap APP_DIRS True olarak ayarlanmış olduğundan, Django otomatik yedek olarak kullanılmak üzere (yani django.contrib.admin bir uygulamadır unutmayın), her uygulama paketinin içinde bir şablon / alt dizin arar olmasıdır.

Anket uygulaması çok karmaşık değildir ve özel yönetici şablonlarına ihtiyaç duymaz. Ancak daha karmaşık hale gelmiş ve bazı işlevselliği için Django'nun standart yönetici şablonlarında değişiklik yapılması gerekiyorsa, projedeki şablonlardan ziyade uygulamanınşablonlarını değiştirmek mantıklı olacaktır. Böylece, anketler uygulamasını yeni bir projeye dahil edebilir ve ihtiyaç duyduğunuz özel şablonları bulacağından emin olabilirsiniz.

Django'nun şablonlarını bulma şekli hakkında daha fazla bilgi için <a href="#">şablon yükleme belgeleri</a>ne bakın.

<hr>

## Yönetici dizini sayfasını özelleştirin

Benzer bir notta, Django admin dizin sayfasının görünümünü ve şeklini özelleştirmek isteyebilirsiniz.

Varsayılan olarak, INSTALLED_APPS'teki yönetici uygulamasına kayıtlı tüm uygulamaları alfabeti olarak görüntüler. Düzende önemli değişiklikler yapmak isteyebilirsiniz. Sonuçta, dizin muhtemelen yöneticinin en önemli sayfası ve kullanımı kolay olmalıdır.

Özelleştirilecek şablon admin/index.html'dir. (önceki bölümdeki admin/base_site.html ile aynı işi yapın - varsayılan dizinden özel şablon dizininize kopyalayın). Dosyayı düzenleyin ve app_list adlı bir şablon değişkeni kullandığını görürsünüz. Bu değişken yüklü olan her Django uygulamasını içerir. Bunu kullanmak yerine, nesneye özel admin sayfalarına olan bağlantıları en iyi olduğunu düşündüğünüz şekilde kodlayabilirsiniz.

<hr>

## Sırada ne var?

Acemi öğreticisi burada biter. Bu arada, <a href="#">buradan nereye gideceğinizle ilgili</a> bazı işaretçileri kontrol etmek isteyebilirsiniz.

Python paketlemeyi biliyorsanız ve anketleri "yeniden kullanılabilir bir uygulamaya" dönüştürmeyi öğrenmek istiyorsanız gelişmiş öğretici: [Yeniden kullanılabilir uygulamalar nasıl yazılır?](#) bölümüne bakın.
