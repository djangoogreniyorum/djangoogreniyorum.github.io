---
layout: general
title: Kurulum - Django Öğreniyorum
---
# Sıradaki Konu Ne?

Yani tüm [giriş konuları](/en/2.0/intro/)nı okudunuz ve Django'yu kullanmaya devam etmek istediğinize karar verdiniz. Bu tanıtım ile sadece yüzeysel çizdik (aslında, her bir sözcüğü okuduysanız, genel belgelerin yaklaşık 5%'ini okudunuz demektir).

## Sırada ne var?

Eh, her an uygulayarak öğrenmenin büyük taraftarıydık. Bu durumda kendinize ait bir projeye başlamanız bizim kadar yeterli bilgiye sahip olmanız gerekir. Yeni püf noktaları öğrenmeniz gerektiğinde, belgelere geri dönün.

Django'nun belgeleri yararlı, okunması kolay ve olabildiğince eksiksiz hale getirmek için çok çaba harcadık. Bu belgenin geri kalan kısmı, belgelerden en iyi şekilde yararlanmak için nasıl çalıştığı hakkında daha fazla bilgi verir.

(Evet, bu belgelerle ilgili bir belge.) Belgelerle ilgili böyle bir belgenin belgenin okunmasına yönelik önceden bir belge yazacağına dair bir planımız olmadığından emin olabilirsiniz.

## Belgeleri bulmak

Django'nun çok fazla belgesi var. Neredeyse 450.000 sözcük ve artan. Bu yüzden neye ihtiyaç duyduğunuzu bulmak sizin açınızdan bazen zorlayıcı olabilir. Başlamak için birkaç iyi yer söyleyelim. [Arama sayfası](#) ve [dizin](#) (index).

Veya sayfalarda gelişi güzel gezinin!

<hr>

## Belgeler Nasıl Düzenleniyor?

Django'nun temel belgeleri farklı ihtiyaçları karşılamak üzere tasarlanarak "parçalara" bölünmüştür.

