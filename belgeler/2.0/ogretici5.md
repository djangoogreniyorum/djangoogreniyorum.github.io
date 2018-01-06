---
layout: general
title: Öğretici 5 - Django Öğreniyorum
---
# İlk Django Uygulamanızı Yazma, Bölüm 5

Bu eğitim, [Öğretici 4]({{site.belgeler_ogretici4}})'ün kaldığı yerden devam ediyor. Anket uygulaması yaptık ve bunun için bazı doğal sınamalar oluşturacağız.

## Doğal sınama tanıtma

### Doğal sınamalar nedir?

Deneyler, kodunuzun çalışmasını sınayan basit işlemlerdir.

Sınamalar farklı seviyelerde çalışır. Bazı sınamalar küçücük bir ayrıntı için geçerli olabilir (belirli bir kalıp yöntemi, değerleri beklendiği gibi döndürür mü?). Diğerleri yazılımın genel işlemini incelemektedir (sitedeki bir dizi kullanıcı girişi istenilen sonucu verir mi?). [Öğretici 2]({{site.belgeler_ogretici2}})'de daha önce yapılan sınamalardan farklı değil, bir yöntemin davranışını incelemek için kabuğu kullanarak veya uygulamayı çalıştırarak veri girme işlemi yapılır.

Doğal sınamalarda farklı olan şey, sınama çalışmalarının sizin tarafınızdan yapılmasıdır. Bir kez bir dizi sınama oluşturursunuz ve daha sonra uygulamanızda değişiklik yaparken, kodunuzun başlangıçta tasarlandığı gibi çalışıp çalışmadığını zaman alıcı el sınamaları yapmak zorunda kalamadan halledebilirsiniz.

<hr>

## Neden sınamalar oluşturmamız gerekiyor?

Durduk yere neden sınamalar oluşturuyoruz acaba?

Python Django'yu öğrenmek için yeterli seviyeye eriştiğinizi düşünebilirsiniz. Öğrenmek ve yapmak için başka bir şeyler daha olması can sıkıcı belki de gereksiz görünebilir. Ne de olsa anketlerimiz şimdi mutlu edici şekilde çalışıyor. Doğal testler oluşturma sorununu geçmesi daha iyi çalışmasını sağlamayacak. Anketler uygulamasını oluşturmak, Django programlamanın en son aşamasını oluşturuyor olsaydı bunlar doğru kabul edilirdi. Doğal sınamaları nasıl oluşturulacağını bilmenize gerek olmazdı. Ancak durum böyle değil. Şimdi öğrenmek için en uygun zamandasın.

### Sınamalar zaman kazandıracaktır

Belirli bir noktaya kadar, 'işe yarayıp yaramayacağının teyid edilmesi', tatmin edici bir sınama olacaktır. Daha katmanlı bir uygulamada, bileşenler arasında düzinelerce karmaşık etkileşime sahip olabilirsiniz.

Bu bileşenlerden herhangi birinde bir değişiklik, uygulamanın davranışı üzerinde beklenmedik sonuçlara neden olabilir. Yine de 'işe yarıyor' gibi görünüp görünmediğini kontrol etmek, kodunuzun işlevselliğiyle, sınama verilerinizin yirmi farklı yüzüyle kullanarak, yalnızca zamanınızı iyi bir şekilde kullanmadığınız, bir şeyi kırmadığınızdan emin olmak anlamına gelebilir.

Doğal sınamalar özellikle sizin için saniyeler içinde bunu yaparken geçerlidir. Bir şeyler yanlış giderse, sınamalar beklenmedik davranışa neden olan kodun tanımlanmasında da yardımcı olur.

Bazen, özellikle kodunuzun düzgün çalışıp çalışmadığını bildiğiniz durumlarda, sınama yazma işiyle yüz yüze gelme durumları olabilir.

Bununla birlikte sınama yazma görevi uygulamanızı elle denemekten veya yeni girilen bir sorunun nedeni belirlemeye çalışmaktan çok daha uygundur.

<hr>

### Sınırlamalar yalnızca sorunları tanımlamaz, onları engeller

Deneyleri yalnızca gelişimin olumsuz bir yönüne bakmak olarak düşünmek yanlış olur.

