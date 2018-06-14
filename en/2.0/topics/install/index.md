---
layout: general
title: Kurulum - Django Öğreniyorum
---
# Django Nasıl Yüklenir? {#how-to-install-django}

Bu belge sizi Django'ya götürecek ve birlikte çalışmaya başlatacak.

## Python Yükleyin {#install-python}

Python ağ çatısı olmak için Django'ya ihtiyaç duyar. Ayrıntılar için [Django ile hangi Python sürümünü kullanabilirim?](/en/2.0/faq/install/#faq-python-version-support) konusuna bakın.

Python'un en son sürümünü [https://www.python.org/downloads/](https://www.python.org/downloads/) adresinden indirebilir ya da işletim sisteminizin paket yöneticisinden yükleyebilirsiniz.

<div data-bilget="genel" markdown="1">
### Jython'da Django

[Jython](http://www.jython.org/) (Python, Java platformu için bir uygulamadır.) Bu yüzden Python 3 ile uyumlu değildir, bu yüzden Django ≥ 2.0 Jython'da çalıştırılamaz.
</div>

<div data-bilget="genel" markdown="1">
### Python on Windows

Sadece Django'dan başlıyorsanız ve Windows işletim örgüsü kullanıyorsanız, [Djago'nun Windows'a nasıl kurulacağı](/en/2.0/howto/windows/) konusu yararlı olabilir.
</div>

<hr>

## Kurulum Apache ve mod_wsgi {#install-apache-and-mod-wsgi}

Django ile denemek istiyorsanız bir sonraki bölüme geçelim; Django, sınamak için kullanabileceğiniz hafif bir ağ sunucusu içerir. Bu nedenle Django'yu üretim aşamasında dağıtmaya hazır olana kadar [Apache](https://httpd.apache.org/)'yi kurmanız gerekmez.

Django'yu bir üretim sitesinde kullanmak isterseniz, [mod_wsgi](http://www.modwsgi.org/) ile [Apache](https://httpd.apache.org/)'yi kullanın. [mod_wsgi](http://www.modwsgi.org/), iki durumdan birinde çalışabilir: gömülü bir durum ve bir daemon durumu. Katıştırılmış durumda, [mod_wsgi](http://www.modwsgi.org/) mod_perl'e benzer: Paython'u [Apache](https://httpd.apache.org/) içine yerleştirir ve sunucu başlatıldığında Python kodunu belleğe yükler. Kod, bir Apache sürecinin ömrü boyunca bellekte kalır. Bu da diğer sunucu düzenlemelerine göre önemli başarım artışı sağlar. Daemon durumunda, mod_wsgi istekleri işleyen bağımsız bir arka plan programı oluşturur. Süreç işlemi, ağ sunucusundan farklı bir kullanıcı olarak çalışabilir. Bu da muhtemelen geliştirilmiş güvenliği sağlar ve tüm Apache ağ sunucusunu yeniden başlatmaya gerek kalmadan daemon işlemi yeniden başlatılabilir. Bu da kod tabanımızı daha sorunsuz bir şekilde yenilemektedir. Kurulumunuz için hangi kipin doğru olduğunu belirlemek için mod_wsgi bileşeni etkinleştirildi. Django mod_wsgi'yi destekleyen herhangi bir Apache sürümüyle çalışacaktır.

Mod_wsgi'yi kurduktan sonra nasıl yapılandırılacağı hakkında bilgi için Django'yu [mod_wsgi ile birlikte nasıl kullanacağınıza](/en/2.0/howto/deployment/wsgi/modwsgi/) bakın.

Herhangi bir nedenle mod_wsgi'yi kullanamazsanız, korkmayın: Django diğer dağıtım seçeneklerini destekler. Biri [uWSGI](/en/2.0/howto/deployment/wsgi/uwsgi/); [nginx](https://nginx.org/) ile çok iyi çalışır. Buna ek olarak, Django çeşitli sunucu düzlemlerinde çalışmasına izin veren WSGI özgülleşimini ([PEP 333](https://www.python.org/dev/peps/pep-3333)) izler.

## Veritabanınızı çalıştırın {#get-your-database-running}

Django'nun veritabanı API işlevselliğini kullanmayı planlıyorsanız, bir veritabanı sunucusunun çalıştığından emin olmanız gerekir. Django birçok farklı veritabanı sunucusunu destekler ve [PostgreSQL](https://www.postgresql.org/), [MySQL](https://www.mysql.com/), [Oracle](https://www.oracle.com/) ve [SQLite](https://www.sqlite.org/) ile resmi olarak desteklenir.

Basit bir proje veya bir üretim ortamında dağıtmayı planlamadığınız bir şey geliştiriyorsanız, ayrı bir sunucu çalıştırmayı gerektirmeyen SQLite genellikle en basit seçenektir. Bununla birlikte, SQLite diğer veritabanlarından pek çok farklılığa sahiptir, dolayısıyla önemli bir şey üzerinde çalışıyorsanız, üretimde kullanmayı planladığınız aynı veritabanıyla geliştirilmesi önerilir.

Resmen desteklenen veritabanlarına ek olarak, diğer veritabanlarını Django ile kullanmanıza izin veren [3. taraflar tarafından sağlanan arka uçlar](/en/2.0/ref/databases/#third-party-notes) da vardır.

Bir veritabanı arka uç ek olarak, Python veritabanı bağlamalarınızın yüklü olduğundan emin olmanız gerekir.

- PostgreSQL kullanıyorsanız, [psycopg2](http://initd.org/psycopg/) paketine ihtiyacınız olacaktır. Daha fazla ayrıntı için [PostgreSQL](/en/2.0/ref/databases/#postgresql-notes) notlarına bakın.
- MySQL kullanıyorsanız, **mysqlclient** gibi bir [V.T. API sürücüsüne](/en/2.0/ref/databases/#mysql-db-api-drivers) ihtiyacınız olacaktır. Ayrıntılar için [MySQL arka uç için notlara](/en/2.0/ref/databases/#mysql-notes) bakın.
- SQLite kullanıyorsanız, [SQLite arka uç noktalarını](/en/2.0/ref/databases/#sqlite-notes) okumak isteyebilirsiniz.
- Oracle kullanıyorsanız, [cx_Orcale](https://oracle.github.io/python-cx_Oracle/)'ın bir kopyasına ihtiyacınız olacak, ancak **Oracle** ve **cx_Oracle**'ın desteklenen sürümleri ile ilgili ayrıntılar için lütfen [Oracle arka planının notlarını](/en/2.0/ref/databases/#oracle-notes) okuyun.
- Resmi olmayan bir 3. taraf arka plan kullanıyorsanız, ek gereksinimler için lütfen verilen belgelere bakın.

Django'nun **manage.py göç** komutunu, kalıplarınız için doğal olarak veritabanı tabloları oluşturmak için kullanmayı planlıyorsanız (ilk olarak Django yüklendikten ve bir proje oluşturduktan sonra), Django'nun sizin ouşturduğunuz veritabanında tablolar oluşturmasına ve değiştirmesine izin vermeniz gerekir. Yeniden kullanıyor; tablolar elle oluşturmayı planlıyorsanız, Django **SELECT**, **INSERT**, **UPDATE** ve **DELETE** izinleri verebilirsiniz. Bu izinlere sahip bir veritabanı kullanıcısı oluşturduktan sonra, ayrıntılarınızı projenizin ayarlar dosyasında belirtin, ayrıntılar için **DATABASES**'e bakın.

Veritabanı sorgularını sınamak için Django'nun [sınama çerçevesini](/en/2.0/topics/testing/) kullanıyorsanız, Django'nun bir sınama veritabanı oluşturmak için izin alması gerekir.

<hr>
## Django'nun eski sürümlerini kaldırın {#remove-any-old-versions-of-django}

Django yüklemenizi önceki bir sürümden yükseltiyorsanız, yeni sürümü yüklemeden önce eski Django sürümünü kaldırmanız gerekecektir.

Django'yu daha önce [pip](https://pip.pypa.io/) veya **easy_install** kullanarak kurduysanız, [pip](https://pip.pypa.io/) veya **easy_install** ile yeniden yüklemek doğal olarak eski sürüme dikkat eder; bu nedenle kendiniz yapmanız gerekmez.

Django'yu daha önce **python setup.py install** kullanarak kurduysanız, kaldırma işlemi, **django dizinini** Python **site-paketlerinizden** silmek kadar basittir. Kaldırmanız gereken dizini bulmak için, aşağıdaki kabuk komut isteminde (etkileşimli Python isteminde değil) çalıştırabilirsiniz:

  <pre data-gnl="1 1p"><code class="language-python">
  $ python -c "import django; print(django.__path__)"
  </code></pre>

<hr>

## Django kodunu yükleyin {#install-the-django-code}

Yükleme yönergeleri, dağıtıma özgü bir paketi yüklemek, en yeni resmi sürümü karşıdan yüklemek veya son geliştirme sürümünü getirip yüklemediğinize bağlı olarak biraz farklıdır.

Hangi yoldan giderseniz gidin, kolaydır.

### Pip ile resmi bir sürüm kurmak {#installing-an-official-release-with-pip}

- [Pip](https://pip.pypa.io/)'i kurun. En kolay olan, [bağımsız pip kurulumcusunu](#installing-with-get-pip-py) kullanmaktır. Dağıtımınızda zaten pip varsa, güncelliğini yitirmişse güncellemeniz gerekebilir. Eskiyse, yükleme yapmadan çalışmayacağını bilirsiniz.
- [Viertualenv](https://virtualenv.pypa.io/) ve [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/)'a bir göz atın. Bu araçlar, örgü çapında paketler yüklemekten daha basit olan ayrılmış Python ortamları sağlar. Ayrıca, yönetici ayrıcalıklarına sahip olmayan paketleri yüklemeye de izin verirler. [Katkıda bulunan ders](/en/2.0/intro/contributing/) bir sanallaştırmanın nasıl oluşturulacağıyla ilgilenir.
- Sanal bir ortam oluşturduktan ve etkinleştirdikten sonra, kabuk komut istemine Django yükle komutunu girin **pip install Django**.

<hr>

### Dağıtıma özgür bir paketin kurulması {#installing-a-distribution-specific-package}

Düzlem / dağıtımınızın resmi Django paketleri / kurulumcuları sağladığını görmek için [dağıtıma özgül notları](/en/2.0/misc/distributions/) kontrol edin. Dağıtım tarafından sağlanan paketler genellikle bağımlılıkların doğal olarak yüklenmesine ve kolay yükseltme yollarına izin verir. Bununla birlikte, bu kapetler nadiren Django'nun en yeni sürümünü içerecektir.

<hr>

### Geliştirme sürümünü yükleme {#installing-the-development-version}

<div data-bilget="genel" markdown="1">
### Django geliştirme takibi

Django'nun en son geliştirme sürümünü kullanmaya karar verirseniz, [geliştirme zaman çizelgesine](https://code.djangoproject.com/timeline) dikkat etmeniz gerekir ve [yaklaşan sürüm için sürüm notlarına](/en/2.0/releases/#development-release-notes) göz kulak olmanız gerekir. Bu kullanmak isteyebileceğiniz yeni özelliklerin yanı sıra Django kopyanızı güncellerken kodunuzda yapmanız gereken değişikler üzreinde size yardımcı olacaktır. (Kararlı sürümler için gerekli değişiklikler sürüm notlarında belgelenmiştir.)
</div>

Django kodunuzu zaman zaman son hata düzeltmeleri ve yeniliklerle güncelleyebilmek için şu yönergeleri uygulayın:

- [Git](https://git-scm.com/)'in kurulu olduğundan ve komutlarını bir kabuktan çalıştırabildiğinizden emin olun. (Bunu sınamak için bir kabuk isteminde git yardımını **git help** girin.)
- Django'nun ana geliştirme dalına şöyle bir göz atın:
    <pre data-gnl="1 1p"><code class="language-python">
    $ git clone https://github.com/django/django.git
    </code></pre>
    Bu, geçerli dizinde django dizini oluşturacaktır.
- Python çeviricinin Django'nun kodunu yükleyebildiğinden emin olun. Bunu yapmanın en kolay yolu, [virtualenv](https://virtualenv.pypa.io/), [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/) ve [pip](https://pip.pypa.io/) kullanmaktır. [Katkıda bulunan öğretici](/en/2.0/intro/contributing/) sanallaştırmanın nasıl oluşturulacağızla ilgilenir.
- Virtualenv'i kurduktan ve etkinleştirdikten sonra aşağıdaki komutu çalıştırın:
    <pre data-gnl="1 1p"><code class="language-python">
    $ pip install -e django/
    </code></pre>
    Bu, Django'nun kodunu içe aktarılabilir kılar ve ayrıca django-admin yardımcı programı komutunu kullanılabilir hale getirecektir. Başka bir deyişle, hepiniz hazırsınız!

Django kaynak kodunun kopyasını güncellemek istediğinizde **git pull** komutunu django dizininden çalıştırın. Bunu yaptığınızda, Git herhangi bir değişikliği doğal olarak indirir.
