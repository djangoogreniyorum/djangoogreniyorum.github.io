---
layout: general
title: Django için ilk yamanızı yazma - Django Öğreniyorum
---
# Django için ilk yamanızı yazma

## Tanıtım {#introduction}

Topluluğu biraz boşluyor musunuz? Belki Django'da çözülmüş olarak görmek istediğiniz bir hata buldunuz ya da eklenmesini istediğiniz küçük bir özellik var.

Django'ya bildirim vermek, endişelerinizin çözümlenmesi için en iyi yoldur. Bu başlangıçta caydırıcı görünebilir. Ama gerçekten çok basittir. Bütün süreç boyunca bunu size göstereceğiz. Böylelikle örnek yardımıyla öğrenebilirsiniz.

### Bu öğretici kimin için? {#who-s-this-tutorial-for}

<div data-bilget="genel" markdown="1">
### Bakınız

Yamaları nasıl göndereceğinizle ilgili bir başvuru arıyorsanız [Yamalar Ekleme](/en/2.0/internals/contributing/writing-code/submitting-patches/) belgelerine bakın.
</div>

Bu öğreticide Django'nun nasıl çalıştığına dair en azından temel bir anlayışa sahip olmanızı bekliyoruz. Bu, [ilk Django uygulamanız](/en/2.0/intro/tutorial01/)ı yazarken mebvcut öğreticilere rahatlıkla bakmanız gerektiği anlamına gelir. Buna ek olarak, Python'u da iyi anlamış olmalısınız. Ancak yapmazsanız, [Dive Into Python](http://www.diveintopython3.net/) Python programcılarının başlamasıyla ilgili şahane (ve ücretsiz) bir çevrimiçi kitaptır.

Sürüm kontrol örgüsüne ve Trac'e aşina olmayanlarınız, bu öğreticinin ve bağlantılarının başlamak için yeterli bilgiyi içerdiğini göreceksiniz. Bununla birlikte, Django'ya düzenli olarak katkıda bulunmayı planlıyorsanız, muhtemelen bu farklı araçlar hakkında bira daha okumak isteyeceksinizdir.

Çoğunlukla, bu öğretici açıklayıcı olmak ister. Böylece en geniş kitleye yararlı olabilir.


<div data-bilget="genel" markdown="1">
### Nereden yardım alınabilir:

Eğer bu öğreticide sorun yaşıyorsanız, lütfen django geliştiricilerine bir ileti gönderin veya yardımcı olabilecek diğer Django kullanıcılarıyla sohbet etmek için irc.freenode.net üzerinde #django-dev tarafına bağlanın.
</div>

<hr>

## Bu öğreticinin içeriği nedir? {#what-does-this-tutorial-cover}

İlk kez Django'ya bir yama ekleyerek size yöneleceğiz. Bu yazının sonunda, hem araçlar hem de süreçler konusunda temel bir anlayışa sahip olmanız gerekir. Özellikle, aşağıdakiler konusunda.

- Git kurulumu
- Django'nun bir geliştirme kopyasını edinme
- Djangonun sınama paketini çalıştırma
- Yama için bir sınama yazma
- Yama kodunu yazma
- Yamayı sınama
- Çekme isteği gönderme
- Daha fazla bilgi için nerelere bakılacağı

Öğreticiyi bitirdikten sonra, [katkıda bulunmak için kalan Django belgeleri](/en/2.0/internals/contributing/)ne bakabilirsiniz. Çok sayıda harika bilgi içerir ve Django'ya düzenli katkıda bulunmak isteyen herkesin okuması zorunludur. Eğer sorularınız varsa, muhtemelen cevaplanır.

<div data-bilget="genel" markdown="1">
### Python 3 gerekli!

Django'nun geçerli sürümü (2.0&#43;) Python 2.7'yi desteklemez. Python 3'ü [Python'nun indirme sayfası](https://www.python.org/downloads/)ndan veya işletim örgünüzün paket yöneticisinden edinin.
</div>

<div data-bilget="genel" markdown="1">
### Windows kullanıcıları için

Python'u Windows'a yüklerken, komut satırında her zaman bulunması için "Add python.exe to Path" seçeneğini işaretlediğinizden emin olun.
</div>

<hr>

## Davranış kodu {#code-of-conduct}

Katkıda bulunan olarak, Django topluluğunu açık ve kapsamlı tutmamıza yardımcı olabilirsiniz. Lütfen [Davranış Kuralları](/conduct/) konusunu okuyun ve uyun.

<hr>

## Git kurulumu {#installing-git}

Bu ders için, Django'nun mevcut geliştirme sürümünü indirmek ve yaptığınız değişiklikler için düzeltme eki dosyaları oluşturmak için Git'in yüklü olması gerekmektedir.

Git'in yüklü olup olmadığını denetlemek için **git** komut satırına girin. Bu komutu bulamadığını söyleyen mesajlar alırsanız, onu indirip yüklemeniz gerekir. Bkz. [Git indirme sayfası](https://git-scm.com/download)

<div data-bilget="genel" markdown="1">
### Windows kullanıcıları için

Windows'da Git'i kurarken, Git'in kendi kabuğunda çalışmasıiçin "Git Bash" seçeneğini seçmeniz önerilir. Bu öğretici kurulumunuzu böyle varsayar.
</div>

Git'e aşina değilseniz, komut satırına **git help** yazarak komutlarını (kurulduktan sonra) daha fazla öğrenebilirsiniz.

## Django geliştirme sürümünün kopyasını edinme {#getting-a-copy-of-django-s-development-version}

Django'ya katkıda bulunmanın ilk adımı, kaynak kodun bir kopyasını edinmektir. İlk olarak, [Github'da Django çatallayın](https://github.com/django/django/fork). Ardından, komut satırından, Django'nun yerel kopyasını bulundurmak istediğiniz dizine gitmek için **cd** komutunu kullanın.

Aşağıdaki komutu kullanarak Django kaynak kodu havuzunu indirin:

  <pre data-gnl="1 1p"><code class="language-python">
  $ git clone git@github.com:YourGitHubName/django.git
  </code></pre>

Django'nun yerel bir kopyasına sahip olduğunuza göre, **pip**'i kullanarak herhangi bir paketi indirdiğiniz gibi kurabilirsiniz. Bunu yapmanın en kolay yolu, projelerinizin her birine birbirine karışmaması için yüklü paketlerin ayrı bir dizinini saklamanıza izin veren Python'da yerleşik bir özellik olan sanal ortam (veya virtualenv) kullanmaktır.

Tüm sanal ortam dosyalarınızı tek bir yerde, örneğin ev dizininizdeki **.virtualenvs/** ortamında saklamak iyi bir fikirdir. Henüz yoksa onu oluşturun:

  <pre data-gnl="1 1p"><code class="language-python">
  $ mkdir ~/.virtualenvs
  </code></pre>

Şimdi şunu çalıştırarak yeni bir virtualenv oluşturun:

  <pre data-gnl="1 1p"><code class="language-python">
  $ python3 -m venv ~/.virtualenvs/djangodev
  </code></pre>

Yol, bilgisayarınıza yeni ortamın kaydedileceği yerdir.

<div data-bilget="genel" markdown="1">
### Windows kullanıcıları için

Etkin komut dosyaları yalnızca örgü kabuğu (**.bat**) ve PowerShell (**.ps1**) için oluşturulduğundan, Windows'da Git Bash kabuğunu kullanıyorsanız dahili venv modülünün kullanılması da çalışmaz. Bunun yerine **virtualenv** paketini kullanın.

  <pre data-gnl="1 1p"><code class="language-python">
  $ pip install virtualenv
  $ virtualenv ~/.virtualenvs/djangodev
  </code></pre>
</div>

<div data-bilget="genel" markdown="1">
### Ubuntu kullanıcıları için

Ubuntu'nun bazı sürümlerinde yukarıdaki komut başarısız olabilir. Bunun yerine öncelikle **pip3** var olduğundan emin olarak **virtualenv** paketini kullanın:

  <pre data-gnl="1 1p"><code class="language-python">
  $ sudo apt-get install python3-pip
  $ # Prefix the next command with sudo if it gives a permission denied error
  $ pip3 install virtualenv
  $ virtualenv --python=`which python3` ~/.virtualenvs/djangodev
  </code></pre>
</div>

virtualenv kurmanın son adımı etkinleştirmektir:

  <pre data-gnl="1 1p"><code class="language-python">
  $ source ~/.virtualenvs/djangodev/bin/activate
  </code></pre>

eğer **kaynak** kodu yoksa bunun yerine bir nokta kullanmayı deneyebilirsiniz:

  <pre data-gnl="1 1p"><code class="language-python">
  $ . ~/.virtualenvs/djangodev/bin/activate
  </code></pre>

<div data-bilget="genel" markdown="1">
### Windows kullanıcıları için

Sanal ortamı Windows'ta etkinleştirmek için şunu çalıştırın:

  <pre data-gnl="1 1p"><code class="language-python">
  $ source ~/virtualenvs/djangodev/Scripts/activate
  </code></pre>
</div>

Yeni bir kabuk penceresi açtığınızda **virtualenv**'yi etkinleştirmeniz gerekir. **virtualenvwrapper** bunu daha kullanışlı hale getirmek için kullanışlı bir araçtır.

Bundan sonra **pip** aracılığıyla yüklediğiniz her şey, diğer ortamlardan ve örgü çapında paketlerden yalıtılarak yeni sanal makinenize kurulacaktır. Ayrıca, şu anda etkinleşen virtualenv adı, hangisini kullandığınızın iznini tutmanıza yardımcı olmak için komut satırında görüntülenir. Devam edin ve daha önce kopyalanmış Django kopyasını yükleyin:

  <pre data-gnl="1 1p"><code class="language-python">
  $ pip install -e /path/to/your/local/clone/django/
  </code></pre>

Django'nun kurulu sürümü şimdi yerel kpyanıza işaret ediyor. Yaptığınız değişiklikleri hemen göreceksiniz; ilk yamanızı yazarken çok yardımcı oluyoruz.

<hr>

## Django'nun önceki derlemesine geri dönüş {#rolling-back-to-a-previous-revision-of-django}

Bu yazıda, bilet #[24788](https://code.djangoproject.com/ticket/24788)'i bir vaka çalışması olarak kullanacağız, bu nedenle Django'nun sürüm geçmişi, biletin yamasının uygulanmasından önce gidilecek yere kadar geriye saracağız. Bu, Django'nun sınama paketini çalıştırmak da dahil olmak üzere, bu yamanın sıfırdan yazılmasına ilişkin tüm adımları atmamızı sağlayacaktır.

**Aşağıdaki öğreticilerin amaçları doğrultusunda eski bir Django derlemesi kullanacağımız halde, kendi yamanızda bir bilet için çalışırken Django'nun mevcut derlemesini kullanmalısınız!**

<div data-bilget="genel" markdown="1">
### Not

Bu biletin düzeltme eki Pavel Marçevski tarafından yazılmış ve Django'ya [işlenen 4df7e8483b2679fc1cba3410f08960bac6f51115](https://github.com/django/django/commit/4df7e8483b2679fc1cba3410f08960bac6f51115) olarak uygulanmıştır. Sonuç olarak, Django'nun derlenmesini bunun hemen öncesinde kullanacağız, [işlenen 4ccfc4439a7add24f8db4ef3960d02ef8ae09887](https://github.com/django/django/commit/4ccfc4439a7add24f8db4ef3960d02ef8ae09887) yapacağız.
</div>

Django'nun kök dizinine gidin (bu, django belgeleri, sınamalar, yazarlar vb. içeren dizin). Sonra, aşağıdaki öğreticide kullanacağımız eski Django derlemesini sağlama yapabilirsiniz:

  <pre data-gnl="1 1p"><code class="language-python">
  $ git checkout 4ccfc4439a7add24f8db4ef3960d02ef8ae09887
  </code></pre>

<hr>

## İlk kez Django'nun sınama paketini çalıştırmak {#running-django-s-test-suite-for-the-first-time}

Django'ya katkıda bulunurken, kod değişikliklerinizin hataları Django'nun diğer alanlarına dahil etmemesi çok önemlidir. Değişikliklerinizi yaptıktan sonra Django'nun hala çalışıp çalışmadığının sağlamasını yapmanın bir yolu Django'nun sınama paketini çalıştırmaktır. Tüm sınamalar geçerse, değişikliklerin Django'yu tamamen kırmadığından emin olabilirsiniz. Daha önce hiç Django'nun sınama paketini çalıştırmadıysanız, çıktısının nasıl görüneceğini öğrenmek için yalnızca bir kere çalıştırmanız iyi bir fikirdir.

Sınama paketini çalıştırmadan önce Django **tests/** dizinindeki ilk **cd**-ing bağlılıklarını yükleyin ve sonra çalıştırın:

  <pre data-gnl="1 1p"><code class="language-python">
  $ pip install -r requirements/py3.txt
  </code></pre>

Kurulum sırasında bir hata ile karşılaşırsanız, örgünüz Python paketlerinden birine veya daha fazlasına bağımlılık eksik çıkabilir. Karşılaştığınız hata iletisiyse başarısız olan paketin belgelerine bakın veya ağda arama yapın.

Şimdi sınama paketini çalıştırmaya hazırız. GNU/Linux, macOS veya Unix'in bir başka lezzetini kullanıyorsanız şunu çalıştırın:

  <pre data-gnl="1 1p"><code class="language-python">
  $ ./runtests.py
  </code></pre>

Şimdi arkanıza yaslanın ve rahatlayın. Django'nun tüm sınama paketi 9600'den fazla farklı sınamaya sahiptir, bu nedenle bilgisayarınızın hızına bağlı olarak 5 ila 15 dakika arasında bir sürede çalışabilir.

Django'nun sınama paketi çalışıyorken, her sınamanın durumunu göstereren bir karakter akışı görürsünüz. **E**, bir sınama sırasında bir hata oluştuğunu ve **F**, bir sanımanın onaylamasının başarısız olduğunu gösterir. Bunların her ikisi de sınamaları arızalı olarak kabul edilir. Bu arada, **x** ve **s** beklenen arızaları ve atlanan sınamaları belirtir. Nokta, geçen sınamaları gösterir.

Atlanan sınamalar genellikle sınamayı çalıştırmak için gereken eksik harici kütüphanelerden kaynaklanır. Bknz. Yaptığınız değişikliklerle ilgili sınamalar için [tüm sınamaları](/en/2.0/internals/contributing/writing-code/unit-tests/#running-unit-tests-dependencies) bağımlılıkların bir listesi için çalıştırın ve bunlardan herhangi birini yüklediğinizden emin olun (bu öğretici için herhangi bir şey gerekmez). Bazı sınamalar belirli bir veritabanı arka ucuna özgüdür ve bu arka uçla sınama edilmezse atlanır. SQLite, varsayılan ayarlar için veritabanı arka ucudur. Sınamaları farklı bir arka plan kullanarak çalıştırmak için başka bir [ayar modülünü kullanma konusu](https://docs.djangoproject.com/en/2.0/internals/contributing/writing-code/unit-tests/#running-unit-tests-settings)na bakın.

Sınamalar tamamlandıktan sonra, sınama paketinin geçip geçmediğini veya başarısız olup olmadığını bildiren bir mesajla karşılanmanız gerekir. Django2nun kodunda herhangi bir değişiklik yapmadığınız için tüm sınama paketi **geçmelidir**. Başarısızlık veya hata alırsanız, önceki adımların tümünü doğru bir şekilde uyguladığınızdan emin olun. Daha fazla bilgi için [aşama sınamalarını çalıştırma](https://docs.djangoproject.com/en/2.0/internals/contributing/writing-code/unit-tests/#running-unit-tests-settings) bölümüne bakın. Python 3.5&#43; kullanıyorsanız, gözardı edilebilecek kullanımdan kaldırılma uyarılarıyla ilgili birkaç başarısızlık olacaktır. Bu hatalar Django'da düzeltildi.

En son Django gövdesinin her zaman kararlı olmayabileceğini unutmayın. Hataya karşı geliştirirken hataların makinenize özgü olup olmadığını veya [Django'nun resmi yapıları](https://djangoci.com/)nda olup olmadığını belirlemek için Django'nun sürekli gömülü yapılarını kontrol edebilirsiniz. Belli bir kurumu görüntülemek için tıklarsanız, başarısızlıkları Python sürümü ve veritabanı arka uçlarıyla ayrılmış olarak gösteren "Yapılandırma Matrisi"ni görüntüleyebilirsiniz.

<div data-bilget="genel" markdown="1">
### Not

Bu öğretici ve üzerinde çalıştığımız bilet için SQLite'a karşı sınama yapmak yeterlidir, ancak sınamaları [farklı bir veritabanı kullanarak yürütmek](https://docs.djangoproject.com/en/2.0/internals/contributing/writing-code/unit-tests/#running-unit-tests-settings) de mümkündür (bazen gereklidir).
</div>

<hr>

## Yamanız için bir dal oluşturmak {#creating-a-branch-for-your-patch}

Herhangi bir değişiklik yapılmadan önce, bilet için yeni bir dal oluşurun.

  <pre data-gnl="1 1p"><code class="language-python">
  $ git checkout -b ticket_24788
  </code></pre>

İstediğiniz dal için herhangi bir isim seçebilirsiniz. "ticket_24788" sadece bir örnek. Bu dal içinde yapılan tüm değişiklikler bilete özgü olacak ve daha önce elde etmiş olduğumuz kodun ana kopyasını etkilemeyecektir.

<hr>

## Biletiniz için bazı sınamalar yazmak

Çoğu durumda, bir yamanın Django'ya kabul edilmesi için sınamaları içermesi gerekir. Hata düzeltme yamaları için bu, böceklenmenin asla Django'ya yeniden gönderilmemesi için bir gerileme sınaması yazılması anlamına gelir. Bir gerileme sınaması böcek halen devam ederken başarısız olacak ve böcek giderildikten sonra geçecek şekilde yazılmalıdır. Yeni özellikler içeren yamalar için yeni özelliklerin doğru şekilde çalıştığından emin olmak için sınamalar eklemeniz gerekir. Yeni özellik yoksa, bunlar da başarısız olmalı ve uygulandıktan sonra geçmelidir.

Bunu yapmanın iyi bir yolu kodda herhangi bir değişiklik yapmadan önce yeni sınamalarınızı yazmaktır. Bu geliştirme tarzına, [sınama odaklı geliştirme](https://en.wikipedia.org/wiki/Test-driven_development) denir ve hem tüm projelere hem de tek yamalara uygulanabilir. Sınamalarınızı yazdıktan sonra, gerçekten başarısız olduklarından emin olmak için (bunları düzeltmediyseniz veya o özelliği henüz eklemediyseniz) çalıştırdınız. Yeni sınamalarınız başarısız olursa, onları düzeltmelisiniz. Sonuçta, hatanın olup olmadığına bakılmaksızın geçen bir gerileme sınaması, o hatanın yolun tekrar tekrar oluşmasını önlemede çok yararlı değildir.

Şimdi elimizdeki örnek için.

### Bilet # 24788 için bazı sınamalar yazmak {#writing-some-tests-for-ticket-24788}

Bilet [# 24788](https://code.djangoproject.com/ticket/24788), küçük bir özellik ekini önermektedir: Biçim sınıflarında sınıf düzeyi nitelik **öneki** belirtebilme özelliği:

  <pre data-gnl="1 1p"><code class="language-python">
  [...] Uygulamalarla birlikte gelen biçimler etkili bir şekilde doğru biçime kararlı ve aynı anda N örgüşen form alanlarının gönderilebileceği (POST) kendi adlarını verebilir.
  </code></pre>

Bu bileti çözmek için **BaseForm** sınıfına bir **önek** özniteliği ekleyeceğiz. Bu sınıfın örneklerini oluştururken **__init__()** yöntemine bir önek geçirirseniz, bu önek oluşturulan örneğinde yine de ayarlanır. Ancak bir önek geçirmemek (veya **None** yani boş geçmek), sınıf düzeyinde önek kullanacaktır. Bu değişiklikleri yapmadan önce değişikliğin doğru şekilde çalıştığını ve gelecekte doğru şekilde çalışmaya devam edebileceğini doğrulamak için birkaç sınama yazacağız.

Django'nun **tests/foms_tests/tests/** dizinine gidin ve **test_forms.py** dosyasını açın. 1674 satırına şu kodu **test_forms_with_null_boolean** işlevinden hemen önce ekleyin:

  <pre data-gnl="1 1p"><code class="language-python">
  def test_class_prefix(self):
      # Prefix can be also specified at the class level.
      class Person(Form):
          first_name = CharField()
          prefix = 'foo'

      p = Person()
      self.assertEqual(p.prefix, 'foo')

      p = Person(prefix='bar')
      self.assertEqual(p.prefix, 'bar')
  </code></pre>

Bu yeni sınama, bir sınıf seviyesi öneki ayarının beklendiği gibi çalıştığını ve bir örneğini oluştururken bir önek değiştirgesini geçirmenin hala işe yaradığını denetler.

<div data-bilget="genel" markdown="1">
### Ancak bu sınama işlemi biraz zor görünüyor...

Daha önce sınamalarla uğraşmak zorunda kalmadıysanız, ilk bakışta yazmak biraz zor görünebilir. Neyse ki, sınama bilgisayar programlamasında çok büyük bir konudur, bu yüzden pek çok bilgi var:

- Django için sınama yazmaya iyi bir ilk bakışı [yazmak ve sınamaları çalıştırmak belgeleri](/en/2.0/topics/testing/overview/)nde bulabilirsiniz.

- Dive Into Python (Çömez Python geliştiricileri için ücretsiz bir çevrimiçi kitap.) [birim sınamanın girişi](http://www.diveintopython3.net/unit-testing.html) konusunu içerir.

- Bunları okuduktan sonra, dişlerinizi batırmak için biraz daha etli bir şeyler istiyorsanız, daima Python [**birim sınama**](https://docs.python.org/3/library/unittest.html#module-unittest) belgelendirmeleri bulunur.
</div>

<hr>

### Yeni sınamanızı yürütmek {#running-your-new-test}

Unutmayın, BaseForm'da herhangi bir değişiklik yapmamışsa, sınamalarımız başarısız olacak. Bunun gerçekten ne olduğundan emin olmak için forms_tests dizinindeki tüm sınamaları çalıştıralım. Komut satırından, cd ile Django tests/ dizinine girin ve şunu çalıştırın:

  <pre data-gnl="1 1p"><code class="language-python">
  $ ./runtests.py forms_tests
  </code></pre>

Sınamalar doğru şekilde yürüdüyse, eklediğimiz sınama yöntemine karşılık gelen bir arıza görmelisiniz. Tüm arızalar başarıyla tamamlandıysa, yukarıda gösterilen sınamayı uygun dizine ve sınıfa eklediğinizden emin olmanız gerekir.

<hr>

## Biletiniz için kod yazmak

Bilet [24788](https://code.djangoproject.com/ticket/24788)'de açıklanan işlevleri ardından Django'ya ekleyeceğiz.

### Bilet # 24788 için kod yazmak {#writing-the-code-for-ticket-24788}

**django/django/forms/** dizinine gidin ve **forms.py** dosyasını açın. 72. satırda **BaseForm** sınıfını bulun ve **field_order** özniteliğinin hemen sağına **önek** sınıf özniteliğini ekleyin:

  <pre data-gnl="1 1p"><code class="language-python">
  class BaseForm:
    # Bu, tüm biçim mantığının temel uygulamasdıdır. Bu sınıfın biçimden
    # farklı olduğunu unutmayın. Daha fazla bilgi için biçim sınıfının
    # açıklamasına bakın. Biçim API'sinde yapılan herhangi bir iyileştirme,
    # *this* sınıfına yapılmalıdır, biçim sınıfına yapılmamalıdır.
    field_order = None
    prefix = None
  </code></pre>

<hr>

### Sınamanızın şimdi geçtiğini doğrulamak {#verifying-your-test-now-passes}

Django'yu değiştirdikten sonra, daha önce yazdığımız sınamaların başarılı olduğundan emin olmalıyız, böylece yukarıda yazdığımız kodun doğru çalışıp çalışmadığını görebiliriz. Sınamaları **forms_tests** dizininde çalıştırmak için, Django tests/ dizinine girip şunu çalıştırın:

  <pre data-gnl="1 1p"><code class="language-python">
  $ ./runtests.py forms_tests
  </code></pre>

Uff, bu sınamaları yazdığımı iyi oldu! Aşağıdaki istisna dışında hala bir başarısızlık görmeniz gerekir:

  <pre data-gnl="1 1p"><code class="language-python">
  AssertionError: None != 'foo'
  </code></pre>

Koşullu ifadeyi **__init__** yöntemine eklemeyi unuttuk. Devam edin ve **self.prefix = prefix**, **django/forms/forms.py** dosyasının 87. satırında bir koşullu ifade ekleyerek değiştirin:

  <pre data-gnl="1 1p"><code class="language-python">
  if prefix is not None:
    self.prefix = prefix
  </code></pre>

Sınamaları tekrar çalıştırın ve her şey artık geçmeli. Başaramazsa, **BaseForm** sınıfını yukarıda gösterildiği gibi doğru şekilde değiştirdiğinizden ve yeni sınamayı doğru bir şekilde kopyaladığınızdan emin olun.

<hr>

## Django'nun sınama paketini ikinci kez çalıştırmak {#running-django-s-test-suite-for-the-second-time}

Paketinizin ve sınamanızın doğru çalıştığını doğruladıktan sonra, Django sınama paketinin tamamını çalıştırarak, yalnızca değişikliğinizin Django'nun diğer alanlarına herhangi bir hata getirmediğini doğrulamak iyi bir fikirdir. Tüm sınama paketini başarıyla geçtiğiniz halde kodunuzun hatasız olduğunu garanti etmez, aksi halde fark edilmeyebilecek birçok hatayı ve gerilemeyi belirlemenize yardımcı olur.

Tüm Django sınama paketini çalıştırmak için, Django **tests/** dizinine girip şunu çalıştırın:

  <pre data-gnl="1 1p"><code class="language-python">
  $ ./runtests.py
  </code></pre>

Başarısızlığı görmediğin sürece ilerlemek güzel.

<hr>

## Belgelendirmeyi Yazmak {#writing-documentation}

Bu yeni bir özelliktir, bu nedenle belgelenmelidir. Django **/docs/ref/forms/api.txt** dosyasının 1068 satırına (dosyanın sonundaki) aşağıdaki bölümü ekleyin.

  <pre data-gnl="1 1p"><code class="language-python">
The prefix can also be specified on the form class::

    &gt;&gt;&gt; class PersonForm(forms.Form):
    ...     ...
    ...     prefix = 'person'

.. versionadded:: 1.9

    The ability to specify ``prefix`` on the form class was added.
  </code></pre>

Bu yeni özellik, yaklaşmakta olan bir sürümde olacağı için **docs/releases/1.9.txt** dosyasındaki "Forms" bölümünde 164 satırında bulunan Django 1.9 sürüm notlarına da eklenir:

  <pre data-gnl="1 1p"><code class="language-python">
  * A form prefix can be specified inside a form class, not only when
    instantiating a form. See :ref:`form-prefix` for details.
  </code></pre>

Belgelendirme yazma hakkında daha fazla bilgi için, sürümleme bitinin tümüyle ilgili bir açıklaması da dahil olmak üzere "[Belgeleri yazma](/en/2.0/internals/contributing/writing-documentation/)" konusuna bakınız. Bu sayfada, yerel olarak belgelerin bir kopyasını nasıl oluşturacağınıza ilişkin bir açıklama da bulunmaktadır. Böylece oluşturulacak HTML'yi önizleme yapabilirsiniz.

<hr>

## Değişikliklerinizi önizleme {#previewing-your-changes}

Şimdi, yamamızda yapılan tüm değişiklikleri yapmanın zamanı geldi. Geçerli Django kopyanız (değişikliklerinizle) ile öğreticide daha önce kullanıma sunduğunu gözden geçirme arasındaki farkları görüntülemek için:

  <pre data-gnl="1 1p"><code class="language-python">
  $ git diff
  </code></pre>

Yukarı ve aşağı taşımak için ok tuşlarını kullanın.



  <pre data-gnl="1 1p"><code class="language-python">
  diff &#45;&#45;git a/django/forms/forms.py b/django/forms/forms.py
  index 509709f..d1370de 100644
  --- a/django/forms/forms.py
  &#43;&#43;&#43; b/django/forms/forms.py
  @@ -75,6 &#43;75,7 @@ class BaseForm:
       # information. Any improvements to the form API should be made to *this*
       # class, not to the Form class.
       field_order = None
  &#43;    prefix = None

       def __init__(self, data=None, files=None, auto_id='id_%s', prefix=None,
                    initial=None, error_class=ErrorList, label_suffix=None,
  @@ -83,7 &#43;84,8 @@ class BaseForm:
           self.data = data or {}
           self.files = files or {}
           self.auto_id = auto_id
  -        self.prefix = prefix
  &#43;        if prefix is not None:
  &#43;            self.prefix = prefix
           self.initial = initial or {}
           self.error_class = error_class
           # Translators: This is the default suffix added to form field labels
  diff --git a/docs/ref/forms/api.txt b/docs/ref/forms/api.txt
  index 3bc39cd..008170d 100644
  &#45;&#45;&#45; a/docs/ref/forms/api.txt
  &#43;&#43;&#43; b/docs/ref/forms/api.txt
  @@ -1065,3 &#43;1065,13 @@ You can put several Django forms inside one ``<form>`` tag. To give each
       &gt;&gt;&gt; print(father.as_ul())
       <li><label for="id_father-first_name">First name:</label> <input type="text" name="father-first_name" id="id_father-first_name" /></li>
       <li><label for="id_father-last_name">Last name:</label> <input type="text" name="father-last_name" id="id_father-last_name" /></li>
  &#43;
  &#43;The prefix can also be specified on the form class::
  &#43;
  &#43;    &gt;&gt;&gt; class PersonForm(forms.Form):
  &#43;    ...     ...
  &#43;    ...     prefix = 'person'
  &#43;
  &#43;.. versionadded:: 1.9
  &#43;
  &#43;    The ability to specify ``prefix`` on the form class was added.
  diff --git a/docs/releases/1.9.txt b/docs/releases/1.9.txt
  index 5b58f79..f9bb9de 100644
  &#45;&#45;&#45; a/docs/releases/1.9.txt
  &#43;&#43;&#43; b/docs/releases/1.9.txt
  @@ -161,6 &#43;161,9 @@ Forms
     :attr:`~django.forms.Form.field_order` attribute, the ``field_order``
     constructor argument , or the :meth:`~django.forms.Form.order_fields` method.

  &#43;* A form prefix can be specified inside a form class, not only when
  &#43;  instantiating a form. See :ref:`form-prefix` for details.
  &#43;
   Generic Views
   ^^^^^^^^^^^^^

  diff --git a/tests/forms_tests/tests/test_forms.py b/tests/forms_tests/tests/test_forms.py
  index 690f205..e07fae2 100644
  --- a/tests/forms_tests/tests/test_forms.py
  &#43;&#43;&#43; b/tests/forms_tests/tests/test_forms.py
  @@ -1671,6 &#43;1671,18 @@ class FormsTestCase(SimpleTestCase):
           self.assertEqual(p.cleaned_data['last_name'], 'Lennon')
           self.assertEqual(p.cleaned_data['birthday'], datetime.date(1940, 10, 9))

  &#43;    def test_class_prefix(self):
  &#43;        # Prefix can be also specified at the class level.
  &#43;        class Person(Form):
  &#43;            first_name = CharField()
  &#43;            prefix = 'foo'
  &#43;
  &#43;        p = Person()
  &#43;        self.assertEqual(p.prefix, 'foo')
  &#43;
  &#43;        p = Person(prefix='bar')
  &#43;        self.assertEqual(p.prefix, 'bar')
  &#43;
       def test_forms_with_null_boolean(self):
           # NullBooleanField is a bit of a special case because its presentation (widget)
           # is different than its data. This is handled transparently, though.
  </code></pre>

Yamanın önizlemesini tamamladığınızda, komut satırına dönmek için q tuşuna basın. Düzeltme eki içeriği iyi görünüyorsa, şimdi değişiklikleri yapmanın zamanı geldi.

<hr>

## Değişiklikleri yamaya koymak {#committing-the-changes-in-the-patch}

Değişiklikleri yapmak için:

  <pre data-gnl="1 1p"><code class="language-python">
  $ git commit -a
  </code></pre>

Bu, taahhüt mesajını yazmak için bir metin düzenleyicisi açar. Taahhüt mesajı yönergelerine uygun ve aşağıdaki gibi bir mesaj yazın:

  <pre data-gnl="1 1p"><code class="language-python">
  Fixed #24788 -- Allowed Forms to specify a prefix at the class level.
  </code></pre>

<hr>

## Taahhüdün iletilmesi için çekme isteği yapılamsı {#pushing-the-commit-and-making-a-pull-request}

Düzeltmeyi tamamladıktan sonra, Github'daki çatlağınıza gönderin (farklıysa, "ticket_24788" alanını şubenizin adıyla değiştirin):

  <pre data-gnl="1 1p"><code class="language-python">
  $ git push origin ticket_24788
  </code></pre>

[Django Github](https://github.com/django/django/) sayfasını ziyaret ederek bir çekme isteği oluşturabilirsiniz. Dallarınızı "Son zamanlarda itilen dallar"'ın altında göreceksiniz. Yanındaki "Karşılaştırın ve istekte bulun" seçeneğini tıklayın.

Lütfen bu öğretici yazı için yapmayın, ancak düzeltme ekinin önizlemesini görüntüleyen bir sonraki sayfada "Çekme isteği oluştur" seçeneğini tıklarsınız.

<hr>

## Sıradaki Adımlar {#next-steps}

Tebrikler, Django'ya bir çekme isteği nasıl yapacağınızı öğrendiniz! İhtiyaç duyabileceğiniz daha ileri tekniklerin ayrıntıları [Git ve GitHub ile çalışma bölümü](/en/2.0/internals/contributing/writing-code/working-with-git/)ndedir.

Artık bu becerileri, Django'nun kod tablasını geliştirmeye yardım ederek iyi bir şekilde kullanabilirsiniz.

### Yeni katkıda bulunanlar için daha fazla bilgi {#more-information-for-new-contributors}

Django için yamalar yazmaya başlamadan önce muhtemelen katkıda bulunmak konusunda yapılacaklara biraz daha bilgiye bir göz atmanız gerekir:

- Django'nun [bilet talep etme ve yamalar gönderme](/en/2.0/internals/contributing/writing-code/submitting-patches/) hakkındaki belgeleri okuduğunuzdan emin olmalısınız. Trac görgü kurallarını, kendiniz için bilet talep etme yöntemlerinizi, yamalar için kodlama tarzını beklemenizi ve diğer birçok önemli ayrıntıyı kapsar.
- [İlk defa katkıda bulunanların Django'nun belgelerini](/en/2.0/internals/contributing/new-contributors/) de okumaları gerekir. Django ile yardımcı olmaya yeni başlayan bizler için çok iyi tavsiyeler var.
- Bundan sonra, katkıda bulunma konusunda daha fazla bilgi için hala açsanız, [Django'nun katkıda bulunulan belgelerinin](/en/2.0/internals/contributing/) geri kalanına bakabilirsiniz. Bir ton yararlı bilgi içerir ve sahip olabileceğiniz sorularınıza yanıt bulmak için kaynağınız olmalıdır.

<hr>

## İlk gerçek biletinizi bulmak {#finding-your-first-real-ticket}

Bu bilgilerin bazılarına baktıktan sonra, dışarı çıkmaya hazır olun ve sizin için bir yama yazmak için kendinize ait bir bilet bulacaksınız. Biletleri "kolay toplama" kriteriyle özel dikkat gösterin. Bu biletler genellikle daha basittir ve ilk kez katkıda bulunan kişiler için mükemmeldir. Django'ya katkıda bulunmayı öğrendiğinizde, daha zor ve karmaşık biletler için yamalar yazmaya devam edebilirsiniz.

Zaten başlamak istiyorsanız (ve kimse sizi suçlayamaz!), yamalara ihtiyaç duyan [kolay biletler listesine ve geliştirilmesi gereken yamalar](https://code.djangoproject.com/query?status=new&status=reopened&has_patch=0&easy=1&col=id&col=summary&col=status&col=owner&col=type&col=milestone&order=priority) içeren [kolay biletlere](https://code.djangoproject.com/query?status=new&status=reopened&needs_better_patch=1&easy=1&col=id&col=summary&col=status&col=owner&col=type&col=milestone&order=priority) göz atmayı deneyin. Sınama yazmaya aşina iseniz, [sınamalara ihtiyaç duyan kolay biletlerin](https://code.djangoproject.com/query?status=new&status=reopened&needs_tests=1&easy=1&col=id&col=summary&col=status&col=owner&col=type&col=milestone&order=priority) listesine de bakabilirsiniz. Django'nun bilet talep etme ve yamaları gönderme ile ilgili belgelerine bağlantıda bahsedilen [biletleri talep etme ile ilgili yönergeleri](/en/2.0/internals/contributing/writing-code/submitting-patches/) izlemeyi unutmayın.

## Çekme isteği oluşturduktan sonra ne var? {/en/2.0/intro/contributing/#what-s-next-after-creating-a-pull-request}

Bir biletin bir düzeltme paketi tamamlandıktan sonra, ikinci bir göz seti tarafından incelenmesi gerekir. Bir çekme isteği gönderdikten sonra, bilet meta verilerini, "düzeltme eki var", "sınamalara ihtiyaç duyulmaz" vb. söylemek için bilet üzerindeki bayrakları ayarlayarak güncelleyin. Böylece başkaları inceleme için bulabilir. Katkıda bulunmak mutlaka sıfırdan bir düzeltme eki yazmak anlamına gelmez. Mevcut yamaları gözden geçirmek de çok yararlı bir katkıdır. Ayrıntılar için [Ayrıştırma biletleri](/en/2.0/internals/contributing/triaging-tickets/) konusuna bakın.