Sınamalar yapılmadığında, bir uygulamanın amacı oldukça sığ olabilir. Kendi kodunuz olsa bile, bazen kendisinin tam olarak ne yaptığını aramakla meşgul olurken kendizini bulabilirsiniz.

Sınamalar bu durumu değiştirir; kodunuzu içeriden aydınlatırlar. Bir şeyler ters gittiğinde, yalnız gittiğini fark etmemiş olsanız bile yanlış giden parçaya ışık tutarlar.

## Sınamalar kodunuzu daha çekici hale getirir

Parlak bir yazılım parçası oluşturmuş olabilirsiniz. Ancak birçok geliştiricinin sınamalara ihtiyacı olmadığından ona bakmayı reddedeceğini göreceksiniz. Sınamalar olmadan güvenemezler ama... Django'nun asıl geliştiricilerinden Jacob Kaplan-Moss, 'Deneylenmeyen kodlar tasarım gereği bozuldu' diyor.

Diğer geliştiriciler, sınamaları ciddiye almadan önce yazılımınızda görmek istedikleri için sınama yazmaya başlamanızın bir başka nedeni var.

<hr>

### Sınamalar takımların birlikte çalışmasına yardımcıdır

Önceki noktalar, bir uygulamayu sürdüren tek bir geliştiricinin bakış açısından yazılmıştır. Karmaşık yugulamalar takımlar tarafından yönetilecektir. Deneyler, meslektaşlarınızın yanlışlıkla kodunuzu kırmamalarını garanti eder (ve bilmeden onlarınkini kırmazsınız). Bir django programcısı gibi yaşamak istiyorsanız, sınama yazarken iyi olmalısınız!

<hr>

## Temel sınama kurmayları

Sınama yazmanın birçok yolu vardır.

Bazı programcılar "[sınama odaklı geliştirme](https://en.wikipedia.org/wiki/Test-driven_development)" adlı bir kararlılığı izlemektedir. Aslında kodlarını yazmadan önce sınamalarını yazıyorlar. Bu, zıt-sezgisel görünebilir, ancak aslında çoğunluklar insanların yapacakları şeyle benzerdir. Bir sorunu açıklarlar ve çözmek için bazı kodlar oluştururlar. Sınama odaklı geliştirme sorunu basitçe bir Python sınama durumunda biçimlendirilir.

Daha sıklıkla, sınamaya yeni başlayanlar bazı kodlar oluşturur ve daha sonra bazı sınamalara sahip olmalarına karar verirler. Belki daha önce bazı sınamalar yazmak daha iyi olurdu, ancak başlamak için hiçbir zaman çok geç değildir.

Bazen sınama yazmaya nederen başlayacağınızı bulmak zordur. Binlerce Python satırı yazdıysanız, sınayabilecek bir şey seçmek kolay olmayabilir. Böyle bir durumda, yeni bir özellik eklediğinizde veya bir hatayı düzelttiğinizde, bir sonraki değişiklik yapışınız ilk sınamanızı yazmak verimli olur.

O halde hemen yapmaya başlayalım.

<hr>

## İlk sınamamızı yazıyoruz

Önce bir hata tespit ediyoruz.

Neyse ki, anket uygulamamızda derhal düzeltilecek küçük bir hata var. Soru son gün içinde yayınlanırsa (doğru) Soru.was_published_recently() yöntemi true değerini döndürür ancak aynı zamanda eğer sorunun yayim_tarihi alanı da gelecektir (kesin değil).

Hatanın gerçekten olup olmadığını sınamak için yöneticiyi kullanarak tarihini gelecekte olan bir soruya dönüştürün ve kamuğu kullanarak yöntemi kontrol edin:

<pre data-gnl="1 1p"><code class="language-python">
  >>> import datetime
  >>> from django.utils import timezone
  >>> from polls.models import Soru
  >>> # 30 gün sonra yayım tarihli bir Soru örneği oluşturma
  >>> gelecegin_sorusu = Soru(yayim_tarihi=timezone.now() + datetime.timedelta(days=30))
  >>> # Son zamanlarda basıldı mı?
  >>> gelecegin_sorusu.was_published_recently()
  True
