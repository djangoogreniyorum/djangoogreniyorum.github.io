---
layout: general
title: Django Öğreniyorum
---
# Yazarlar İçin

Bu belge yazarların uygulaması gereken yönergeler için yazılmıştır. Yazarlar en başta Django 2.0 sürümünün belgelerini türkçeleştirmek amacıyla aramıza katılır. Bu nedenle bahsedilecek olan konular ağırlıklı olarak türkçeleştirme işleri üzerinedir. Eğer [yazar başvurusu](https://github.com/djangoogreniyorum/djangoogreniyorum.github.io/issues/1) yaparak yazar olmamışsanız ilk olarak okumanız gereken sayfadasınız.

## Nasıl Başlayacağım?

- İlk olarak github sayfamıza [github djangoogreniyorum.github.io](https://github.com/djangoogreniyorum/djangoogreniyorum.github.io/) geçin.
- Dosyaları yüzeysel inceleyin.
- Projeyi yerel sunucunuza indirin.
- Yerel sunucu kullanmak istiyorsanız eğer şu şartlara sağlayın.
   - Jekyll düzenini bilgisayarda yüklü değilse yükleyin. Talimatlar için [tıklayın](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/).
- Resmi django sitesine giriş yapın <a href="https://djangoproject.com">https://djangoproject.com</a> ve türkçeleştirilen sayfalarda olmayan sayfaları keşfedip çeviriye başlayın.

## .md dosyalarıyla çalışacağız

Çevirilerimizi .md uzantılı dosyalara yapacağız. Bazı durumlarda .html kullanabiliriz. Ancak genel olarak .md ile çalışacağız. md dosyaları hakkında bilgi için [tıklayın](https://guides.github.com/features/mastering-markdown/). Eğer ilk defa bir md dosyası görüyorsanız kaynak dosyalarımızda oluşturulmuş md dosyalarını inceleyebilirsiniz. HTML çalışması yapmaktan daha kolay olduğunu fark edeceksiniz. Bu arada md markdown demektir. Bilmeniz gereken diğer konulara yazının devamında değindik.

## URL düzeni

Resmi djangoproject.com sitesinde alanadı altında bulunan url sözdizimlerini birebir kullanacağız. Örnek vermek gerekirse; url'de https://docs.djangoproject.com/en/2.0/intro/tutorial01/ şöyle bir değer görmektesiniz diyelim. Alan adı https://docs.djangoproject.com kullanılmakta. Bu kısım türkçeleştirme aşamasında bağ belirtirken silinir. Böylelikle sözdizimi birebir kullanılarak dosyalar sözdizimine göre oluşturulur ve kullanılır.

Örneğin biz bu sayfayı daha önceden çevirmiştik. Şimdi her iki bağlantıyı alt alta yazıp arasındaki farkı görelim.

- https://docs.djangoproject.com/en/2.0/intro/tutorial01/
- https://djangoogreniyorum.github.io/en/2.0/intro/tutorial01/

Dikkat ettiğiniz üzere sadece alan adları değiştirildi. Bunu bir değiştirilmez kural olarak görün. Çeviri yapacağınız sayfanın sadece alan adını silerek bağlantı bilgisi yanımlayacaksını. Eğer .md dosyası yazıyorsanız parantezler içine şöyle yazacaksınız;

- &#91;&#93;(/en/2.0/intro/tutorial01/)

Eğer html dosya oluşturuyorsanız şöyle yazacaksınız;

- &lt;a href="/en/2.0/intro/tutorial01/"&gt;&lt;/a&gt;

Böylelikle urlleri doğru bir şekilde belirtmiş olursunuz. Şimdi bir sonraki aşamaya gelelim.

## Dizin oluşturma ve dosyalandırma

Bir sayfayı türkçeleştirmeye karar verdiğiniz. Peki dosyayı nereye nasıl yerleştireceğiz? İşte burada dosyaları URL yoluna uygun şekilde dizinler içine yerleştireceğiz. Yukarıda verilen bağ değerine bakarak dizinler içinde ilgili dosyayı bulmaya çalışın. Böylelikle kurguyu anlamış olacaksınız.

### Değişmeli bağlar

Bazı durumlarda katı url yerine değişmeli url düzeni kullanmamız gerekir. Yani gelecekte değişme olasılığı bulunan bağlardan bahsediyoruz. İşte bu durumda bir değişken tanımlar gibi config.yml dosyası içinde bağ imi tanımlıyoruz.

Kullanımına gelince, config.yml dosyasındaki bağ imini çağırmak için süslü parantezlerden 2 tane açıp 2 tane kapatarak arasına **site.kullanilacak_bag_degeri** yazıyoruz. Böylelikle tek bir yerden bağ değerini değiştirerek her yere yansıtmamız mümkün ve hızlı olabiliyor. Örnek kullanım aşağıdaki gibidir.

  <pre><code>
&#123;&#123; site.degisken_adi &#125;&#125;
  </code></pre>

## Yeni belge oluşturduğunuzda

İlk defa bir belge dosyası oluşturuyorsanız eğer dosyanın en başına ayarları tanımlamanız gerekmektedir. Bunlar şöyledir;

  <pre><code>
  &ndash;&ndash;&ndash;
  layout: general
  title: Django Öğreniyorum Pencere Başlığı
  &ndash;&ndash;&ndash;
  </code></pre>

### Kolay gelsin.

- **not:** Eğer bu belge yeterli gelmiyorsa Django Öğrenim topluluğuna konuyu dile getirmeyi ihmal etmeyiniz.
