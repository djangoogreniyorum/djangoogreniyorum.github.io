---
layout: general
title: İleri Seviyede Öğretici - Django Öğreniyorum
---
# İleri Seviyede Öğretici: Yeniden Kullanılabilir Uygulamalar Nasıl Yazılır?

Bu sayfa <a href="/en/2.0/intro/tutorial07/">Öğretici 7</a>'nin kaldığı yerden başlıyor. Ağ anketimizi, yeni projelerde tekrar kullanabileceğiniz ve diğer insanlarla paylaşabileceğiniz bağımsız bir Python paketine dönüştürüyor olacağız.

Kısa bir süre çnce 1-7 öğreticilerini tamamladık. Örnek projeyi aşağıda açıklananlarla eşleşecek şekilde incelemenizi öneririz.

## Yeniden Kullanılabilirlik Önemlidir

Bir ağ uygulamasını tasarlamak, oluşturmak, sınamak ve sürdürmek yoğun bir iştir. Birçok Python ve Django projeleri ortak sorunları paylaşmaktadır. Bu tekrarlanan işlerden bir kısmını kurtarabilirsek harika olmaz mı?

Yeniden kullanılabilirlik, Python'da yaşam biçimidir. Python Paket Dizini (PyPI) kendi Python programlarında kullanabileceğini geniş bir paket yelpazesine sahiptir. Projenize dahil edebileceğiniz mevcut yeniden kullanılabilir uygulamalar için <a href="#">Django Paketlerini</a> göz gezdirin. Django kendisi de sadece bir Python paketi. Bu, mevcut Python paketlerini veya Django uygulamalarını alıp kendi ağ projenize oluşturabileceğiniz anlamına gelir. Sadece projenizi benzersiz yapan parçaları yazmanız yeterlidir.

Üzerinde çalıştığımız gibi bir anket uygulamasına ihtiyaç duyan yeni bir proje başlattığınızı varsayalım. Bu uygulamayı nasıl yeniden kullanılabilir yaparız? Neyse ki, halen bu yolda ilerliyorsun. <a href="/en/2.0/intro/tutorial03/">Öğretici 3</a>'te, bir anket kullanarak anketleri proje düzeyinde URLconf'tan nasıl ayırdığımızı gördük. Bu yazıda, uygulamanın yeni projelerde kullanımı kolay hale getirilmesi ve başkalarının yükleyip kullanabilmesi, yayınlamaya hazır hale getirilmesi için daha ileri adımlar atacağız.

<div data-bilget="genel" markdown="1">
### Paket? Uygulama?

Bir Python paketi kolayca yeniden kullanmak için ilgili Python kodunu gruplamaya bir yol sağlar. Bir paket Python kodunun bir veya daha fazla dosyasını içerir. Bunlar "modüller" olarak da bilinir.

Bir paket "import foo.bar" veya "from foo import bar" ile alınabilir. Bir paket oluşturmak için bir dizin (sandık gibi) __init__.py özel bir dosya içermelidir; bu dosya boş olsa bile.

Bir Django uygulaması sadece bir Django projesinde kullanılması amaçlanan bir Python paketidir. Bir uygulama, kalıplar, sınamalar, URL'ler ve görünümler alt modülleri olması gibi ortak Django sözleşmelerini kullanabilir.

Daha sonra başkalarının yüklemesi kolay bir Python paketinin oluşturulması sürecini tanımlamak için ambalaj terimini kullanıyoruz. Bu biraz karışık olabilir biliyoruz.
</div>

<hr>

## Projeniz ve tekrar kullanılabilir uygulamanız

Önceki eğitimlerden sonra, projemiz şöyle görünmelidir:

  <pre data-gnl="1 1p"><code class="language-python">
    benimsite/
        manage.py
        benimsite/
            __init__.py
            settings.py
            urls.py
            wsgi.py
        anketler/
            __init__.py
            admin.py
            migrations/
                __init__.py
                0001_initial.py
            models.py
            static/
                anketler/
                    images/
                        background.png
                    style.css
            templates/
                anketler/
                    ayrinti.html
                    index.html
                    sonuclar.html
            tests.py
            urls.py
            views.py
        templates/
            admin/
                base_site.html
  </code></pre>
