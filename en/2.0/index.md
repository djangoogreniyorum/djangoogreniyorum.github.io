---
layout: general
title: Belgeler - Django Öğreniyorum
---
## Django Belgeleri
Django hakkında ihtiyaç duyabileceğin her şey.

## Belgelendirme nasıl düzenlenmiştir?

Djang'nun çok sayıda belgeleri var. Nasıl düzenlendiğine ilişkin üst düzey bir bakış, bazı şeyleri nerede bulacağınıza bilmenize yardımcı olur:

- [Dersler](/en/2.0/intro/), ağ uygulaması oluşturmak için sizi bir dizi adımla ilerletir. Django'ya veya ağ uygulamanın geliştirilmesine yeni başladıysanız buradan başlayın. Ayrıca aşağıdaki "İlk adımlar" a bakın.
- [Konu başlıkları](#), önemli konuları ve kavramları oldukça yüksek bir seviyede tartışmakta ve yararlı geçmiş bilgileri açıklamaları sağlamaktadır.
- [Kaynakça rehberi](#), API'ler ve Django'nun makinelerinin diğer yönleri için teknik kaynakçalar içerir. Nasıl çalıştığını ve nasıl kullanılacağını açıklarlar, ancak temel kavramları temel bir anlayışa sahip olduğunuzu varsayarlar.
- [Nasıl yapılır rehberi](#) tariflerdir. Temel sorunlara ve kullanım durumlarına yönelik adımlarla size rehberlik ediyorlar. Derslerden daha ileri düzeydedirler ve Django'nun nasıl çalıştığına dair bazı bilgileri varsaymaktadırlar.

<hr>

## İlk Adımlar

Programlamada veya Django'da yeni misiniz? Burası başlamak için uygun bir yer!

- En baştan: [Genel Bakış](/en/2.0/intro/overview/) | [Kurulum](/en/2.0/intro/install/)
- Dersler: [Bölüm 1: İstekler ve Yanıtlar](/en/2.0/intro/tutorial01/) | [Bölüm 2: Modeller ve yönetici sitesi](/en/2.0/intro/tutorial02/) | [Bölüm 3: Görünümler ve şablonlar](/en/2.0/intro/tutorial03/) | [Bölüm 4: Formlar ve genel görünümler](/en/2.0/intro/tutorial04/) | [Bölüm 5: Test](/en/2.0/intro/tutorial05/) | [Bölüm 6: Statik dosyalar](/en/2.0/intro/tutorial06/) | [Bölüm 7: Yönetici sitesini özelleştirme](/en/2.0/intro/tutorial07/)
- İleri Düzey Rehberler: [Yeniden kullanılabilir uygulamalar nasıl yazılır?](/en/2.0/intro/reusable-apps/) | [Django için ilk yamanızı yazma](/en/2.0/intro/contributing/)

## Kalıp Katmanı

Django, ağ uygulamalarınızın verilerini yapılandırmak ve işlemek için bir soyutlama katmanı (modeller) sağlar. Aşağıda bununla ilgili daha fazla bilgi edinebilirsiniz:

- Kalıplar: [Modellere Giriş](/en/2.0/topics/db/models/) | [Alan biçimleri](/en/2.0/ref/models/fields/) | [Dizinler](/en/2.0/ref/models/indexes/) | [Meta seçenekleri](/en/2.0/ref/models/options/) | [Model sınıfı](/en/2.0/ref/models/class/)
- Sorgu Setleri: [Sorgular oluşturma](/en/2.0/topics/db/queries/) | [QuerySet yöntemi başvurusu](/en/2.0/ref/models/querysets/) | [Arama ifadeleri](/en/2.0/ref/models/lookups/)
- Model Örnekleri: [Örnek yöntemleri](/en/2.0/ref/models/instances/) | [İlgili nesnelere erişme](/en/2.0/ref/models/relations/)
- Göçler: [Göç Giriş](/en/2.0/topics/migrations/) | [İşlem referansları](/en/2.0/ref/migration-operations/) | [ŞemaEditör](/en/2.0/ref/schema-editor/) | [Taşıma yazıları yazma](/en/2.0/howto/writing-migrations/)
- İleri düzey: [Yöneticiler](/en/2.0/topics/db/managers/) | [Ham SQL](/en/2.0/topics/db/sql/) | [İşlemler](/en/2.0/topics/db/transactions/) | [Birleştirme](/en/2.0/topics/db/aggregation/) | [Arama](/en/2.0/topics/db/search/) | [Özel Alanlar](/en/2.0/howto/custom-model-fields/) | [Çoklu veritabanları](/en/2.0/topics/db/multi-db/) | [Özel bakışlar](/en/2.0/howto/custom-lookups/) [Sorgu ifadeleri](/en/2.0/ref/models/expressions/) | [Koşullu ifadeler](/en/2.0/ref/models/conditional-expressions/) | [Veritabanı işlevleri](/en/2.0/ref/models/database-functions/)
- Diğer: [Desteklenen veritabanları](/en/2.0/ref/databases/) | [Eski veritabanları](/en/2.0/howto/legacy-databases/) | [İlk veri sağlanması](/en/2.0/howto/initial-data/) | [Veritabanı erişimini uyarlama](/en/2.0/topics/db/optimization/) | [PostgreSQL'e özgü özellikler](/en/2.0/ref/contrib/postgres/)

## Görünüm (View) Katmanı

Django, bir kullanıcının isteğini işlemekten ve cevabı geri getirmekten sorumlu mantığı kapsüllemek için "görünümler" (views) kavramına sahiptir. Görünümlerle ilgili bilmeniz gereken her şeyi aşağıdaki bağlantılardan bulabilirsiniz:

- Temel bilgiler: [URLconf'lar](/en/2.0/topics/http/urls/) | [İşlevleri göster](/en/2.0/topics/http/views/) | [Kısayollar](/en/2.0/topics/http/shortcuts/) | [Süsleyiciler](/en/2.0/topics/http/decorators/)
- Kaynakça: [Yerleşik görünümler](/en/2.0/ref/views/) | [Etki / Tepki (istek/yanıt) nesneleri](/en/2.0/ref/request-response/) | [TemplateResponse nesneleri](/en/2.0/ref/template-response/)
- Dosya yüklemeleri: [Genel bakış](/en/2.0/topics/http/file-uploads/) | [Dosya nesneleri](/en/2.0/ref/files/file/) | [Depolama API'si](/en/2.0/ref/files/storage/) | [Dosyaları yönetme](/en/2.0/topics/files/) | [Özel depolama alanı](/en/2.0/howto/custom-file-storage/)
- Sınıf tabanlı görünümler: [Genel bakış](/en/2.0/topics/class-based-views/) | [Dahili görüntüleme görünümleri](/en/2.0/topics/class-based-views/generic-display/) | [Dahili düzenleme görünümleri](/en/2.0/topics/class-based-views/generic-editing/) | [Mixins'i kullanma](/en/2.0/topics/class-based-views/mixins/) | [API başvurusu](/en/2.0/ref/class-based-views/) | [Düzleştirilmiş dizinler](/en/2.0/ref/class-based-views/flattened-index/)
- Gelişmiş: [CSV Oluşturma](/en/2.0/howto/outputting-csv/) | [PDF oluşturma](/en/2.0/howto/outputting-pdf/)
- Middleware: [Genel bakış](/en/2.0/topics/http/middleware/) | [Dahili Middleware sınıflar](/en/2.0/ref/middleware/)

## Şablon Katmanı

Şablon katmanı, kullanıcıya sunulan bilgileri sunmak için tasarımcı dostu bir sözdizimi sağlar. Bunun tasarımcılar tarafından nasıl kullanılabileceğini ve programcılar tarafından nasıl genişletilebileceğini öğrenin:

- Temel bilgiler: [Genel Bakış](/en/2.0/topics/templates/)
- Tasarımcılar için: [Dil genel özet](/en/2.0/ref/templates/language/) | [Dahili etiketler ve filtreler](/en/2.0/ref/templates/builtins/) | [İnsansılaşma](/en/2.0/ref/contrib/humanize/)
- Programcılar için: [Şablon API](/en/2.0/ref/templates/api/) | [Özel etiketler ve filtreler](/en/2.0/howto/custom-template-tags/)

## Biçemler (forms)

Django, formların oluşturulmasını ve form verilerini yönlendirmeyi kolaylaştıran zengin bir çerçeve sunar.

- Temel özellikler: [Genel bakış](/en/2.0/topics/forms/) | [Form API'si](/en/2.0/ref/forms/api/) | [Dahili alanlar](/en/2.0/ref/forms/fields/) | [Yerleşik aygıtlar (widget)](/en/2.0/ref/forms/widgets/)
- Gelişmiş: [Modeller için formlar](/en/2.0/topics/forms/modelforms/) | [Ortam bütünleştirme](/en/2.0/topics/forms/media/) | [Formsets](/en/2.0/topics/forms/formsets/) | [Doğrulamanın özelleştirilmesi](/en/2.0/ref/forms/validation/)

## Gelişme Süreci

Django uygulamalarının geliştirilmesi ve test edilmesinde size yardımcı olacak çeşitli bileşenler ve araçlar hakkında bilgi edinin:

- Ayarlar: [Genel bakış](/en/2.0/topics/settings/) | [Tüm ayarlar listesi](/en/2.0/ref/settings/)
- Uygulamalar: [Genel bakış](/en/2.0/ref/applications/)
- İstisnalar: [Genel bakış](/en/2.0/ref/exceptions/)
- django-admin ve manage.py: [Genel bakış](/en/2.0/ref/django-admin/) | [Özel komut ekleme](/en/2.0/howto/custom-management-commands/)
- Test: [Giriş](/en/2.0/topics/testing/) | [Test yazma ve çalıştırma](/en/2.0/topics/testing/overview/) | [Dahil edilen test araçları](/en/2.0/topics/testing/tools/) | [İleri konular](/en/2.0/topics/testing/advanced/)
- Dağıtım: [Genel bakış](/en/2.0/howto/deployment/) | [WSGI sunucuları](/en/2.0/howto/deployment/wsgi/) | [Statik dosyaları dağıtma](/en/2.0/howto/static-files/deployment/) | [Eposta ile kod hatalarını takip etme](/en/2.0/howto/error-reporting/)

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

- [Genel bakış](/en/2.0/topics/i18n/) | [Uluslarasılaşma](/en/2.0/topics/i18n/translation/) | [Yerelleştirme](/en/2.0/topics/i18n/translation/#how-to-create-language-files) | [Yerelleştirilmiş ağ kullanıcı arabirimi biçimlendirme ve form girişi](/en/2.0/topics/i18n/formatting/)
- [Zaman dilimi](/en/2.0/topics/i18n/timezones/)

## Başarım ve Uyarlama

Kodunuzun daha verimli çalışmasına yardımcı olacak çeşitli teknikler ve araçlar vardır. Daha hızlı ve daha az örgü kaynağı kullanma.

- [Genel bakış](/en/2.0/topics/performance/)

## Coğrafi çerçeve

[GeoDjango](/en/2.0/ref/contrib/gis/), dünya çapında coğrafi bir ağ çerçevesi olmayı hedeflemektedir. Amacı GIS ağ uygulamalarını mümkün olduğunca kolay hale getirmek ve mekansal olarak etkinleştirilmiş verilerin gücünü kullanmaktır.

## Ortak ağ uygulama araçları

Django, ağ uygulamalarının geliştirilmesinde yaygın olarak ihtiyaç duyulan çok sayıda araç sunar:

- Kimlik doğrulama: [Genel bakış](/en/2.0/topics/auth/) | [Kimlik doğrulama örgüsünü kullanma](/en/2.0/topics/auth/default/) | [Şifre yönetimi](/en/2.0/topics/auth/passwords/) | [Kimlik doğrulamayı özelleştirme](/en/2.0/topics/auth/customizing/) | [API kaynakçası](/en/2.0/ref/contrib/auth/)
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

- Topluluk: [Katılma](/en/2.0/internals/contributing/) | [Serbest bırakma işlemi](/en/2.0/internals/release-process/) | [Takım düzenlemeleri](/en/2.0/internals/organization/) | [Kaynak kodu deposu](/en/2.0/internals/git/) | [Güvenlik politikaları](/en/2.0/internals/security/) | [Posta listeleri](/en/2.0/internals/mailing-lists/)
- Tasarım felsefeleri: [Genel bakış](/en/2.0/misc/design-philosophies/)
- Belgelendirme: [Bu belgelendirme hakkıdna](/en/2.0/internals/contributing/writing-documentation/)
- Üçüncü taraf dağıtımları: [Genel bakış](/en/2.0/misc/distributions/)
- Django zamanla: [API istikrarı](/en/2.0/misc/api-stability/) | [Sürüm notları ve yükseltme talimatları](/en/2.0/releases/) | [Kullanımdan kaldırma zaman çizelgesi](/en/2.0/internals/deprecation/)
