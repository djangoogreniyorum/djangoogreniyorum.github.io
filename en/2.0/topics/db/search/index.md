---
layout: general
title: Search - Django Öğreniyorum
---
# Arama {#search}

Ağ uygulamaları için ortak bir görev, kullanıcı girişi ile veritabanındaki bazı verileri aramaktır. Basit bir durumda, bu bir nesnenin listesini bir kategori ile süzebilir. Daha karmaşık bir kullanım örneği, ağırlıklandırma, kategori oluşturma, vurgulama, birden çok dilde arama ve benzeri şeyleri gerektirebilir. Bu belge, olası kullanım durumlarından ve kullanabileceğiniz araçlardan bazılarını açıklamaktadır.

[Sorgulama yapma](/en/2.0/topics/db/queries/) bölümünde kullanılanlarla aynı kalıpları ele alacağız.

## Kullanım Örnekleri {#use-cases}

### Standart metin önerileri {#standard-textual-queries}

Metin tabanlı alanlar, basit eşleme işlemleri için bir seçime sahiptir. Örneğin, böyle bir yazarın araştırımasına izin vermek isteyebilirsiniz:

  <pre data-gnl="1 1p"><code class="language-python">
  &gt;&gt;&gt; Author.objects.filter(name__contains='Terry')
  [&lt;Author: Terry Gilliam&gt;, &lt;Author: Terry Jones&gt;]
  </code></pre>

Bu, kullanıcıların yazarın adının tam bir alt dizilimini bilmesini gerektirdiğinden, çok kırılgan bir çözümdür. Daha iyi bir yaklaşım, büyük küçük harf duyarlılığı olmayan bir eşleşme olabilir (icontains), ancak bu sadece marjinal olarak daha iyidir.

<hr>

### Bir veritabanının daha gelişmiş karşılaştırma işlevleri {/en/2.0/topics/db/search/#a-database-s-more-advanced-comparison-functions}

PostgreSQL kullanıyorsanız, Django, daha karmaşık sorgulama seçeneklerinden yararlanmanıza izin vermek için [veritabanına özel araçlar sunar](/en/2.0/ref/contrib/postgres/search/). Diğer veritabanları, olasılıkla eklentiler veya kullanıcı tanımlı işlevler aracılığıyla farklı araç seçeneklerine sahiptir. Django şu an için herhangi bir destek bulmuyor. Veritabanlarının sahip olabileceği işlevsellik türlerini göstermek için PostgreSQL'in bazı örneklerini kullanacağız.

<div data-bilget="genel" markdown="1">
### Diğer veritabanlarında aramak

