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

Django'nun geçerli sürümü (2.0+) Python 2.7'yi desteklemez. Python 3'ü [Python'nun indirme sayfası](https://www.python.org/downloads/)ndan veya işletim örgünüzün paket yöneticisinden edinin.
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

# DEVAMI y-a-k-ı-n-d-a
