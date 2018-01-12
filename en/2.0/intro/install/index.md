---
layout: general
title: Django Kurulumu - Django Öğreniyorum
---

# Hızlı Kurulum Rehberi {#quick-install-guide}

Django'yu kullanmadan önce, onu yüklemeniz gerekli. Tüm olasılıkları kapsayan eksiksiz bir [kurulum rehberimiz](/en/2.0/topics/install/) var; bu rehber size kurulum süreci boyunca ihtiyacınız olan adımları gösterecek.

## Python Kur {#install-python}

Python ağ çatısına dönüştürülebilmesi için Django'ya gerek duyar. [Django ile hangi Python sürümünü kullanabilirim?](/en/2.0/faq/install/#faq-python-version-support) konusunun ayrıntılarını inceleyin. Python, [SQLite](https://sqlite.org/) adlı hafif bir veritabanına sahiptir. Bu nedenle başlangıçta veritabanı kurmanıza gerek kalmayacaktır.

Python'un en son sürümünü <a href="https://www.python.org/downloads/">https://www.python.org/downloads/</a> adresinden veya işletim sisteminizin paket yöneticisinden edinebilrisiniz.

Kabuğunuzdan **python** yazarak Python'un kurulu oluşunu doğrulayabilirsiniz; şöyle bir şey göreceksiniz:

  <pre data-gnl="1 1p"><code class="language-python">
  Python 3.4.x
  [GCC 4.x] on linux
  Type "help", "copyright", "credits" or "license" for more information.
  &gt;&gt;&gt;
  </code></pre>

<hr>

## Veritabanı Kurmak {#set-up-a-database}

Bu adım yalnızda PostgreSQL, MySQL veya Oracle gibi "büyük" bir veritabanı motoruyla çalışmak istiyorsanız gereklidir. Böyle bir veritabanı yüklemek için [veritabanı kurulum bilgilerine](/en/2.0/topics/install/#database-installation) bakın.

<hr>

## Django'nun Eski Sürümlerini Kaldırın {#remove-any-old-versions-of-django}

Django yüklemenizi önceki bir sürümden yükselterek yapıyorsanız, yeni sürümü yüklemeden önce [eski Django sürümünü kaldırmanız](/en/2.0/topics/install/#removing-old-versions-of-django) gerekecektir.

<hr>

## Django'yu Yükle {#install-django}

Django'yu yüklemek için üç basit seçeneğiniz var.

- [Resmi bir sürüm kurun.](/en/2.0/topics/install/#installing-official-release) Çoğu kullanıcı için en iyi yaklaşım budur.
- [İşletim sistemi dağıtımınız tarafından sağlanan](/en/2.0/topics/install/#installing-distribution-package) Django sürümünü yükleyin.
- [En iyi geliştirme sürümünü yükleyin.](/en/2.0/topics/install/#installing-development-version) Bu seçenek en iyi ve en yeni özellikleri isteyen ve yeni kodları çalıştırmaktan çekinmeyen meraklıları içindir. Geliştirme sürümünde yeni hatalarla karşılaşmanız olasıdır. Ancak rapor ederek Django'nun geliştirimesine yardımcı olabilirsiniz. Ve yine üçüncü taraf paketlerinin sürümleri, en son kararlı sürüme göre uyumlu olma olasılığı düşük olacaktır.

<div data-bilget="genel" markdown="1">
### Daima, kullandığınız Django sürümüne karşılık gelen belgelere bakın!

İlk iki adımdan birini yaparsanız, **geliştirme sürümünde yeni** olarak işaretlenmiş belge bölümlerine göz atın. Bu cümle, yalnızca Django geliştirme sürümlerinde bulunan özellikleri işaretler ve büyük olasılıkla kararlı bir sürümle çalışmazlar.
</div>

<hr>

## Doğrulama {#verifying}

Django'nun Python taraından görülebildiğini doğrulamak için kabuğunuzdan **python** yazın. Sonra Python komut isteminde Django'yu almayı deneyin:

  <pre data-gnl="1 1p"><code class="language-python">
  &lt;&lt;&lt; import django
  &lt;&lt;&lt; print(django.get_version())
  2.0
  </code></pre>

Sizde Django'nun başka bir sürümü kurulu olabilir.

<hr>

## İşte Bu Kadar! {#that-s-it}

Öğreticiye geçebilirsiniz... [Buradan devam edin.](/en/2.0/intro/tutorial01/)

[**Bir bakışta Django**](/en/2.0/intro/overview/) | [**İlk django uygulamanızı yazma, bölüm 1**](/en/2.0/intro/tutorial01/)
{: .sayfalandırma}