[**django.contrib.postgres**](/en/2.0/ref/contrib/postgres/#module-django.contrib.postgres) tarafından sağlanan arama araçlarının tamamı, [özel aramalar](/en/2.0/ref/models/lookups/) ve [veritabanı işlevleri](/en/2.0/ref/models/database-functions/) gibi tamamen ortak API'lerde oluşturulmuştur. Veritabanınıza bağlı olarak, benzer API'lere izin vermek için sorgular oluşturabilmelisiniz. Bu şekilde ulaşılamyacak belirli şeyler varsa, lütfen bir bilet açın.

</div>

Yukarıdaki örnekte, büyük küçük harfe duyarsız bir aramanın daha yararlı olacağını belirledik. İngilizce olmayan isimlerle uğraşırken, [akıcı olmayan bir karşılaştırmanın](/en/2.0/ref/contrib/postgres/lookups/#std:fieldlookup-unaccent) kullanılması daha da geniltirilebilir:

  <pre data-gnl="1 1p"><code class="language-python">
  &gt;&gt;&gt; Author.objects.filter(name__unaccent__icontains='Helen')
  [&lt;Author: Helen Mirren&gt;, &lt;Author: Helena Bonham Carter&gt;, &lt;Author: Hélène Joy&gt;]
  </code></pre>

Bu, adın farklı bir harflerle eşleştirildiği başka bir orunu gösteriyor. Bu durumda uyumsuzluk var. **Helen** aramaları **Helena** ya da **Hélène**'i alacak ama tersi değil. Başka bir seçenek ise, harf dizilerini karıştıran [**trigram_similar**](/en/2.0/ref/contrib/postgres/lookups/#std:fieldlookup-trigram_similar) karşılaştırmasını kullanmaktır.

Örneğin:

  <pre data-gnl="1 1p"><code class="language-python">
  &gt;&gt;&gt; Author.objects.filter(name__unaccent__lower__trigram_similar='Helen')
  [&lt;Author: Helen Mirren&gt;, &lt;Author: Hélène Joy&gt;]
  </code></pre>

Şimdi farklı bir sorunumuz var. Çok daha uzun olduğu gibi "Helena Bonham Carter"'ın uzun adı görünmüyor. Trigram aramaları, üç harfin tüm olasılıklarını dikkate alır ve kaç tane arama ve kaynak dizesinde gördüğünü karşılaştırır. Daha uzun isimler için, kaynak dizesinde görünen ve dolayısıyla yakın eşleşme olarak nitelendirilmeyen daha fazla olasılık var.

Buradaki karşılaştırma işlevlerinin doğru seçimi, kullandığınız diller ve aranan metin türü gibi belirli veri kümenize bağlıdır. Gördüğümüz örneklerin tamamı, kullanıcıların kaynak verilere yakın (değişen tanımlar verilerek) bir şeyler girmeleri muhtemel kısa dizeler üzerindedir.

<hr>

### Belge tabanlı arama {/en/2.0/topics/db/search/#document-based-search}

Basit veritabanı işlemleri, büyük metin bloklarını düşündüğünüzde çok basit bir yaklaşımdır. Yukarıdaki örnekler, bir dizi karakterle ilgili işlemler olarak düşünülebilirken, tam metin araması gerçek kelimelere bakar. Kullanılan örgüye bağlı olarak, aşağıdaki fikirlerden bazılarını kullanması olasıdır.

- "A", "the", "and" gibi "durdurma sözcükleri" ni yok saymak.
- "Midilli" ve "midillilerin" benzer şekilde kabul edildiği sözcükleri sıkıştırmak.
- Sözcükleri, yazıda ne sıklıkta göründükleri veya başlık veya anahtar sözcükler gibi görünen alanların önemi gibi farklı ölçütlere dayalı olarak ağırlıklandırma.

Arama yazılımını kullanmak için pek çok seçenek var. Bazıları [Elastic](https://www.elastic.co/) ve [Solr](https://lucene.apache.org/solr/). Bunlar belge tabanlı arama çözümler. Onları Django modellerinden gelen verilerle sınamak için, verilerinizi bir metinli belgeye, yani veritabanı kimliklerine yapılan geri göndermeleri de içeren bir katmana ihtiyacınız olacak. Motoru kullanan belirli bir arama belgeyi döndürdüğünde, veritabanında arama yapabilirsiniz. Bu işleme yardımcı olmak üzere tasarlanmış çeşitli üçüncü parti kitaplıklar vardır.

#### PostgreSQL desteği {#postgresql-support}

PostgreSQL'in dahili olarak tam metin arama uygulaması vardır. Bazı diğer arama motorları kadar güçlü olmasa da, veritabanınızda olmanın avantajı vardır ve bu nedenle kategorileştirme gibi diğer ilişkisel sorgularla kolayca birleştirilebilir.

[**django.contrib.postgres**](/en/2.0/ref/contrib/postgres/#module-django.contrib.postgres) modülü, bu sorguları yapmak için bazı yardımcılar sağlar. Örneğin, basit bir sorgu "peynir" den bahsedilen tüm blog girişlerini seçmek olabilir:

  <pre data-gnl="1 1p"><code class="language-python">
  &gt;&gt;&gt; Entry.objects.filter(body_text__search='cheese')
  [&lt;Entry: Cheese on Toast recipes&gt;, &lt;Entry: Pizza recipes&gt;]
  </code></pre>

Ayrıca alanları ve ilgili kalıpları bir arada süzebilirsiniz:

  <pre data-gnl="1 1p"><code class="language-python">
  &gt;&gt;&gt; Entry.objects.annotate(
  ...     search=SearchVector('blog__tagline', 'body_text'),
  ... ).filter(search='cheese')
  [
      &lt;Entry: Cheese on Toast recipes&gt;,
      &lt;Entry: Pizza Recipes&gt;,
      &lt;Entry: Dairy farming in Argentina&gt;,
  ]
  </code></pre>

Ayrıntılı bilgi için **contrib.postgres** [tam metin arama belgesi](/en/2.0/ref/contrib/postgres/search/)ne bakın.