- [Giriş belgesi](/en/2.0/intro/), Django'ya yeni gelen insanlar için veya genel olarak ağ geliştirmeye gelenlere yöneliktir. Derinliği olan herhangi bir şeyi kapsamak. Bunun yerine Django'nun "hislerini" anlatan üst düzey bir bakış sunar.
- Öte yandan, [konu başlıkları](#), Django'nun ayrı bölümlerine derinlemesine dalıyor. Django'nun [kalıp örgüsü (model system)](#), [şablon motoru (template engine)](#), [biçim çerçevesi (forms framework)](#) ve daha fazlası için eksiksiz kılavuzlar var.
- Olasılıkla zamanımızın çığunu harcamak istediğiniz yer burasıdır; kılavuzlarla yolunuzu bulursanız, Django hakkında bilmek zorunda olduğunuz her şeyi bilmeniz gerekir.
- Ağ geliştirme genellikle belirgindir, derin değildir. Sorunlar birçok alana yayılır. Sık karşılaşdığımız "Nasıl yaparım" sorularına cevap veren bir dizi "[Nasıl yapılır?](#)" kılavuzları yazdık. Burada, Django ile PDF'ler oluşturma, özel şablon etiketleri yazma ve daha fazlası hakkında bilgi bulacaksınız.

   Sıkça sorulan sorulara verilen cevaplar [SSS](#) bölümünde de bulunabilir.
- Kılavuzlar ve nasıl yapılır konuları Django'da bulunan her sınıfı, işlevi ve yöntemi kapsamaz. Öğrenmeye çalıştığınızda çok zor olurdu. Bunun yerine, bireysel sınıflar, işlevler, yöntemler ve modüller hakkında ayrıntılar kaynak olarak sağlanır. Burası belirli bir işlevin veya ihtiyacınız olan her şeyin ayrıntılarını bulmaya dönüşeceğiniz yerdir.
- Toplu kullanım bir proje dğaıtmak istiyorsanız, belgelerimizde çeşitli dağıtım kurulumları için birkaç rehberin yanı sıra düşünmeniz gereken bazı şeyler için bir dağıtım kontrol listesi bulunmaktadır.
- Son olarak, genellikle çoğu geliştirici ile ilgili olmayan bazı "özel" belgeler var. Bu, Django'ya kod eklemek isteyenlerin [sürüm notlarını](#) ve [dahili belgeleri](#). Ve başka yerlere sığmayan [birkaç şeyi içerir](#).

<hr>

## Belgeler nasıl güncellenir

Django kod tabanı tıpkı geliştirilmiş ve iyileştirilmiş günlük gibidir. Çeşitli nedenlerle belgeleri iyileştiriyoruz:

- Dilbilgisi / yazım hatası düzeltmeleri gibi içerik düzeltmeleri.
- Genişletilmesi gereken mevcut bölümlere bilgi veya örnek eklemek.
- Belgelendirilmemeiş Django özelliklerini belgelemek. (Bu tür özelliklerin listesi azalıyor ancak hala var.)
- Yeni özellikler için belgeler eklemek. Yeni özellikler eklendiğinde veya Django API'leri ile davranışları değiştikçe.

Django belgeleri koduyla aynı kaynak yönetim örgüsünde tutulur. Git deponuzun docs dizininde bulunur. Çevrimiçi olan her belge depoda ayrı bir metin dosyasıdır.

## Nereden Edinirim?

Django belgelerini çeşitli şekillerde okuyabilirsiniz. bunlar tercih sırasına göre

### Ağ Üzerinde (İnternette)

Django belgelerinin en son sürümü [https://docs.djangoproject.com/en/dev/](https://docs.djangoproject.com/en/dev/) adresinde bulunmaktadır. Bu HTML sayfaları, kaynak denetimindeki metin dosyalarından doğal olarak oluşturulur. Bu, Django'da "en yeni ve en büyükleri" yansıttığı anlamına gelir; en yeni düzeltmeleri ve eklemeleri içerir ve sağdece Django geliştirme sürümünün kullanıcıları tarafından kullanılabilir olan en yeni Django özelliklerini tartışırlar. (Aşağıdaki "sürümler arasındak farklar" bölümüne bakın)

Bilet örgüsünde değişiklikler, düzeltmeler ve öneriler göndererek belgeleri iyileştirmeye yardımcı olmanızı öneririz. Django geliştiricileri, bilet örgüsünü etkin olarak izler ve herkes için belgeleri geliştirmek üzere görüşlerinizden faydalanırlar.

Bununla birlikte, biletlerin geniş teknik destek sorularını sormak yerine açıkça belgelerle ilişkili olması gerektiğini unutmayın. Özel Django kurulumunuzla ilgili yardıma ihtiyacınız varsa, bunun yerine django kullanıcıları posta listesi veya #django IRC kanalı deneyin.

### Düz metin olarak

Çevrimdışı okumak veya sadece kolaylık sağlamak için Django belgelerini düz metinde okuyabilirsiniz.

Django'nun resmi bir sürümünü kullanıyorsanız, kodun sıkıştırılmış paketinin bu sürüm için belgeleri içeren dizin içerdiğine dikkat edin.

Django (aka "trunk") geliştirme sürümünü kullanıyorsanız, dizinin tüm belgeleleri içerdiğini unutmayın. En son değişiklikleri almak için Git hesabınızı güncelleyebilirsiniz.

Metin belgelerinden yararlanmanın düşük teknoloji bir yolu, tüm belgelerde bir cümle aramak için Unix grep yardımcı programını kullanmaktadır. Örneğin, bu size herhangi bir Django belgesinde "max_length" ifadesinin her bir sözünü gösterecektir:

  <pre data-gnl="1 1p"><code class="language-python">
  $ grep -r max_length /path/to/django/docs/
  </code></pre>

### HTML gibi, yerelce

HTML belgelerinin yerel bir kopyasını birkaç basit adresten edinebilirsiniz:

- Django2nun belgeleri düz metinden HTML'ye dönüştürmek için Sphinx adlı bir örgü kullanır. Paketi Sphinx ağ sitesinden indirerek veya pip kullanarak kurmanız gerekir:

   <pre data-gnl="1 1p"><code class="language-python">
    $ grep -r max_length /path/to/django/docs/
    </code></pre>

- Ardından belgeleri HTML'ye çevirmek için sadece birlikte gelen Makefile'yi kullanın:

   <pre data-gnl="1 1p"><code class="language-python">
   $ cd path/to/django/docs
   $ make html
   </code></pre>

   Bunun için [GNU Make](https://www.gnu.org/software/make/)'in kurulu olması gerekir.

   Eğer Windows işletim örgüsünü kullanıyor iseniz, seçenek olarak birlikte verilen toplu iş dosyasını kullanabilirsiniz:

   <pre data-gnl="1 1p"><code class="language-python">
   cd path&#92;to&#92;django&#92;docs
   make.bat html
   </code></pre>

   HTML bekleri docs/&#95;build/html ye yerleştirilecektir.

<hr>

### Sürümler arasındaki farklar

Daha önce de belirttiğimiz gibi, Git deponuzdaki metin belgeleri "en yeni ve en büyük" değişiklikleri ve eklemeleri içerir. Bu değişiklikler genellikle Django geliştirme sürümünde - Django'nun git ("trunk") sürümünde eklenen yeni özelliklerin belgelerini içerir. Bu nedenle, çerçevenin çeşitli sürümleri için belgeleştirmenin düz tutulması konusundaki yordamlarımıza değinmek gerekir.

### Bu yordamı uyguluyoruz:

- djangoproject.com hakkında birincil belgeler Git'deki en son belgelerden oluşuan bir HTML sürümüdür. Bu belgeler her zaman en son resmi Django sürümüne ve ayrıca en son sürümden bu yana çerçevede eklenen/değiştirilen özelliklere karşılık gelir.
- Django'nun geliştirme sürümüne özellikler ekledikçe, aynı Git tamamlama işlemindeki belgeleri güncellemeye çalışıyoruz.
- Belgelerdeki özellik değişikliklerini / eklemelerini ayırt etmek için şu ifadeyi kullanırız: "X.Y sürümünde yeni", bir sonraki sürüm olan X.Y olmak üzere (dolayısıyla geliştirilen).
- Belgeleştirme düzeltmeleri ve iyileştirmeler, yetkililerin takdirine bağlı olarak son sürüm birliğine geri gönderilebilir; ancak, Django sürümünün artık desteklenmemesi durumunda, belgeler bu sürüm ile ilgili başka güncelleme almaz.
- Ana belgeleştirme ağ sayfası, önceki tüm sürümlerin belgelerine bağlantılar içerir. Kullanmakta olduğunuz Django sürümüne karşılık gelen belgeler sürümünü kullandığınızdan emin olun!