</code></pre>

Gelecekteki şeyler 'son' içine girmemesi gerektiği için bu yanlış (false) olarak dönmeliydi. True verdiği için açıkça yanlıştır.

<hr>

### Hatayı açığa çıkarmak için bir sınama oluşturun

Kabukta sorunun sınanması için yaptığımız şey, doğal bir sınamada tam olarak neler yapabileceğimiz. Bu yüzden bunu doğal sınama haline getirelim.

Bir uygulamanın sınamaları için geleneksek bir yer bulunur, uygulamanın tests.py dosyası. Sınama örgüsü adı test ile başlayan herhangi bir dosyada sınamaları doğal olarak bulacaktır.

Anket uygulamasında tests.py dosyasına şunu koyun:

anketler/tests.py

<pre data-gnl="1 1p"><code class="language-python">
  import datetime

  from django.utils import timezone
  from django.test import TestCase

  from .models import Soru


  class SoruModelTests(TestCase):

      def sinama_gelecegin_sorusu_son_zamanlarda_gelme(self):
          """
          was_published_recently() yayim_tarihi gelecekte olan sorular için False döndürür.
          """
          zaman = timezone.now() + datetime.timedelta(days=30)
          gelecegin_sorusu = Question(yayim_tarihi=zaman)
          self.assertIs(gelecegin_sorusu.was_published_recently(), False)
</code></pre>

Burada yaptığımız şey gelecekte olan bir yayim_tarihi ile bir Soru örneği oluşturan bir yöntem kullanılarak django.test.TestCase alt sınıfı oluşturuldu. Daha sonra, yanlış olması gereken was_published_recently() çıktısını kontrol ederiz.

<hr>

### Sınamayı yürütmek

Kabukta sınamamızı yapabiliriz:

<pre data-gnl="1 1p"><code class="language-python">
$ python manage.py test anketler
</code></pre>

ve şöyle bir şey göreceksiniz:

<pre data-gnl="1 1p"><code class="language-python">
  Creating test database for alias 'default'...
  System check identified no issues (0 silenced).
  F
  ======================================================================
  FAIL: sinama_gelecegin_sorusu_son_zamanlarda_gelme (anketler.tests.SoruModelTests)
  ----------------------------------------------------------------------
  Traceback (most recent call last):
    File "/path/to/benimsite/anketler/tests.py", line 16, in sinama_gelecegin_sorusu_son_zamanlarda_gelme
      self.assertIs(gelecegin_sorusu.was_published_recently(), False)
  AssertionError: True is not False

  ----------------------------------------------------------------------
  Ran 1 test in 0.001s

  FAILED (failures=1)
  Destroying test database for alias 'default'...
</code></pre>

Burada ne oldu:

{.liste}
* python manage.py test anketler sınama için anketler uygulamasına baktı
* django.test.TestCase sınıfının bir alt sınıfı bulundu.
* Sınamanın amacı için özel bir veritabanı oluşturuldu.
* Sınama yöntemleri için neyin adı 'test' ile başlıyor diye bakıldı.
* sinama_gelecegin_sorusu_son_zamanlarda_gelme içinde yayim_tarihi 30 gün ertelenmiş olarak soru oluşturuldu.
* ve assertls() yöntemi kullanılarak, was_published_recently() öğesinin True döndürdüğünü keşfettik, ancak False değeri bekliyorduk.

Sınama, hangi sınamanın başarısız olduğunu ve hatanın hangi satırda meydana geldiğini bildirir.

<hr>

### Hatayı giderme

Biz sorunun ne olduğunu zaten biliyoruz: Soru.was_published_recently() eğer yayim_tarihi ileri bir tarihse False döndürmeli. models.py içindeki yöntem düzeltilmeli. Böylece tarih sadece geçmişte olursa True döndürecek:

anketler/models.py
<pre data-gnl="1 1p"><code class="language-python">
  def was_published_recently(self):
      now = timezone.now()
      return now - datetime.timedelta(days=1) <= self.pub_date <= now
</code></pre>

ve tekrar sınayın:

<pre data-gnl="1 1p"><code class="language-python">
  Creating test database for alias 'default'...
  System check identified no issues (0 silenced).
  .
  ----------------------------------------------------------------------
  Ran 1 test in 0.001s

  OK
  Destroying test database for alias 'default'...
</code></pre>

Bir hatayı tespit ettikten sonra, onu açığa çıkaran bir sınama yazdık ve sınamadaki hatalar giderildi. Böylece hatadaki hatayı düzelttik.

Gelecekte uygulamanızla birlikte başka birçok şey yanlış olabilir, ancak yanlışlıkla bu hatayı tekrar başlatmayacağımızdan emin olabiliriz. Çünkü sınamayı çalıştırmak derhal bizi uyaracaktır. Uygulamanın bu küçük kısmını güvenli bir şekilde sonsuza dek sabitlenmiş düşünebiliriz.

<hr>

### Daha kapsamlı sınamalar

Buradayken, was_published_recently() yöntemini daha da kısaltabiliriz. Aslında, bir hatayı başka bir hatayla düzeltmek durumunda olsaydık utanç verici olurdu.

Yöntemin davranışını daha kapsamlı bir şekilde sınamak için, aynı sınıfa iki tane daha sınama yöntemi ekleyin:

anketler/tests.py
<pre data-gnl="1 1p"><code class="language-python">
  def sinama_gecmisin_sorusu_son_zamanlarda_gelme(self):
      """
      was_published_recently() yayim_tarihi 1 günden daha eski olan sorular için False döndürecek.
      """
      time = timezone.now() - datetime.timedelta(days=1, seconds=1)
      gecmisin_sorusu = Soru(yayim_tarihi=time)
      self.assertIs(gecmisin_sorusu.was_published_recently(), False)

  def sinama_bugunun_sorusu_son_zamanlarda_gelme(self):
      """
      was_published_recently()yayim_tarihi son 24 saat olan sorular için True döndürecek.
      """
      time = timezone.now() - datetime.timedelta(hours=23, minutes=59, seconds=59)
      bugunun_sorusu = Soru(yayim_tarihi=time)
      self.assertIs(bugunun_sorusu.was_published_recently(), True)
</code></pre>

Ve şimdi, Soru.was_published_recently() geçmiş, bugün ve gelecek tarihine sahip sorular için değerler döndürerek sınayan üç sınayıcımız var.

Yine anketler basit bir uygulamadır. Ancak gelecekte ve etkileşimde bulunan kod ne olursan olsun karmaşık olmasına karşın, artık sınamalar için yazdığımız yöntemin beklenen yollarla davranacağına dair bazı güvencelerimiz vardır.

<hr>

## Bir görünümü sınayalım

Anket uygulamaları oldukça belirsizdir: yayim_tarihi alanı gelecekte bulunanlar da dahil olmak üzere herhangi bir soruyu yayınlar. Bunu iyileştirmeliyiz. Gelecek tarihli bir yayim_tarihi ayarlamak, o anın o zamana kadar yayınlandığını, ancak o zaman kadar görüntülenmediği anlamına gelir.

### Bir görünüm için bir sınama

Yukarıdaki hatayı düzelttiğimizde, önce sınamayı yazdık sonra düzeltmek için kodu yazdık. Aslında bu sınama odaklı geliştirme için basit bir örnekti. Ancak işi hangi sırayla gerçekleştirdiğimiz önemli değil.

İlk sınamamızda, kodun iç davranışına yakından odaklandık. Bu sınamada, davranışını bir internet tarayıcısı üzerinden bir kullanıcı tarafından yaşanacağını kontrol etmek istiyoruz.

Bir şeyi düzeltmeye çalışmadan önce elimizdeki araçlara göz atalım.

<hr>

## Django sınama istemcisi

Django, görünüm düzeyinde kodla etkileşime giren bir kullanıcıyı taklit etmek için bir sınama istemcisi sunmaktadır. tests.py'de veya kabukta bile kullanabilirsiniz.

tests.py dosyasında gerekli olmayacak birkaç şey yapmamız gerektiğinde kabukla tekrar başlayacağız. Birincisi, sınama ortamını kabukta kullanmaktır:

