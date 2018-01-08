---
layout: general
title: Belgeler - Django Öğreniyorum
---
## Django Belgeleri
Django hakkında ihtiyaç duyabileceğin her şey.

## Belgelendirme nasıl düzenlenmiştir?

Djang'nun çok sayıda belgeleri var. Nasıl düzenlendiğine ilişkin üst düzey bir bakış, bazı şeyleri nerede bulacağınıza bilmenize yardımcı olur:

- [Dersler](/en/2.0/intro/), ağ uygulaması oluşturmak için sizi bir dizi adımla ilerletir. Django'ya veya ağ uygulamanın geliştirilmesine yeni başladıysanız buradan başlayın. Ayrıca aşağıdaki "[İlk adımlar](/en/2.0/#index-first-steps)" a bakın.
- [Konu başlıkları](/en/2.0/topics/), önemli konuları ve kavramları oldukça yüksek bir seviyede tartışmakta ve yararlı geçmiş bilgileri açıklamaları sağlamaktadır.
- [Kaynakça rehberi](/en/2.0/ref/), API'ler ve Django'nun makinelerinin diğer yönleri için teknik kaynakçalar içerir. Nasıl çalıştığını ve nasıl kullanılacağını açıklarlar, ancak temel kavramları temel bir anlayışa sahip olduğunuzu varsayarlar.
- [Nasıl yapılır rehberi](/en/2.0/howto/) tariflerdir. Temel sorunlara ve kullanım durumlarına yönelik adımlarla size rehberlik ediyorlar. Derslerden daha ileri düzeydedirler ve Django'nun nasıl çalıştığına dair bazı bilgileri varsaymaktadırlar.

<hr>

## İlk Adımlar(#index-first-steps)

Programlamada veya Django'da yeni misiniz? Burası başlamak için uygun bir yer!

- En baştan: [Genel Bakış](/en/2.0/intro/overview/) &#124; [Kurulum](/en/2.0/intro/install/)
- Dersler: [Bölüm 1: İstekler ve Yanıtlar](/en/2.0/intro/tutorial01/) &#124; [Bölüm 2: Modeller ve yönetici sitesi](/en/2.0/intro/tutorial02/) &#124; [Bölüm 3: Görünümler ve şablonlar](/en/2.0/intro/tutorial03/) &#124; [Bölüm 4: Formlar ve genel görünümler](/en/2.0/intro/tutorial04/) &#124; [Bölüm 5: Test](/en/2.0/intro/tutorial05/) &#124; [Bölüm 6: Statik dosyalar](/en/2.0/intro/tutorial06/) &#124; [Bölüm 7: Yönetici sitesini özelleştirme](/en/2.0/intro/tutorial07/)
- İleri Düzey Rehberler: [Yeniden kullanılabilir uygulamalar nasıl yazılır?](/en/2.0/intro/reusable-apps/) &#124; [Django için ilk yamanızı yazma](/en/2.0/intro/contributing/)

## Kalıp Katmanı

Django, ağ uygulamalarınızın verilerini yapılandırmak ve işlemek için bir soyutlama katmanı (modeller) sağlar. Aşağıda bununla ilgili daha fazla bilgi edinebilirsiniz:

- Kalıplar: [Modellere Giriş](/en/2.0/topics/db/models/) &#124; [Alan biçimleri](/en/2.0/ref/models/fields/) &#124; [Dizinler](/en/2.0/ref/models/indexes/) &#124; [Meta seçenekleri](/en/2.0/ref/models/options/) &#124; [Model sınıfı](/en/2.0/ref/models/class/)
- Sorgu Setleri: [Sorgular oluşturma](/en/2.0/topics/db/queries/) &#124; [QuerySet yöntemi başvurusu](/en/2.0/ref/models/querysets/) &#124; [Arama ifadeleri](/en/2.0/ref/models/lookups/)
- Model Örnekleri: [Örnek yöntemleri](/en/2.0/ref/models/instances/) &#124; [İlgili nesnelere erişme](/en/2.0/ref/models/relations/)
- Göçler: [Göç Giriş](/en/2.0/topics/migrations/) &#124; [İşlem referansları](/en/2.0/ref/migration-operations/) &#124; [ŞemaEditör](/en/2.0/ref/schema-editor/) &#124; [Taşıma yazıları yazma](/en/2.0/howto/writing-migrations/)
- İleri düzey: [Yöneticiler](/en/2.0/topics/db/managers/) &#124; [Ham SQL](/en/2.0/topics/db/sql/) &#124; [İşlemler](/en/2.0/topics/db/transactions/) &#124; [Birleştirme](/en/2.0/topics/db/aggregation/) &#124; [Arama](/en/2.0/topics/db/search/) &#124; [Özel Alanlar](/en/2.0/howto/custom-model-fields/) &#124; [Çoklu veritabanları](/en/2.0/topics/db/multi-db/) &#124; [Özel bakışlar](/en/2.0/howto/custom-lookups/) [Sorgu ifadeleri](/en/2.0/ref/models/expressions/) &#124; [Koşullu ifadeler](/en/2.0/ref/models/conditional-expressions/) &#124; [Veritabanı işlevleri](/en/2.0/ref/models/database-functions/)
- Diğer: [Desteklenen veritabanları](/en/2.0/ref/databases/) &#124; [Eski veritabanları](/en/2.0/howto/legacy-databases/) &#124; [İlk veri sağlanması](/en/2.0/howto/initial-data/) &#124; [Veritabanı erişimini uyarlama](/en/2.0/topics/db/optimization/) &#124; [PostgreSQL'e özgü özellikler](/en/2.0/ref/contrib/postgres/)

## Görünüm (View) Katmanı

Django, bir kullanıcının isteğini işlemekten ve cevabı geri getirmekten sorumlu mantığı kapsüllemek için "görünümler" (views) kavramına sahiptir. Görünümlerle ilgili bilmeniz gereken her şeyi aşağıdaki bağlantılardan bulabilirsiniz:

- Temel bilgiler: [URLconf'lar](/en/2.0/topics/http/urls/) &#124; [İşlevleri göster](/en/2.0/topics/http/views/) &#124; [Kısayollar](/en/2.0/topics/http/shortcuts/) &#124; [Süsleyiciler](/en/2.0/topics/http/decorators/)
- Kaynakça: [Yerleşik görünümler](/en/2.0/ref/views/) &#124; [Etki / Tepki (istek/yanıt) nesneleri](/en/2.0/ref/request-response/) &#124; [TemplateResponse nesneleri](/en/2.0/ref/template-response/)
- Dosya yüklemeleri: [Genel bakış](/en/2.0/topics/http/file-uploads/) &#124; [Dosya nesneleri](/en/2.0/ref/files/file/) &#124; [Depolama API'si](/en/2.0/ref/files/storage/) &#124; [Dosyaları yönetme](/en/2.0/topics/files/) &#124; [Özel depolama alanı](/en/2.0/howto/custom-file-storage/)
- Sınıf tabanlı görünümler: [Genel bakış](/en/2.0/topics/class-based-views/) &#124; [Dahili görüntüleme görünümleri](/en/2.0/topics/class-based-views/generic-display/) &#124; [Dahili düzenleme görünümleri](/en/2.0/topics/class-based-views/generic-editing/) &#124; [Mixins'i kullanma](/en/2.0/topics/class-based-views/mixins/) &#124; [API başvurusu](/en/2.0/ref/class-based-views/) &#124; [Düzleştirilmiş dizinler](/en/2.0/ref/class-based-views/flattened-index/)
- Gelişmiş: [CSV Oluşturma](/en/2.0/howto/outputting-csv/) &#124; [PDF oluşturma](/en/2.0/howto/outputting-pdf/)
- Middleware: [Genel bakış](/en/2.0/topics/http/middleware/) &#124; [Dahili Middleware sınıflar](/en/2.0/ref/middleware/)

## Şablon Katmanı

Şablon katmanı, kullanıcıya sunulan bilgileri sunmak için tasarımcı dostu bir sözdizimi sağlar. Bunun tasarımcılar tarafından nasıl kullanılabileceğini ve programcılar tarafından nasıl genişletilebileceğini öğrenin:

- Temel bilgiler: [Genel Bakış](/en/2.0/topics/templates/)
- Tasarımcılar için: [Dil genel özet](/en/2.0/ref/templates/language/) &#124; [Dahili etiketler ve filtreler](/en/2.0/ref/templates/builtins/) &#124; [İnsansılaşma](/en/2.0/ref/contrib/humanize/)
- Programcılar için: [Şablon API](/en/2.0/ref/templates/api/) &#124; [Özel etiketler ve filtreler](/en/2.0/howto/custom-template-tags/)

## Biçemler (forms)

Django, formların oluşturulmasını ve form verilerini yönlendirmeyi kolaylaştıran zengin bir çerçeve sunar.

- Temel özellikler: [Genel bakış](/en/2.0/topics/forms/) &#124; [Form API'si](/en/2.0/ref/forms/api/) &#124; [Dahili alanlar](/en/2.0/ref/forms/fields/) &#124; [Yerleşik aygıtlar (widget)](/en/2.0/ref/forms/widgets/)
- Gelişmiş: [Modeller için formlar](/en/2.0/topics/forms/modelforms/) &#124; [Ortam bütünleştirme](/en/2.0/topics/forms/media/) &#124; [Formsets](/en/2.0/topics/forms/formsets/) &#124; [Doğrulamanın özelleştirilmesi](/en/2.0/ref/forms/validation/)

## Gelişme Süreci

Django uygulamalarının geliştirilmesi ve test edilmesinde size yardımcı olacak çeşitli bileşenler ve araçlar hakkında bilgi edinin:

- Ayarlar: [Genel bakış](/en/2.0/topics/settings/) &#124; [Tüm ayarlar listesi](/en/2.0/ref/settings/)
- Uygulamalar: [Genel bakış](/en/2.0/ref/applications/)
- İstisnalar: [Genel bakış](/en/2.0/ref/exceptions/)
- django-admin ve manage.py: [Genel bakış](/en/2.0/ref/django-admin/) &#124; [Özel komut ekleme](/en/2.0/howto/custom-management-commands/)
- Test: [Giriş](/en/2.0/topics/testing/) &#124; [Test yazma ve çalıştırma](/en/2.0/topics/testing/overview/) &#124; [Dahil edilen test araçları](/en/2.0/topics/testing/tools/) &#124; [İleri konular](/en/2.0/topics/testing/advanced/)
- Dağıtım: [Genel bakış](/en/2.0/howto/deployment/) &#124; [WSGI sunucuları](/en/2.0/howto/deployment/wsgi/) &#124; [Statik dosyaları dağıtma](/en/2.0/howto/static-files/deployment/) &#124; [Eposta ile kod hatalarını takip etme](/en/2.0/howto/error-reporting/)

## Yönetici (admin)

Django'nun en gözde özelliklerinden biri olan doğallaştırılmış yönetici arayüzü hakkında bilmeniz gereken her şeyi bulun:

- [Yönetim sitesi](/en/2.0/ref/contrib/admin/)
- [Yönetici işlemleri](/en/2.0/ref/contrib/admin/actions/)
- [Yönetici belgeleri üreticisi](/en/2.0/ref/contrib/admin/admindocs/)

## Güvenlik

Güvenlik, ağ uygulamalarının geliştirilmesinde çok önemli bir konudur ve Django, birden fazla koruma araçları ve mekanizmaları sağlar:

- [Güvenlik genel görünümü](/en/2.0/topics/security/)
- [Django'da açıklanan güvenlik sorunları](/en/2.0/releases/security/)
- [Tıkır tıkırtı koruması](/en/2.0/ref/clickjacking/)
- [Siteler arası talep sahibinin korunması](/en/2.0/ref/csrf/)
- [Şifreleme imzalama](/en/2.0/topics/signing/)
- [Güvenlik middleware](/en/2.0/ref/middleware/#security-middleware)

## Uluslararasılaşma ve yerelleştirme

Django, çok dilli ve dünya bölgesi uygulamaları geliştirme konusunda size yardımcı olacak sağlam bir uluslararasılaştırma ve yerelleştirme çerçevesi sunar:

- [Genel bakış](/en/2.0/topics/i18n/) &#124; [Uluslarasılaşma](/en/2.0/topics/i18n/translation/) &#124; [Yerelleştirme](/en/2.0/topics/i18n/translation/#how-to-create-language-files) &#124; [Yerelleştirilmiş ağ kullanıcı arabirimi biçimlendirme ve form girişi](/en/2.0/topics/i18n/formatting/)
- [Zaman dilimi](/en/2.0/topics/i18n/timezones/)

## Başarım ve Uyarlama

Kodunuzun daha verimli çalışmasına yardımcı olacak çeşitli teknikler ve araçlar vardır. Daha hızlı ve daha az örgü kaynağı kullanma.

- [Genel bakış](/en/2.0/topics/performance/)

## Coğrafi çerçeve

[GeoDjango](/en/2.0/ref/contrib/gis/), dünya çapında coğrafi bir ağ çerçevesi olmayı hedeflemektedir. Amacı GIS ağ uygulamalarını mümkün olduğunca kolay hale getirmek ve mekansal olarak etkinleştirilmiş verilerin gücünü kullanmaktır.

## Ortak ağ uygulama araçları

Django, ağ uygulamalarının geliştirilmesinde yaygın olarak ihtiyaç duyulan çok sayıda araç sunar:

- Kimlik doğrulama: [Genel bakış](/en/2.0/topics/auth/) &#124; [Kimlik doğrulama örgüsünü kullanma](/en/2.0/topics/auth/default/) &#124; [Şifre yönetimi](/en/2.0/topics/auth/passwords/) &#124; [Kimlik doğrulamayı özelleştirme](/en/2.0/topics/auth/customizing/) &#124; [API kaynakçası](/en/2.0/ref/contrib/auth/)
- [Önbellekleme (caching)](/en/2.0/topics/cache/)
- [İz bırakma (logging)](/en/2.0/topics/logging/)
- [Eposta gönderme](/en/2.0/topics/email/)
- [Beslemeler (RSS / Atom)](/en/2.0/ref/contrib/syndication/)
- [Sayfa numaralandırma](/en/2.0/topics/pagination/)
- [Mesajlar çerçevesi](/en/2.0/ref/contrib/messages/)
- [Serileştirme](/en/2.0/topics/serialization/)
- [Oturumlar](/en/2.0/topics/http/sessions/)
- [Site imgeleri (haritalar)](/en/2.0/ref/contrib/sitemaps/)
- [Sabit dosyalar yönetimi](/en/2.0/ref/contrib/staticfiles/)
- [Veri doğrulama](/en/2.0/ref/validators/)

## Diğer Temel İşlevler

Django çerçevesinin diğer temel işlevleri hakkında bilgi edinin:

- [Şartlı içerik işleme](/en/2.0/topics/conditional-view-processing/)
- [İçerik türleri ve genel ilişkiler](/en/2.0/ref/contrib/contenttypes/)
- [Düz sayfalar](/en/2.0/ref/contrib/flatpages/)
- [Yönlendirmeler](/en/2.0/ref/contrib/redirects/)
- [İmler (sinyaller)](/en/2.0/topics/signals/)
- [Örgü kontrol çerçevesi](/en/2.0/topics/checks/)
- [Siteler çerçevesi](/en/2.0/ref/contrib/sites/)
- [Django'da unikod](/en/2.0/ref/unicode/)

## Django açık kaynak projesi

Django projesinin gelişim süreci ve bunun nasıl katkıda bulunulabileceği hakkında bilgi edinin:

- Topluluk: [Katılma](/en/2.0/internals/contributing/) &#124; [Serbest bırakma işlemi](/en/2.0/internals/release-process/) &#124; [Takım düzenlemeleri](/en/2.0/internals/organization/) &#124; [Kaynak kodu deposu](/en/2.0/internals/git/) &#124; [Güvenlik politikaları](/en/2.0/internals/security/) &#124; [Posta listeleri](/en/2.0/internals/mailing-lists/)
- Tasarım felsefeleri: [Genel bakış](/en/2.0/misc/design-philosophies/)
- Belgelendirme: [Bu belgelendirme hakkıdna](/en/2.0/internals/contributing/writing-documentation/)
- Üçüncü taraf dağıtımları: [Genel bakış](/en/2.0/misc/distributions/)
- Django zamanla: [API istikrarı](/en/2.0/misc/api-stability/) &#124; [Sürüm notları ve yükseltme talimatları](/en/2.0/releases/) &#124; [Kullanımdan kaldırma zaman çizelgesi](/en/2.0/internals/deprecation/)