[Öğretici 7](/en/2.0/intro/tutorial07/)'de benimsite/templates oluşturdunuz ve [Öğretici 3]({{site.belgeler_ogretiici3}})'deki anketler/templates oluşturdunuz. Şimdi, belki de proje ve uygulama için ayrı şablon dizinleri seçmeyi seçmek daha açıktı: Anket uygulamasının parçası olan her şey anketlerde. Uygulamanın kendine yetebilmesini sağlar ve yeni bir projeye dahil edilmesini kolaylaştırır.

Anketler dizini şimdi yeni bir Django projesine kopyalanabilir ve derhal yeniden kullanılabilir. Yine de yayınlanmaya hazır değil. Bunun için başkalarının yüklemesini kolaylaştırmak için uygulamayı paketlemeliyiz.

<hr>

## Bazı önkoşulların yüklenmesi

Mevcut Python paketleme durumu, çeşitli araçlar ile biraz karışıktır. Bu yazıda, paketimizi oluşturmak için <a href="https://pypi.python.org/pypi/setuptools">setuptools</a> kullanacağız. Tavsiye edilen paketleme aleti (dağıtma çatalıyla birleştirildi). Ayrıca, <a href="https://pypi.python.org/pypi/pip">pip</a>'i kurmak ve kaldırmak için kullanacağız. Şimdi bu iki paketi kurmalısın. yardıma ihtiyacınız varsa, <a href="{{site.basliklar_kurulum}}">Django'nun pip ile nasıl kurulacağına</a> bakabilirsiniz. Kurulum araçlarını aynı yolla yükleyebilirsiniz.

<hr>

## Uygulamanızı Paketleyin

Python paketlemesi, uygulamanızı kolayca yüklenip kullanılabilecek belirli bir biçimde hazırlamak anlamına gelir. Django'nun kendisi buna çok benzer yapıdadır. Anketler gibi küçük bir uygulama için bu süreç çok zor değil.


- İlk olarak, anketler için Django projenizin dışında bir üst dizin oluşturun. Bu dizini django-anketler olarak adlandırın.
   <div data-bilget="genel" markdown>

### Uygulamanız için bir ad seçme

Paketiniz için bir ad seçerken mevcut paketlerle çakışan isimlendirmeyi önlemek için PyPI gibi kaynakları kontrol edin. Dağıtılacak bir paket oluştururken genellikle modül adının önüne koymak kullanılışlıdır. Bu, Django uygulamalarını arayan diğer kişilerin uygulamanızı Django'ya özgü olarak tanımlamasına yardımcı olur.

Uygulama etiketleri (diğer bir deyişle, uygulama paketlerine nokta şeklindeki yolun son kısmı) INSTALLED_APPS içinde benzersiz olmalıdır. Aynı etiketi Django <a href="#">katkı paketlerinden</a> herhangi biriyle (ör. Auth, admin veya messages) kullanmaktan kaçının.
    </div>

- Anketler dizinini django-anketler dizinine taşıyın.

- Aşağıdaki içeriğe sahip bir django-anketler/README.rst dosyası oluşturun:
   django-anketler/README.rst
   <pre data-gnl="1 1p"><code class="language-python">
        =====
        Anketler
        =====

        Anketler, ağ tabanlı anketleri yapmak için basit bir Django uygulamasıdır.
        Ziyaretçiler her soru için sabit sayıda cevap arasından seçim yapabilir.

        Ayrıntılı belgeler "belgeler" dizininde yer almaktadır.

        Hızlı Başla
        -----------

        1. Bunun gibi INSTALLED_APPS ayarlarınıza "anketler" ekleyin::

            INSTALLED_APPS = [
                ...
                'anketler',
            ]

        2. Projenizin urls.py dosyasında anketler URLconf'a şöyle ekleyin::

            path('anketler/', include('anketler.urls')),

        3. Anket kalıplarını oluşturmak için 'python manage.py migrate' komutunu çalıştırın.

        4. Bir anket oluşturmak için geliştirme sunucusunu başlatın ve http://127.0.0.1:8000 /admin/
           adresini ziyaret edin (Yönetici uygulamasının etkinleştirilmesi gerekir).

        5. http://127.0.0.1:8000/anketler/ ankete katılmak için ziyaret edin.
      </code></pre>