<pre data-gnl="1 1p"><code class="language-python">
  >>> from django.test.utils import setup_test_environment
  >>> setup_test_environment()
</code></pre>

setup_test_environment(), aksi takdirde kullanılamayacak olan response.context gibi yanıtlarda bazı ek nitelikleri incelememizi sağlayacak bir şablon oluşturucuyu yükler. Bu yöntemin bir sınama veritabanını kurmadığına dikkat edin. Çünkü aşağıdakiler varolan veritabanına karşı çalıştırılacak ve çıktı zaten oluşturduğunuz soruları temel alarak farklılık gösterebilir. settings.py dosyasındaki TIME_ZONE değeri doğru değilse beklenmedik sonuçlara neden olabilirsiniz. Ayarlama yaptığınızı hatırlamıyorsanız devam etmeden önce sağlamasını yapın.

Sonra sınama istemci sınıfını içeri aktarmanız gerekir. (daha sonra tests.py dosyasında kendi istemcisi ile birlikte gelen django.test.TestCase sınıfını kullanacağız, dolayısıyla bu gerekli olmayacaktır):

<pre data-gnl="1 1p"><code class="language-python">
  >>> from django.test import Client
  >>> # kullanım için istemcinin bir örneğini oluştur.
  >>> client = Client()
</code></pre>

Hazır olduğunda, isemciden bizim için biraz çalışma yapmasını isteyebiliriz:

<pre data-gnl="1 1p"><code class="language-html">
  >>> # '/'dan cevap alın
  >>> response = client.get('/')
  Not Found: /
  >>> # O adresden 404 beklemeliyiz; bunun yerine "geçersiz HTTP_HOST üstbilgisi" hatası ve bir 400 yanıtı görürseniz, daha önce açıklanan setup_test_environment() çağrısını olasılıkla ihmal ettiniz.
  >>> response.status_code
  404
  >>> # Öte yandan '/anketler/' de bir şey bulmamız gerektiğini düşünüyoruz. Bunun yerine sabit kodlu URL yerine 'reverse()' kullanacağız.
  >>> from django.urls import reverse
  >>> response = client.get(reverse('anketler:index'))
  >>> response.status_code
  200
  >>> response.content
  b'\n    <ul>\n    \n    * <a href="/anketler/1/">Yenilikler ne?</a>\n    \n  \n\n'
  >>> response.context['son_sorular_listesi']
  <QuerySet [<Soru: Yenilikler ne?>]>
</code></pre>

<hr>

### Görünümü geliştiriyoruz

Anketler listesi henüz yayınlanmamış anketleri (yani gelecekte olan bir yayim_tarihi olan anketleri) göstermektedir. Onu düzeltelim.

[Öğretici 4]({{site.belgeler_ogretici4}})'de, ListeView temel alınarak sınıf tabanlı bir görünüm sunduk:

anketler/views.py
<pre data-gnl="1 1p"><code class="language-python">
  class IndexView(generic.ListView):
      tema_adi = 'anketler/index.html'
      context_object_name = 'son_sorular_listesi'

      def get_queryset(self):
          """En son yayınlanan beş soruya cevap verin."""
          return Soru.objects.order_by('-yayim_tarihi')[:5]
</code></pre>

get_queryset() yöntemini değiştirmeli ve timezone.now() ile karşılaştırarak tarihini kontrol etmeliyiz. Önce bir içe aktarma yapalım:

anketler/views.py
<pre data-gnl="1 1p"><code class="language-python">
  from django.utils import timezone
</code></pre>

ve sonra get_queryset yöntemini şöyle değiştirmeliyiz:

anketler/views.py
<pre data-gnl="1 1p"><code class="language-python">
  def get_queryset(self):
      """
      En son yayınlayann beş soruyu geri gönderin (belirlenenler hariç gelecekte yayınlanacaktır).
      """
      return Soru.objects.filter(
          yayim_tarihi__lte=timezone.now()
      ).order_by('-yayim_tarihi')[:5]
</code></pre>

Soru.objects.filter (yayim_tarihi__lte=timezone.now()), yayim_tarihi değeri veya timezone.now'dan daha eski veya eşit olan soruları içeren bir sorgu dizesi döndürür.

