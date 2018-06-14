---
layout: general
title: Öğretici 2 - Django Öğreniyorum
---
# İlk Django Uygulamanızı Yazma, Bölüm 2 {#writing-your-first-django-app-part-2}

Bu öğretici, [öğretici 1](/en/2.0/intro/tutorial01/)'in kaldığı yerden devam ediyor. Veritabanını kuracağız, ilk kalıbımızı oluşturacağız ve Django'nun doğal olarak oluşturulan yönetici sitesine hızlı bir giriş yapacağız.

## Veritabanu Kurulumu {#database-setup}

Şimdi, **benimsite/settings.py** dosyasını açın. Bu dosya, Django ayarlarını temsil eden kalıp düzerinde değişkenleri olan normal bir Python modülüdür.

Varsayılan olarak yapılandırılmış veritabanı SQLite'dır. Veritabanları konusunda yeniyseniz veya sadece Django'yu denemekle ilgileniyorsanız, bu en kolay seçimdir. SQLite Python'a dahildir. Bu nedenle veritabanınızı desteklemek için başka bir şey yüklemeniz gerekmez. Bununla birlikte, ilk gerçek projenizi başlatırken, veritabanı geçişindeki baş ağılarından kaçınmak için PostgreSQL gibi daha ölçeklenebilir bir veritabanı kullanmak isteyebilirsiniz.
Başka bir veritabanı kullanmak isterseniz, uygun [veritabanı bağlamalarını](/en/2.0/topics/install/#database-installation) yükleyin ve aşağıdaki anahtarları [DATABASES](/en/2.0/ref/settings/#std:setting-DATABASES)'ın varsayılan ('**default**') öğesinde veritabanı bağlantı ayarlarınıza uyacak şekilde değiştirin:


- [**ENGINE**](/en/2.0/ref/settings/#std:setting-DATABASE-ENGINE): '**django.db.backends.sqlite3**', '**django.db.backends.postgresql**', '**django.db.backends.mysql**' veya '**django.db.backends.oracle**'. [Diğerarka uçlar](/en/2.0/ref/databases/#third-party-notes)da mevcuttur.
- [**NAME**](/en/2.0/ref/settings/#std:setting-NAME): Veritabanızın adı. SQLite kullanıyorsanız, veritabanı bilgisayarındaki bir dosya olacaktır. Bu durumda, [**NAME**](/en/2.0/ref/settings/#std:setting-NAME)</a> dosyasının dosya adı da dahil olmak üzere tam mutlak yol olmalıdır. Varsayılan değer olan **os.path.join(BASE_DIR, 'db.sqlite3')**, dosyayı proje dizininizde depolar.

SQLite veritabanınız olarak kullanılıyorsa, [**USER** (kullanıcı)]([NAME](/en/2.0/ref/settings/#std:setting-NAME)), [PASSWORD (şifre)](/en/2.0/ref/settings/#std:setting-PASSWORD) ve [**HOST** (sunucu)](/en/2.0/ref/settings/#std:setting-HOST) gibi ek ayarlar eklenmelidir. Daha fazla bilgi için, [**DATABASES**](/en/2.0/ref/settings/#std:setting-DATABASES) etkinleştirme belgelerine bakın.

<div data-bilget="genel" markdown="1">
### SQLite dışındaki veritabanları için

SQLite dışıdna bir veritabanı kullanıyorsanız, bu noktaya kadar bir veritabanı oluşturduğunuzdan emin olun. Bunu "**CREATE DATABASE database_name;**" ile veritabanınızın etkileşimli isteminde yapın.

Ayrıca, **benimsite/settings.py** dosyasında sağlanan veritabanı kullanıcısının "veritabanı oluştur" ayrılacılıklarına sahip olduğundan emin olun. Bu, daha sonraki bir öğreticide ihtiyaç duyulacak bir [sınama veritabanının](/en/2.0/topics/testing/overview/#the-test-database) doğal olarak oluşturulmasını sağlar.

SQLite kullanıyorsanız, önceden bir şey oluşturmanız gerekmez - veritabanı dosyası gerektiğinde doğal olarak oluşacaktır.
</div>

**benimsite/settings.py** dosyasını düzenlerken, [**TIME_ZONE**](/en/2.0/ref/settings/#std:setting-TIME_ZONE) değerini sat diliminizde ayarlayın.

Ayrıca, dosyanın üst kısmındaki [**INSTALLED_APPS**](/en/2.0/ref/settings/#std:setting-INSTALLED_APPS) ayarına dikkat edin. Bu Django örneğinde etkinleştirilen tüm Django uygulamalarının adlarını tutar. Uygulamalar birden fazla projede kullanılabilir ve bunları başkaları tarafından projelerinde kullanılmak üzere paketleyebilir veya dağıtabilirsiniz.

Varsayılan olarak, [**INSTALLED_APPS**](/en/2.0/ref/settings/#std:setting-INSTALLED_APPS), hepsi Django ile birlikte gelen aşağıdaki uygulamaları içerir:

- [**django.contrib.admin**](/en/2.0/ref/contrib/admin/#module-django.contrib.admin): Yönetici sitesi. Kısa süre kullanacaksın.
- [**django.contrib.auth**](/en/2.0/topics/auth/#module-django.contrib.auth): Bir kimlik doğrulama örgüsü.
- [**django.contrib.contenttypes**](/en/2.0/ref/contrib/contenttypes/#module-django.contrib.contenttypes): İçerik türleri için bir çatı.
- [**django.contrib.sessions**](/en/2.0/topics/http/sessions/#module-django.contrib.sessions): Bir oturum çatısı.
- [**django.contrib.messages**](/en/2.0/ref/contrib/messages/#module-django.contrib.messages): Bir mesajlaşma çatısı.
- [**django.contrib.staticfiles**](/en/2.0/ref/contrib/staticfiles/#module-django.contrib.staticfiles): Sabit dosyaları yönetmek için bir çatı.

Bu uygulamalar, genel durum için bir kolaylık olarak varsayılan olarak bulunur.

Bu uygulamalardan bazıları en az bir veritabanı tablosu kullanıyor, bu nedenle tabloları kullanabilmeniz için önce veritabanında oluşturmamız gerekiyor. Bunu yapmak için aşağıdaki komutu çalıştırın:

  <pre data-gnl="1 1p"><code class="language-python">
  $ python manage.py migrate
  </code></pre>

[**migrate (Göç)**](/en/2.0/ref/django-admin/#django-admin-migrate) komutu, [**INSTALLED_APPS**](/en/2.0/ref/settings/#std:setting-INSTALLED_APPS) ayarına bakar ve **benimsite/settings.py** dosyanızdaki veritabanı ayarlarına ve uygulama ile birlikte gelen veritabanı göçlerine göre gerekli tüm veritabanı tablolarını oluşturur (bunları daha sonra ele alacağız). Uygulandığı her bir taşıma işlemi için bir ileti görürsünüz. Eğer ilgileniyorsanız, veritabanınız için komut satırı istemcisini çalıştırın ve

- **\dt** (PostgreSQL),
- **SHOW_TABLES;** (MySQL),
- **.schema** (SQLite)
- **SELECT TABLE_NAME FROM USER_TABLES;** (Oracle)

Django'nun oluşturduğu tabloları görüntülemek için kullanın.

<div data-bilget="genel">
### Minimalistler için

Yukarıda söylediğimiz gibi, varsayılan durum, genel durum için dahil edilmiş ancak herkesin bunlara ihtiyacı yok. Bunların herhangi birine veya tamamına ihtiyacınız yoksa, [**INSTALLED_APPS**](/en/2.0/ref/settings/#std:setting-INSTALLED_APPS)çalıştırmadan önce [**INSTALLED_APPS**](/en/2.0/ref/settings/#std:setting-INSTALLED_APPS) uygun satırları yorumlamaktan veya silmekten çekinmeyin. [**migrate (Göç)**](/en/2.0/ref/django-admin/#django-admin-migrate) komutu yalnızca [**INSTALLED_APPS**](/en/2.0/ref/settings/#std:setting-INSTALLED_APPS) uygulamasındaki [**migrate (Göç)**](/en/2.0/ref/django-admin/#django-admin-migrate) işlemlerini [**INSTALLED_APPS**](/en/2.0/ref/settings/#std:setting-INSTALLED_APPS).
</div>

<hr>

## Kalıp (Model) Oluşturma {#creating-models}

Şimdi, modellerinizi - aslında, veritabanı düzeninizle (ek meta verilerle) tanımlayacağız.

<div data-bilget="genel" markdown="1">
### Felsefe

Bir kalıp, verilerinizle ilgili gerçeğin tek ve kesin kaynağıdır. Sakladığınız verilerin önemli alanlarını ve davranışlarını içerir. Django, [KURU Prensibi](/en/2.0/misc/design-philosophies/#dry)'ni izler. Amaç, veri modelinizi tek bir yerde tanımlamak ve bunlardan doğal olarak türetmektir.

Bu, göçleri de içerir - örneğin Ruby On Rails'in aksine, göçler tamamen modeller dosyanızdan türetilir ve aslında yalnızca mevcut modellerinize uyacak şekilde veritabanı şemasını güncellemek için Django'nun kullanabileceği bir geçmişi vardır.
</div>

Basit anket uygulamamızda, iki kalıp oluşturacağız: **Soru** ve **Seçim** . Bir sorunun bir soru ve yayın tarihi vardır. **Seçim** iki alana sahiptir: seçim metni ve oy toplaması. Her **Seçim** bir Soru ile ilişkilendirilir.

anketler/models.py
  <pre data-gnl="1 1p"><code class="language-python">
  from django.db import models


  class Soru(models.Model):
      soru_metni = models.CharField(max_length=200)
      yayim_tarihi = models.DateTimeField('date published')


  class Secim(models.Model):
      soru = models.ForeignKey(Soru, on_delete=models.CASCADE)
      secim_metni = models.CharField(max_length=200)
      oylar = models.IntegerField(default=0)
  </code></pre>

Kod basittir. Her model, [**django.db.models.Model**](/en/2.0/ref/models/instances/#django.db.models.Model) alt sınıflarının sınıfıyla temsil edilir. Her modelin sınıf değişkenleri vardır, bunların her biri modelin bir veritabanı alanını temsil eder.

Her alan bir [**Field**](/en/2.0/ref/models/fields/#django.db.models.Field) sınıfı örneği ile temsil edilir - örneğin, karakter alanları için [**CharField**](/en/2.0/ref/models/fields/#django.db.models.CharField) ve tarih [**CharField**](/en/2.0/ref/models/fields/#django.db.models.CharField) için [**DateTimeField**](/en/2.0/ref/models/fields/#django.db.models.DateTimeField) . Bu, Django'ya her alanın ne tür verilere sahip olduğunu söyler.

Her bir [**Field**](/en/2.0/ref/models/fields/#django.db.models.Field) örneğinin adı (örneğin; **soru_metni** veya **yayim_tarihi**) makinenin dostu biçiminde olduğu alanın adıdır. Bu değeri Python kodunuzda kullanacaksınız ve veritabanınız bu sütun adı olarak kullanılacaktır.

İnsanlar tarafından okunabilen bir isim belirlemek için [**Field**](/en/2.0/ref/models/fields/#django.db.models.Field) isteğe bağlı olarak ilk konumsal durumunu kullanabilirsiniz. Bu, Django'nun içgözlem birkaç bölümünde kullanılır ve belgelendirme olarak iki katına çıkar. Bu alan sağlanmazsa, Django makine tarafından okunabilen adı kullanacaktır. Bu örnekte, yalnızca **Soru.yayim_tarihi** için insan taraınfan okunabilen bir tanım tanımladık. Bu modeldeki diğer tüm alanlar için alanın makine tarafından okunabilen adı insan tarafından okunabilir bir ad olarak yeterlidir.  

Bazı [**Field**](/en/2.0/ref/models/fields/#django.db.models.Field) sınıflarının durumları gereklidir. Örneğin, [**CharField**](/en/2.0/ref/models/fields/#django.db.models.CharField), size bir [**max_length**](/en/2.0/ref/models/fields/#django.db.models.CharField.max_length) gerektirir. Bu, yalnızca veritabanı şemasında değil, doğrulamada da kullanılmaktadır. Yakında yakından göreceğiz.

Bir [**Field**](/en/2.0/ref/models/fields/#django.db.models.Field) ayrıca çeşitli isteğe bağlı bağımsız değişkenlere sahip olabilir; Bu durumda, oylar alanının [**default**](/en/2.0/ref/models/fields/#django.db.models.Field.default) **değeri** 0 olması gerekir.  

Son olarak [**ForeignKey**](/en/2.0/ref/models/fields/#django.db.models.ForeignKey) kullanarak bir ilişkinin tanımlandığını unutmayın. Django'ya her **Seçim**'in tek bir **Soru** ile alakalı olduğunu söyler. Django, tüm ortak veritabanı ilişkilerini desteklemektedir. Bire-bir, bire-çok, çoğa-bir, çoğa-çok  

<hr>

## Kalıpları (Model) Etkinleştirme {#activating-models}

Model kodun birazı bile Django'ya çok fazla bilgi verir. Bununla birlikte, Django şunları yapabilir:

- Bu uygulama için bir veritabanı şeması oluşturun (**CREATE TABLE** ifadeleri)
- **Soru** ve **Seçim** nesnelerine erişmek için bir Python veritabanı erişim API'si oluşturun.

Ancak önce projemize **anketler** uygulamasının yüklenmesini söylemeleyiz.
<div data-bilget="genel" markdown="1">
### Felsefe

Django uygulamaları "takılabilir": Bir uygulamayı birden fazla projede kullanabilirsiniz ve belirli bir Django yüklemesine bağlı olmak zorunda olmadıkları için uygulamaları dağıtabilirsiniz.
</div>

Uygulamayı projemize eklemek için [**INSTALLED_APPS**](/en/2.0/ref/settings/#std:setting-INSTALLED_APPS) ayar bölümünde yapılandırma sınıfına bir kaynakça eklememiz gerekiyor. **AnketlerAyar** sınıfı **anketler/apps.py** dosyasında yer alır. Bu nedenle noktalı yolu '**anketler.apps.AnketlerAyar**' bu şekildedir. **benimsite/settings.py** dosyasını düzenleyin ve bu noktalı yolu [**INSTALLED_APPS**](/en/2.0/ref/settings/#std:setting-INSTALLED_APPS) ayarına ekleyin. Dosya şöyledir:

benimsite/settings.py
<pre data-gnl="1 1p"><code class="language-python">
  INSTALLED_APPS = [
       'anketler.apps.AnketlerAyar' ,
       'django.contrib.admin' ,
       'django.contrib.auth' ,
       'django.contrib.contenttypes' ,
       'django.contrib.sessions' ,
       'django.contrib.messages'
       'django.contrib.staticfiles' ,
   ]
</code></pre>

Şimdi Django **anketler** uygulamasını projeye nasıl eklendiğini biliyorsunuz. Örgüye tamamen dahil etmek için devam ediyoruz. Aşağıdaki komutu çalıştırın.

<pre data-gnl="1 1p"><code class="language-python">
  $ python manage.py makemigrations anketler
</code></pre>

Sonuç olarak şöyle bir karşılık almalısınız:
<pre data-gnl="1 1p"><code class="language-python">
  Migrations for 'anketler':
    anketler/migrations/0001_initial.py:
      - Create model Secim
      - Create model Soru
      - Add field soru to secim
</code></pre>

**makemigrations** çalıştırarak, modellerinizde bazı değişiklikler yaptınız (bu durumda yeni olanları) ve değişikliklerin bir geçiş olarak saklanmasını istediğinizi Django'ya belirttiniz.

Göçler (migrations), Django'nun modellerinizdeki değişiklikleri depolamasını sağlar. Bunlar sadece diskte olan dosyalardır. İsterseniz yeni modeliniz için taşıma işlemini okuyabilirsiniz; bu dosya **anketler/migrations/0001_initial.py** dosyasıdır. Endişelenmeyin, Django birtane oluşturdğunda bunları okumayı beklemeyin. Ancak Django'nun birşeyleri nasıl değiştirdiğini elle değiştirmek istiyorsanız yeniden düzenlenebilir olacak şekilde tasarlanmıştır.

Geçişleri sizin için gerçekleştirecek ve veritabanınızın şemasını doğal olarak yönetebilecek bir komut var. Bu komuta [migrate](/en/2.0/ref/django-admin/#django-admin-migrate) veriyoruz. Ancak önce hangi göç işleminin olacağını göreceğiz. [sqlmigrate](/en/2.0/ref/django-admin/#django-admin-sqlmigrate) komutu geçiş adlarını alır ve [sqlmigrate](/en/2.0/ref/django-admin/#django-admin-sqlmigrate) döndürür:

<pre data-gnl="1 1p"><code class="language-python">
  $ python manage.py sqlmigrate anketler 0001
</code></pre>

Aşağıdakine benzer bir şey görmelisiniz (okunabilirlik için yeniden biçimlendirdik):
<pre data-gnl="1 1p"><code class="language-python">
  BEGIN;
  --
  -- Secim modelini oluştur
  --
  CREATE TABLE "anketler_secim" (
      "id" serial NOT NULL PRIMARY KEY,
      "secim_metni" varchar(200) NOT NULL,
      "oylar" integer NOT NULL
  );
  --
  -- Soru modelini oluştur
  --
  CREATE TABLE "anketler_soru" (
      "id" serial NOT NULL PRIMARY KEY,
      "soru_metni" varchar(200) NOT NULL,
      "yayim_tarihi" timestamp with time zone NOT NULL
  );
  --
  -- Secim'e soru alanı ekle
  --
  ALTER TABLE "anketler_secim" ADD COLUMN "soru_id" integer NOT NULL;
  ALTER TABLE "anketler_secim" ALTER COLUMN "soru_id" DROP DEFAULT;
  CREATE INDEX "anketler_secim_7aa0f6ee" ON "anketler_secim" ("soru_id");
  ALTER TABLE "anketler_secim"
    ADD CONSTRAINT "anketler_secim_soru_id_246c99a640fbbd72_fk_anketler_soru_id"
      FOREIGN KEY ("soru_id")
      REFERENCES "anketler_soru" ("id")
      DEFERRABLE INITIALLY DEFERRED;

  COMMIT;
</code></pre>

Aşağıdakilere dikkat edin:

- Tablo isimleri, uygulamanın adını (anketler) ve modelin küçük harf adını - **soru** ve **secim** birleştirerek doğal olarak oluşturuluyor. (Bu davranışı geçersiz kılabilirsiniz.)
- Birincil anahtarlar (id) doğal olarak eklenir. (Bunu da geçersiz kılabilirsiniz.)
- Sözleşmeye göre, Django **_id** yabancı anahtar alan adına ekler. (Evet, bu da geçersiz kılınabilir.)
- Yabancı anahtar ilişkisi, bir **ForeignKey** kısıtlaması ile açıkça belirtilir. **DEFERRABLE** parçaları hakkında endişelenmeyin. Bu sadece PostgreSQL'e işlemin sonuna kadar yabancı anahtarı zorlamamasını söylüyor.
- Kullanmakta olduğunuz veritabanına uyacak şekilde tasarlandığından, **auto_increment** (MySQL), **serial** (PostgreSQL) veya **integer primary key autoincrement** (SQLite) gibi veritabanına özel alan türleri doğal olarak sizin için ele alınır. Aynı, alan adlarının alıntılanması için de geçerlidir. Örnek, çift tırnak işareti veya tek tırnak işareti kullanmak.
- [sqlmigrate](/en/2.0/ref/django-admin/#django-admin-sqlmigrate) komutu, veritabanınızdaki geçiş işlemini gerçekte yürütmez. Yalnızca SQL Django'nun düşündüğünü görebilmeniz için ekranda yazdırır. Django'nun ne yapacağını kontrol etmek veya değişiklikler için SQL komut dosyaları gerektiren veritabanı yöneticileri varsa yararlıdır.

İlginizi çekiyorsa [**python manage.py check**](/en/2.0/ref/django-admin/#django-admin-check) de çalıştırabilirsiniz; bu, [**göç**](/en/2.0/ref/django-admin/#django-admin-migrate) yapmadan veya veritabanına dokunmadan projenizdeki herhangi bir sorunu denetler.

Şimdi, veritabanınızda bu model tablolarını oluşturmak için [**migrate**](/en/2.0/ref/django-admin/#django-admin-migrate) komutunu tekrar çalıştırın.
<pre data-gnl="1 1p"><code class="language-python">
$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, anketler, sessions
Running migrations:
  Rendering model states... DONE
  Applying anketler.0001_initial... OK
</code></pre>

[**Göç**](/en/2.0/ref/django-admin/#django-admin-migrate) komutu, uygulanmış olan tüm göçleri alır (Django, hangilerinin veritabanında **django_migrations** olarak adlandırılan özel bir tablo kullanılarak uygulandığını izler) ve bunları veritabanınıza karşı çalıştırır; aslında, modellerinizde yaptığınız değişiklikleri şema ile uyumlu yapar.

Geçişler çok güçlüdür ve veritabanınızı veya tablolarınızı silmenize, yenilikler yapmanıza gerek kalmadan projenizi geliştirirken zamanla modellerinizi değiştirmenizi sağlar. Veri kaybetmeden canlı olarak veritabanınızı güncelleme konusunda uzmanlaşmıştır. Öğreticinin daha sonraki bölümünde ayrıntılı bir şekilde değineceğiz. Ancak şimdilik, model değişikliklerini yapmak için üç adımlı kılavuzu unutmayın:


- Kalıplarınızı değiştirin (**models.py** içinde)
- Bu değişiklikleri geçirmek için [**python manage.py makemigrations**](/en/2.0/ref/django-admin/#django-admin-makemigrations) çalıştırın.
- Bu değişiklikleri veritabanına geçirmek için [**python manage.py migrate**](/en/2.0/ref/django-admin/#django-admin-migrate) çalıştırın

Göç işlemleri yapmak ve uygulamak için ayrı komutların bulunmasının nedeni, sürüm denetimi sistemimize göç işlemlerini taahhüt etmeniz ve bunları uygulamanızla birlikte göndereceğiniz olmasıdır. Yalnızca gelişiminizi kolaylaştırmakla kalmaz aynı zamanda diğer geliştiriciler tarafından da kullanılabilirler ve üretim aşamasında olurlar.

**manage.py** yardımcı programının neler yapabileceği hakkında tam bilgi için [django-admin belgelerini](/en/2.0/ref/django-admin/) okuyun.

<hr>

## API ile oynamak {#playing-with-the-api}
Şimdi etkileşimli Python kabuğuna atlayalım ve Django'nun sunduğu ücretsiz API ile oynamaya başlayalım. Python kabuğunu çağırmak için şu komutu kullanın:

<pre data-gnl="1 1p"><code class="language-python">
  $ python manage.py shell
</code></pre>

Bunu yalnızca basitçe "python" yazarak kullanıyoruz. Çünkü **manage.py**, **DJANGO_SETTINGS_MODULE** ortam değişkenini ayarlar; Django'ya **benimsite/settings.py** dosyanıza Python içe aktarma yolu verir.

<div data-bilget="genel" markdown="1">
### Manage.py'yi atlama

manage.py kullanmayı tercih ederseniz sorun değil. Sadece DJANGO_SETTINGS_MODULE ortam değişkeni benimsite.settings, düz bir Python kabuğu başlatın ve Django'yu kurun:

<pre data-gnl="1 1p"><code class="language-python">
  &gt;&gt;&gt; import django
  &gt;&gt;&gt; django.setup()
</code></pre>

Bu bir [AttributeError](#) oluşturursa, muhtemelen Django'nun bu öğretici sürümle eşleşmeyen bir sürümünü kullanıyorsunuz demektir. Ya eski öğreticiye veya daya yeni Django sürümüne geçmek isteyeceksiniz.

Manage.py dosyasının bulunduğu dizinden python çalıştırmalısınız veya bu dizinin python yolunda olduğundan emin olun. Böylece import benimsite çalışacaktır.

Tüm bunlar hakkında daha fazla bilgi için [django-admin belgelerine](#) bakın.
</div>

Kabukta iken [veritabanı API](/en/2.0/topics/db/queries/)'sini keşfedin:

<pre data-gnl="1 1p"><code class="language-python">
  &gt;&gt;&gt; from anketler.models import Soru, Secim   # Sadece yazdığımız model sınıflarını içe aktarın.

  # Sistemde henüz hiçbir soru yok.
  &gt;&gt;&gt; Soru.objects.all()
  &lt;QuerySet []&gt;

  # Yeni bir Soru oluştur.
  # Varsayılan ayarlar dosyasında zaman dilimlerinin desteği etkin olduğundan,
  # bu nedenle Django yayim_tarihi için tzinfo ile bir datetime bekliyor.
  # timezone.now() kullanın datatime.datetime.now() yerine... Ve doğru olanı yapacağız.
  &gt;&gt;&gt; from django.utils import timezone
  &gt;&gt;&gt; q = Soru(soru_metni="Yenilikler ne?", yayim_tarihi=timezone.now())

  # Nesneyi veritabanına kaydedin. save() öğesini açıkça çağırmalısınız.
  &gt;&gt;&gt; q.save()

  # Şimdi bir ID'si var.
  &gt;&gt;&gt; q.id
  1

  # Python nitelikleri üzerinden model alan değerlerine erişir.
  &gt;&gt;&gt; q.soru_metni
  "Yenilikler ne?"
  &gt;&gt;&gt; q.yayim_tarihi
  datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=&lt;UTC&gt;)

  # Öznitelikleri değiştirip sonra save() yöntemini çağırarak değerleri değiştirin.
  &gt;&gt;&gt; q.soru_metni = "N'haber?"
  &gt;&gt;&gt; q.save()

  # objects.all() veritabanındaki tüm soruları görüntüler.
  &gt;&gt;&gt; Soru.objects.all()
  &lt;QuerySet [&lt;Soru: Soru object (1)&gt;]&gt;
</code></pre>

Bir dakika bekle. **&lt;Soru: Soru object (1)&gt;**, bu nesnenin yararlı bir gösterimi değil. **Soru** kalıbını (**anketler/models.py** dosyasında) düzenleyerek ve hem **Soru** hem de **Secim** bir [**__str__()**](/en/2.0/ref/models/instances/#django.db.models.Model.__str__) yöntemi ekleyerek düzeltelim:

anketler/models.py
<pre data-gnl="1 1p"><code class="language-python">
  from django.db import models

  class Soru(models.Model):
      # ...
      def __str__(self):
          return self.soru_metni

  class Secim(models.Model):
      # ...
      def __str__(self):
          return self.secim_metni
</code></pre>

[**__str__()**](/en/2.0/ref/models/instances/#django.db.models.Model.__str__) yöntemlerini modellerinize eklemek önemlidir; yalnızca etkileşimli istemle uğraşırken kendi kolaylığınız için değil, aynı zamanda nesnelerin temsilleri Django'nun doğal olarak oluşturulan yönetici tarafından kullanıldığı için de geçerlidir.
Bunların olağan Python yöntemleri olduğuna dikkat edin. Demo için özel bir yöntem ekleyelim:

anketler/models.py
<pre data-gnl="1 1p"><code class="language-python">
import datetime

from django.db import models
from django.utils import timezone


class Soru(models.Model):
    # ...
    def was_published_recently(self):
        return self.yayim_tarihi >= timezone.now() - datetime.timedelta(days=1)
</code></pre>

**import datetime** ekini ve **django.utils import timezone**'u, sırasıyla [**django.utils.timezone**](/en/2.0/ref/utils/#module-django.utils.timezone) içindeki Python standart [**datetime**](https://docs.python.org/3/library/datetime.html#module-datetime) modülüne ve Django saat dilimi ile ilgili yardımcı programlara başvurmaya dikkat edin. Python'da saat dilimini işleme konusunda bilginiz yoksa, [saat dilimi destek belgeleri](/en/2.0/topics/i18n/timezones/)nde daha fazla bilgi edinebilirsiniz.

Bu değişiklikleri kaydedin ve **python manage.py shell** tekrar çalıştırarak yeni bir Python etkileşimli kabuğu başlatın:

<pre data-gnl="1 1p"><code class="language-python">
  &gt;&gt;&gt; from anketler.models import Soru, Secim

  # __str__() ekimizin çalıştığından emin olun.
  &gt;&gt;&gt; Soru.objects.all()
  &lt;QuerySet [&lt;Soru: Yenilikler ne?&gt;]&gt;

  # Django, tamamen anahtar sözcükler tarafından yönlendirilen
  # zengin bir veritabanı arama API'si sağlar.
  &gt;&gt;&gt; Soru.objects.filter(id=1)
  &lt;QuerySet [&lt;Soru: Yenilikler ne?&gt;]&gt;
  &gt;&gt;&gt; Soru.objects.filter(soru_metni__startswith='Yenilikler')
  &lt;QuerySet [&lt;Soru: Yenilikler ne?&gt;]&gt;

  # Bu yıl yayınlanan soruyu cevaplayın.
  &gt;&gt;&gt; from django.utils import timezone
  &gt;&gt;&gt; simdiki_yil = timezone.now().year
  &gt;&gt;&gt; Soru.objects.get(yayim_tarihi__year=simdiki_yil)
  &lt;Soru: Yenilikler ne?&gt;

  # Var olmayan bir kimlik isteğinde bulunduğunuzda, bu bir istisna oluşturacaktır.
  &gt;&gt;&gt; Soru.objects.get(id=2)
  Traceback (most recent call last):
      ...
  DoesNotExist: Soru matching query does not exist.

  # Birincil anahtarla arama en yaygın olanıdır. bu yüzden Django, birincil anahtar tam aramaları için kısayol sağlar.
  # Aşağıdakiler Soru.objects.get(id=1) ile aynıdır.
  &gt;&gt;&gt; Soru.objects.get(pk=1)
  &lt;Soru: Yenilikler ne?&gt;

  # Özel yöntemimizin çalıştığından emin olun.
  &gt;&gt;&gt; q = Soru.objects.get(pk=1)
  &gt;&gt;&gt; q.was_published_recently()
  True

  # Soru için birkaç seçenek verin. Create çağrısı yeni bir
  # Secim nesnesi oluşturur, INSERT deyimi yapar, seiçimi kullanılabilir seçenekler
  # grubuna ekler ve yeni Secim nesnesini döndürür.
  &gt;&gt;&gt; q = Soru.objects.get(pk=1)

  # İlgili nesne setinden herhangi bir seçeneği görüntüler. Şimdiye kadar hiçbiri görüntüleniyor.
  &gt;&gt;&gt; q.secim_set.all()
  &lt;QuerySet []&gt;

  # Üç seçenek oluşturun.
  &gt;&gt;&gt; q.secim_set.create(secim_metni='Fazla değil', oylar=0)
  &lt;Secim: Not much&gt;
  &gt;&gt;&gt; q.secim_set.create(secim_metni='Gökyüzü', oylar=0)
  &lt;Secim: The sky&gt;
  &gt;&gt;&gt; c = q.secim_set.create(secim_metni='Sadece tekrar hackleniyor', oylar=0)

  # Seçim nesnelerinin, ilgili Soru nesnelerine API erişimi vardır.
  &gt;&gt;&gt; c.soru
  &lt;Question: Yenilikler ne?&gt;

  # Ve tersi de mümkün: Soru nesneleri Secim nesnelerine erişebilir.
  &gt;&gt;&gt; q.secim_set.all()
  &lt;QuerySet [&lt;Secim: Fazla değil&gt;, &lt;Secim: Gökyüzü&gt;, &lt;Secim: Sadece tekrar hackleniyor&gt;]&gt;
  &gt;&gt;&gt; q.secim_set.count()
  3

  # API, ilişkileri ihtiyacınız olan yere kadar doğal olarak izler. İlişkileri ayırmak için çift altçizgi kullanın.
  # Bu istediğiniz kadar çok derinlikli olarak çalışır. Yani sınır yok. Bu sene yayim_tarihi olan herhangi bir sorunun
  # seçimini yapın (yukarıda oluşturduğumuz 'simdiki_yil' değişkenini tekrar kullanın.)
  &gt;&gt;&gt; Secim.objects.filter(question__pub_date__year=current_year)
  &lt;QuerySet [&lt;Secim: Fazla değil&gt;, &lt;Secim: Gökyüzü&gt;, &lt;Secim: Sadece tekrar hackleniyor&gt;]&gt;

  # Seçimlerden birini silelim. Bunun için delete() kullanın.
  &gt;&gt;&gt; c = q.secim_set.filter(secim_metni__startswith='Sadece tekrar')
  &gt;&gt;&gt; c.delete()
</code></pre>

Kalıp ilişkileri hakkında daha fazla bilgi için bkz. [İlgili nesnelere erişme](/en/2.0/ref/models/relations/). API aracılığıyla alan araştırmaları yapmak için çift altçizgi kullanma konusunda daha fazla bilgi için bkz: [Alan arama](/en/2.0/topics/db/queries/#field-lookups-intro). Veritabanı API'si ile ilgili tüm ayrıntılar için [Veritabanı API'si başvuru](/en/2.0/topics/db/queries/)muza bakın.

<hr>

## Django Yönetici Tanıtımı {#introducing-the-django-admin}
<div data-bilget="genel">
### Felsefe

Personeliniz veya müşterileriniz için içerik eklemek, değiştirmek ve silmek için yönetici siteleri oluşturmak çok yaratıcılık gerektirmeyen sıkıcı bir iştir. Bu nedenle, Django tamamen modeller için yönetici arayüzlerinin oluşturulmasını doğallaştırır.

Django, "içerik yayıncıları" ile "kamu" sitesi arasında çok net bir ayrımla, bir habercilik odası ortamında yazılmıştır. Site yöneticileri, haber öyküleri, etkinlikler, spor skorları vb. eklemek için örgüyü kullanır ve bu içerik genel sitede görüntülenir. Django, site yöneticilerinin içeriği düzenlemeleri için birleşik bir arabirim oluşturma sorunu çözer.

Yönetici, site ziyaretçileri tarafından kullanılacak şekilde tasarlanmamıştır. Site yöneticileri içindir.
</div>

### Yönetici kullanıcısı oluşturma {#creating-an-admin-user}

Önce, yönetici siteye giriş yapabilecek bir kullanıcı oluşturmamız gerekecek. Aşağıdaki komutu çalıştırın:

<pre data-gnl="1 1p"><code class="language-python">
  $ python manage.py createsuperuser
</code></pre>

İstediğiniz kullanıcı adını girin ve enter tuşuna basın:

<pre data-gnl="1 1p"><code class="language-python">
  Username: admin
</code></pre>

Ardından istediğiniz e-posta adresini girmeniz istenir:

<pre data-gnl="1 1p"><code class="language-python">
  Email address: admin@example.com
</code></pre>

Son adım şifrenizi girmektir. İki kez şifrenizi girmeniz istenecek ve ikinci seferinde ilk şifrenizi onaylayacaksınız.

<pre data-gnl="1 1p"><code class="language-python">
  Password: **********
  Password (again): *********
  Superuser created successfully.
</code></pre>

<hr>

### Geliştirme sunucunusu başlatmak (yerel sunucu) {#start-the-development-server}

Django admin sitesi varsayılan olarak etkinleştirilir. Gelişim sunucusuna başlayalım ve keşfedelim.
Sunucu çalışmıyorsa, şu şekilde başlatın:

<pre data-gnl="1 1p"><code class="language-python">
  $ python manage.py runserver
</code></pre>

Şimdi, bir internet ağı tarayıcısı açın ve yerel etki alanınızdaki "/admin/" adresine gidin. Örneğin http://127.0.0.1:8000/admin/
Yöneticinin giriş ekranı karşınıza gelecektir.

<img src="https://docs.djangoproject.com/en/2.0/_images/admin01.png" alt="">

[Çeviri](/en/2.0/topics/i18n/translation/) varsayılan olarak açık olduğundan, giriş ekranı, tarayıcınızın ayarlarına ve Django'nun bu dil için bir çeviri yapmasına bağlı olarak kendi dilinizde görüntülenebilir.

<hr>

## Admin sitesini giriniz {#enter-the-admin-site}

Şimdi, önceki adımda oluşturduğunuz süper kullanıcı hesabı ile giriş yapmayı deneyin. Django admin dizin sayfasını görmelisiniz:

<img src="https://docs.djangoproject.com/en/2.0/_images/admin02.png" alt="">

Düzenlenebilir içerik türlerinden bazılarını görmelisiniz: gruplar ve kullanıcılar. Django tarafından gönderilen kimlik doğrulama çerçevesi olan [**django.contrib.auth**](/en/2.0/topics/auth/#module-django.contrib.auth) tarafından sağlanmaktadır.

<hr>

## Anket uygulamasını admin'de değiştirilebilir yapın {#make-the-poll-app-modifiable-in-the-admin}

Ama bizim anket uygulaması nerede? Yönetici dizini sayfasında görüntülenmez.
Yapmanız gereken tek şey: Yöneticiye, **Soru** nesnelerinin yönetici arayüzüne sahip oldğunu söylememiz gerekiyor. Bunu yapmak için **anketler/admin.py** dosyasını açın ve şu şekilde düzenleyin:

anketler/admin.py
<pre data-gnl="1 1p"><code class="language-python">
  from django.contrib import admin

  from .models import Soru

  admin.site.register(Soru)
</code></pre>

<hr>

## Ücretsiz admin işlevini keşfedin {#explore-the-free-admin-functionality}

Artık **Soru** kısmını kaydettik, Django bunun admin dizini sayfasında gösterileceğini biliyor:

<img src="https://docs.djangoproject.com/en/2.0/_images/admin03t.png" alt="">

"Sorular"'ı tıklayın Artık sorularınız için "değişim listesi" sayfasındasınız. Bu sayfada veritabanındaki tüm sorular görüntülenir ve bunları değiştirmek için birini seçmenizi sağlar. Daha önce yarattığımız "Yenilikler ne?" sorusu var:

<img src="https://docs.djangoproject.com/en/2.0/_images/admin04t.png" alt="">

Düzenlemek için "Yenilikler ne?" sorusuna tıklayın:

<img src="https://docs.djangoproject.com/en/2.0/_images/admin05t.png" alt="">

Burada dikkat edilmesi gereken hususlar:

- Form: **Soru** kalıbından doğal olarak oluşturulur.
- Farklı model alan türleri ([**DateTimeField**](/en/2.0/ref/models/fields/#django.db.models.DateTimeField), [**CharField**](/en/2.0/ref/models/fields/#django.db.models.CharField)) uygun HTML girişi <a href="#">CharField</a> karşılık gelir. Her alan türü kendisini Django admininde nasıl görüntüleyeceğini bilir.
- Her [**DateTimeField**](/en/2.0/ref/models/fields/#django.db.models.DateTimeField) ücretsiz JavaScript kısayollarını alır. Tarihler bir "Bugün" kısayolu ve takvim açılır penceresi alıyor ve zamanlarda "Şimdi" kısayolu ve sık girilen saatleri listeleyen uygun bir açılır pencere elde ediliyor.


Sayfanın alt kısmı size birkaç seçenek sunar:

- Kaydet: Değişiklikleri kaydeder ve bu tür nesne için değişim listesi sayfasına geri döner.
- Kaydet ve devam et: Değişiklikleri kaydeder ve bu nesne için yönetici sayfasını yeniden yükler.
- Kaydet ve başka ekle: Değişiklikleri kaydeder ve bu nesne türü için yeni, boş bir form yükler.
- Sil: Silme onayı sayfası görüntüler.

"Yayınlanan tarih" in değeri [Öğretici 1](/en/2.0/intro/tutorial01/)'de soru oluşturduğunuz saatle eşleşmiyorsa, muhtemelen [**TIME_ZONE**](/en/2.0/ref/settings/#std:setting-TIME_ZONE) ayarı için doğru değeri ayarlamayı unuttuğunuz anlamına gelir. Değiştirin, sayfayı yeniden yükleyin ve doğru değerin göründüğünden emin olun.

"Bugün" ve "Şimdi" kısayollarını tıklatarak "Yayınlanan tarihi" değiştirin. Daha sonra "Kaydet ve devam et"i tıklayın. Ardından sağ üst köşedeki "Geçmiş"i tıklayın. Bu nesne yapılan değişikliklerin tümünü Django yöneticisi aracılığıyla, değişikliği yapan kişinin zaman damgası ve kullanıcı adı ile birlikte listelendiği bir sayfa görürsünüz:

<img src="https://docs.djangoproject.com/en/2.0/_images/admin06t.png" alt="">

Modeller API'sinden memnun kaldıysanız ve kendinizi yönetici sitesiyle tanıdıysanız, anket uygulamalarımıza daha fazla görünüm ekleme hakkında bilgi edinmek için [Öğretici 3](/en/2.0/intro/tutorial03/) bölümünü okuyun.

[**İlk django uygulamanızı yazma, bölüm 1**](/en/2.0/intro/tutorial01/) | [**İlk django uygulamanızı yazma, bölüm 3**](/en/2.0/intro/tutorial03/)
{: .sayfalandırma}