- Bir django-anketler/lisans dosyası oluşturun. Bir yetki belgesi seçmek bu öğreticinin kapsamı dışındadır, ancak bir yetki belgesi olmadan halka açık olarak yayımlanan kodun işe yaramaz olduğunu söylemek yeterlidir. Django ve birçok Django uyumlu uygulamalar BSD yetki belgesi altında dağıtılır; bununla birlikte, kendi yetki belgenizi almakta özgürsünüzdür. Yetki belgesi seçiminizin kodunuzu kimin kullanabileceğini etkileyeceğini unutmayın.

- Ardından, uygulamanın kurulması ve kurulum ayrıntıları sağlayan bir setup.py dosyası oluşturacağız. Bu dosyanın tam açıklaması bu öğreticinin kapsamı dışındadır. Ancak <a href="#">setuptools</a> belgelerinde iyi bir açıklama vardır. Aşağıdaki içeriğe sahip bir django-anketler/setup.py dosyası oluşturun:
   django-anketler/setup.py
   <pre data-gnl="1 1p"><code class="language-python">
        import os
        from setuptools import find_packages, setup

        with open(os.path.join(os.path.dirname(__file__), 'README.rst')) as readme:
            README = readme.read()

        # setup.py'nin herhangi bir yoldan çalıştırılmasına izin ver
        os.chdir(os.path.normpath(os.path.join(os.path.abspath(__file__), os.pardir)))

        setup(
            name='django-anketler',
            version='0.1',
            packages=find_packages(),
            include_package_data=True,
            license='BSD License',  # örnek yetki belgesi
            description='Ağ tabanlı anketleri yapmak için basit bir Django uygulaması.',
            long_description=README,
            url='https://www.orneksite.com/',
            author='Senin İsmin',
            author_email='seninismin@orneksite.com',
            classifiers=[
                'Environment :: Web Environment',
                'Framework :: Django',
                'Framework :: Django :: X.Y',  # uygun bir şekilde X.Y yerine sürüm yazın
                'Intended Audience :: Developers',
                'License :: OSI Approved :: BSD License',  # örnek yetki belgesi
                'Operating System :: OS Independent',
                'Programming Language :: Python',
                'Programming Language :: Python :: 3.5',
                'Programming Language :: Python :: 3.6',
                'Topic :: Internet :: WWW/HTTP',
                'Topic :: Internet :: WWW/HTTP :: Dynamic Content',
            ],
        )
      </code></pre>

- Varsayılan olarak pakete sadece Python modülleri ve paketleri dahildir. Ek dosyalar eklemek için bir MANIFEST.in dosası oluşturmamız gerekir. Önceki adımda bahsedilen <a href="#">setuptools</a> belgeleri bu dosyayı daha ayrıntılı olarak ele alır. Şablonları README.rst ve yetki belgesi dosyamızı eklemek için aşağıdaki içeriğe sahip bir django-anketler/MANIFEST.in dosyası oluşturun:

   django-anketler/MANIFEST.in
   <pre data-gnl="1 1p"><code class="language-python">
        include LICENSE
        include README.rst
        recursive-include anketler/static *
        recursive-include anketler/templates *
      </code></pre>
- İsteğe bağlı, ancak uygulamanızla ayrıntılı belgeler eklemek için önerilir. Gelecekteki belgeler için boş bir dizin django-anketelr/docs oluşturun. Django-anketler/MANIFEST.in dosyasına ek bir satır ekleyin:
   <pre data-gnl="1 1p"><code class="language-python">