<hr>

### Yeni görünümü sınıyoruz

Artık, bunun çalışma sunucusunu çalıştırıp siteyi tarayıcınıza yükleyebilirsiniz. Geçmiş ve gelecek tarihi olan Sorular oluşturarak ve yalnızca yayınlanmış olanları listediğini kontrol ederek beklendiği gibi davrandığından emin olabilirsiniz. Bunu etkileyebilecek her değişiğikliği yaptığınızda bunu yapmak zorunda kalmak istemezsiniz. Dolayısıyla, yukarıdaki kabuk oturumumuza dayanarak bir sınama oluşturalım.

anketler/tests.py dosyasına aşağıdakileri ekleyin:

anketler/tests.py
<pre data-gnl="1 1p"><code class="language-python">
from django.urls import reverse
</code></pre>

ve soru oluşturmak için bir kısayol işlevi oluşturacağız. Bir de yeni bir sınama sınıfı oluşturacağız:

anketler/tests.py
<pre data-gnl="1 1p"><code class="language-python">
  def soru_olustur(soru_metni, gunler):
      """
      Verilen 'soru_metni' ile bir soru oluşturun ve şimdiye kadar verilen belirli gün sayısını yayınlayın (geçmişte yayınlanan sorular için eksi, henüz yayınlanmayan sorular için artı kullanın).
      """
      time = timezone.now() + datetime.timedelta(days=gunler)
      return Soru.objects.create(soru_metni=soru_metni, yayim_tarihi=time)


  class SoruIndexViewTests(TestCase):
      def sinama_soru_yok(self):
          """
          Hiçbir soru yoksa, uygun bir mesaj görüntülenir.
          """
          response = self.client.get(reverse('anketler:index'))
          self.assertEqual(response.status_code, 200)
          self.assertContains(response, "Uygun anket yok.")
          self.assertQuerysetEqual(response.context['son_sorular_listesi'], [])

      def sinama_eski_soru(self):
          """
          yayim_tarihi eski olan sorular.
          """
          soru_olustur(soru_metni="Eski soru.", days=-30)
          response = self.client.get(reverse('anketler:index'))
          self.assertQuerysetEqual(
              response.context['son_sorular_listesi'],
              ['<Soru: Eski soru.>']
          )

      def sinama_gelecekteki_soru(self):
          """
          yayim_tarihi gelecek bir tarihte olan sorular görüntülenmez
          """
          soru_olustur(soru_metni="Gelecekteki soru.", days=30)
          response = self.client.get(reverse('anketler:index'))
          self.assertContains(response, "Uygun anket yok.")
          self.assertQuerysetEqual(response.context['son_sorular_listesi'], [])

      def sinama_gecmis_ve_gelecekteki_sorular(self):
          """
          Hem geçmiş hem de gelecekteki sorular mevcutsa, yalnızca geçmiş sorular görüntülenir.
          """
          soru_olustur(soru_metni="Eski soru.", days=-30)
          soru_olustur(soru_metni="Gelecekteki soru.", days=30)
          response = self.client.get(reverse('anketler:index'))
          self.assertQuerysetEqual(
              response.context['son_sorular_listesi'],
              ['<Soru: Eski soru.>']
          )

      def sinama_iki_eski_soru(self):
          """
          Sorular dizin sayfası birden fazla soru görüntüleyebilir
          """
          soru_olustur(soru_metni="1. eski soru", days=-30)
          soru_olustur(soru_metni="2. eski soru", days=-5)
          response = self.client.get(reverse('anketler:index'))
          self.assertQuerysetEqual(
              response.context['son_sorular_listesi'],
              ['<Soru: 2. eski soru>', '<Soru: 1. eski soru>']
          )
</code></pre>

Bunlardan bazılarına daha yakından bakalım

Birincisi, soru oluşturma sürecinin bir kısmını tekrarlamak için bir soru kısayol işlevi soru_olustur'dur

sinama_soru_yok herhangi bir soru oluşturmaz, ancak iletiyi kontrol eder: "Anket yok" ve son_sorular_listesi'n boş olduğunu doğrular. Django.test.TestCase sınıfının bazı ek onaylama yöntemleri sağladığını unutmayın. Bu örneklerde, assertContains() ve assertQuerysetEqual() kullanıyoruz.

sinama_eski_soru'da bir soru oluşturuyoruz ve sorunun listede göründüğünü doğruluyoruz.

sinama_gelecekteki_soru'da yayim_tarihi gelecekte bir tarihte olan soru oluşturuyoruz. Veritabanı her sınama yöntemi için sıfırlanır, böylece ilk soru artık yoktur ve dolayısıyla dizinin içinde herhangi bir soru bulunmaması gerekir.

Ve bunun gibi... Aslında, sınamaları, sitede yönetici girdisi ve kullanıcı deneyimi hikayesini anlatmak ve bunu her bölgede ve örgünün her yeni durumu için kontrol ederek kullandığımız zaman, beklenen sonuçlar yayınlanmaktadır.

<hr>

### AyrintiView'u sınamak

Elimizde ne var? Ancak gelecekteki sorular sayfada görünmese de doğru URL'yi bildikleri veya tahmin ettikleri kullanıcılar yine de onlara ulaşabilir. Dolayısıyla, AyrintiView'e benzer bir kısıt koymamız gerekiyor:

anketler/views.py
<pre data-gnl="1 1p"><code class="language-python">
  class AyrintiView(generic.DetailView):
      ...
      def get_queryset(self):
          """
          Henüz yayınlanmamış sorular hariç
          """
          return Soru.objects.filter(yayim_tarihi__lte=timezone.now())
</code></pre>

Ve elbette, yayim_tarihi geçmişte olan bir sorunun gösterilebileceğini ve gelecekte bir yayim_tarihi olanın görüntülenemeyeceğini kontrol etmek için bazı sınamalar ekleyeceğiz:

anketler/tests.py
<pre data-gnl="1 1p"><code class="language-python">
  class SoruAyrintiViewTests(TestCase):
  def sinama_gelecekteki_soru(self):
      """
      Gelecekte bir yayim_tarihi içeren sorunun ayrıntılı görünümü bulunamadı 404 notunu döndürür.
      """
      gelecekteki_soru = soru_olustur(soru_metni='Gelecekteki soru.', days=5)
      url = reverse('anketler:ayrinti', args=(gelecekteki_soru.id,))
      response = self.client.get(url)
      self.assertEqual(response.status_code, 404)

  def sinama_eski_soru(self):
      """
      Geçmiş tarihli yayim_tarihi olan bir sorunun ayrıntılı görünümünü sorunun metniyle görüntüler.
      """
      eski_soru = soru_olustur(soru_metni='Eski soru.', days=-5)
      url = reverse('anketler:ayrinti', args=(eski_soru.id,))
      response = self.client.get(url)
      self.assertContains(response, eski_soru.soru_metni)
</code></pre>

<hr>

### Daha fazla sınama için fikirler

SonuclarView^'e benzer bir get_queryset yöntemi eklemeliyiz ve bu görünüm için yeni bir sınama sınıfı oluşturmalıyız. Yeni oluşturduklarımızla çok benzer olacak. Aslında çok fazla tekrar olacaktır.'

Ayrıca, uygulamayı diğer yollarla iyileştirebiliriz. Bu yol boyunca sınamalar ekleyebiliriz. Örneğin; soruların sitede hiçbir seçeneğe sahip olmayacak şekilde yaınlanması saçmadır. Dolayısıyla, görünümüz bunu kontrol edebilir ve bu tür soruları hariç tutabilir. Sınamalarımız seçimler olmadan bir soru oluşturur ve yayınlanmadığını sınar. Ayrıca seçimler ile benzer bir soru oluşturur ve yayınlandığını sınar.

Belki de oturum açmış olan yönetici kullanıcılarının, yayınlanmamış soruları görmesine izin verilmelidir. Sıradan ziyaretçilere izin verilmemelidir. Yine: bunu gerçekleştirmek için yazılıma ne eklenmesi gerekiyorsa, sınamayı ilk önce yazıp sonrada sınamaya geçip geçmemesine ya da mantığı önce kodunuzda yapmaya sonra da bir sınama yapmaya özen gösterin.