recursive-include docs *
   </code></pre>

Bazı dosyalar eklemediğiniz sürece belgeler dizininin paketinize dahil edilmeyeceğini unutmayın. Pek çok Django uygulaması, belgelerini readthedocs.org gibi siteler aracılığıyla çevrimiçi olarak da sağlar.

- Paketinizi python setup.py sdist ile oluşturmayı deneyin (django-anketler'den çalıştırın). Bu, dist adlı bir dizin oluşturur ve yeni paketiniz django-anketler-0.1.tar.gz dosyası olarak alırsınız.

- Paketleme hakkında daha fazla bilgi için bkz. [Python'un Paketleme ve Dağıtma Projesi Eğitimi](https://packaging.python.org/distributing/)

<hr>

## Kendi paketini kullanma
- Anketler dizinini projenin dışına taşıdığımız için artık çalışmıyor. Şimdi bunu yeni django-anketler paketimizi yükleyerek çözeceğiz.

<div data-bilget="genel" markdown="1">
Aşağıdaki adımlar django-anketleri bir kullanıcı kitaplığı olarak yüklemektedir. Kullanıcı başına kurulumların paketin örgü genelinde kurulmasına göre çok daha fazla çıkarı vardır; örneğin yönetici erişimine sahip olmadığınız sistemlerde kullanılabilir ve kapetin sistem hizmetlerini ve makinenin diğer kullanıcılarını etkilemesini önler.

Kullanıcı başına yüklemelerin, o kullanıcı olarak çalışan örgü araçlarının davranışını hala etkileyebileceğini unutmayın, böylecek sanalenv daha sağlam bir çözümdür. (Aşağı bakın)
</div>

- Paketi yüklemek için pip kullanın (zaten kurmuştunuz değil mi?):
  <pre data-gnl="1 1p"><code class="language-python">
    pip install --user django-anketler/dist/django-anketler-0.1.tar.gz
  </code></pre>

- Şansla, Django projeniz şimdi tekrar düngün çalışmalıdır. Bunu onaylamak için sunucuyu yeniden başlatın.

- Paketi kaldırmak içinse yine pip kullanın
  <pre data-gnl="1 1p"><code class="language-python">
    pip uninstall django-anketler
  </code></pre>

<hr>

## Uygulamanızın yayınlanması

Artık django anketlerini paketledik ve sınadık. Dünyayla paylaşamya hazır durumda! Bu sadece bir örnek değilse, şimdi yapabilirsiniz.

- Paketi bir arkadaşınıza e-posta yoluyla gönderin.
- Paketi ağ sitenize yükleyip paylaşın.
- Paketi, [Python Paket İndeksi (PyPI)](https://pypi.python.org/pypi) gibi birkamu havuzunda yayınlayın. [packaging.python.org](https://packaging.python.org/distributing/#uploading-your-project-to-pypi) bunun için iyi bir öğreticiye sahiptir.

<hr>

## Python paketlerini virtualenv ile yükleme

Daha önce, anketler uygulamasını bir kullanıcı kitaplığı olarak kurduk. Bunun bazı olumsuzlukları var:

- Kullanıcı kitaplıklarının değiştirilmesi, örgümüzdeki diğer Python yazılımını etkileyebilir.
- Bu paketin (veya aynı ada sahip diğerlerinin) birden fazla sürümünü çalıştırmanız mümkün olmayacaktır.

Tipik olarak, bu durumlar yalnızca birkaç Django projesini korurken ortaya çıkar. Ne yaparlarsa yapsınlar, en iyi çözüm, <a href="https://virtualenv.pypa.io/">virtualenv</a> kullanmaktır. Bu araç, her biri kendi kitaplıkları ve paket ad alanının kopyasını içeren çok sayıda ayrılmış Python ortamını korumanıza izin verir.