Belirli bir noktada, sınamalarınıza bakmanız ve kodunuzun sınama şişkinliği çeken olup olmadığını merak ediyorsunuz ve bu bize aşağıdakileri getiriyor:

<hr>

## Sınarken daha iyidir

Sınamalarımız kontrol dışı kaldığı anlaşılıyor. Bu oranda sınamalarımızda bizim uygulamalarımızdan daha fazla kod olacak ve tekrar yapmak estetik değil. Kodumuzun geri kalanının zarif oluşuna nazaran...

Önemli değil. Şişmelerine izin ver. Çoğunlukla, bir kez bir sınama yazılabilir ve ardından unutabilirsiniz. Programınızı geliştirmeye devam ederken yararlı işlevini yerine getirmeye devam edecektir.

Bazen sınamaların güncellenmesi gerekecektir. Düşüncelerimizi değiştirdiğimizi ve böylece yalnızca seçenekli soruların yayınlandığını varsayalım. Bu durumda, mevcut sınamalarımızın bir çoğu başarısız olur. Bize tam olarak hangi sınamaların güncelleştirileceğini bildirdiklerini, böylece sınamaların kendilerine bakmalarına yardımcı olur.

En kötü ihtimalle, gelişmeye devam ederken, artık gereksiz olan bazı sınamalarımız olduğunu göreceksiniz. Bu bile bir sorun değil. Artık sınamalar iyi bir şeydir.

Sınamalarınız mantıklı bir şekilde düzenlendiği sürece, yönetilemez olmaz. İyi kurallar şunları içerir:

{.liste}
* Her kalıp veya görünüm için ayrı bir sınama sınıfı (TestClass)
* sınamak istediğiniz her koşul kümesi için ayrı bir sınama yöntemi
* İşlevlerini tanımlayan sınama yöntem adları

<hr>

## Daha fazla sınama

Bu eğitimde yalnızca sınama temellerinin bazıları tanıtılacaktır. Yapabileceğiniz çok şey var ve bazı çok akıllıca şeyleri elde etmek için elinizin altında çok sayıda yararlı araç var.

Örneğin, burada yaptığımız sınamalar, bir kalıbın dahili mantığının bazıları ve görünümlerimizin yayınlanma biçimi hakkında bilgi verirken, HTML'nizin bir tarayıcıda gerçekte nasıl işlendiğini sınamak için Selenyum gibi bir "tarayıcı içi" çerçeve kullanabilirsiniz. Bu araçlar, yalnızca sizin Django kodunuzun davranışını değil, aynı zamanda örneğin JavaScript'inizin davranışınızı kontrol etmenizi sağlar. Sınamaların bir tarayıc başlattığını görmek ve sitenizle etkileşime girmeye başlamak için sanki bir insan kullanıyor gibi davranacak! Django, Selenyum gibi araçlar ile bütünleştirmeyi kolaylaştırma için LiveServerTestCase'yi içerir

Karmaşık bir uygulamanız varsa, kalite kontrolünün kendisinin en azından kısmen doğal hale getirilebilmesi için, sürekli bütünleştirme amacıyla her taahüdünde doğal olarak sınamalar yapmak isteyebilirsiniz.

Ugulamanızın denenmemiş kısımlarını bulmanın iyi bir yolu, kod kapmasını kontrol etmekdir. Bu aynı amanda kırılgan veya ölü kodu tanımlamaya yardımcı lur. Bir parçayı sınayamazsınız, genellikle kodun yeniden biçimlendirilmesi veya kaldırılması gerektiği anlamına gelir. Kapsam, ölü kodu tanımlamanıza yardımcı olacaktır. Ayrıntılar için coverage.py ile bütünleştirme bölümüne bkaın.

Django'da sınama hakkında kapsamlı bilgi içerir.

<hr>

## Sırada ne var?

Sınama ile ilgili tüm ayrıntılar için [Django'da Sınama](#) konusuna bakınız.

Django görünümlerini sınamaya razı olduğunuzda, sabit dosya yönetimi hakkında bilgi edinmek için [Öğretici 6]({{site.belgeler_ogretici6}}) bölümünü okuyun.
