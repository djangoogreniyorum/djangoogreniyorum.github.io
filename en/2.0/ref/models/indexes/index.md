---
layout: general
title: Model index reference - Django Öğreniyorum
---
# Kalıp indeks kaynakça / Model index reference {#module-django&#46;db&#46;models&#46;indexes}

<div data-bilget="varsayılan" markdown="1">
**Django 1.11. da yeni**
</div>
İndeks sınıfları, veritabanı dizinlerinin oluşturulmasını kolaylaştırır. [**Meta.indexes**](/en/2.0/ref/models/options/#django.db.models.Options.indexes) seçeneğini kullanarak eklenebilirler. Bu belge, [indeks seçeneklerini](https://docs.djangoproject.com/en/2.0/ref/models/indexes/#index-options) içeren [**Index**](/en/2.0/ref/models/indexes/#django.db.models.Index)in API kaynakçalarını açıklamaktadır.

<div data-bilget="genel" markdown="1">
### Yerleşik dizinlere başvurma

İndeksler **django.db.models.indexes**'de tanımlanır. Ancak kolaylık sağlamak için [**django.db.models**](/en/2.0/topics/db/models/#module-django.db.models) içine aktarılırlar. Standart kural, **from django.db import models** kullanmak ve indeksler için kalıp olarak başvurmaktır. **&lt;IndexClass&gt;**
</div>

## **Index** seçenekleri {#index-options}

*class* **Index(*fields=[], name=None, db_tablespace=None)*[[source]](https://docs.djangoproject.com/en/2.0/_modules/django/db/models/indexes/#Index)**

Veritabanında  bir indeks (B-Tree) oluşturur.

## **fields** {#fields}

### Index.fields {#django&#46;db&#46;models&#46;Index&#46;fields}

Dizinin (indeks) istenildiği alanların adının bir listesi.

Varsayılan olarak, dizinler her sütun için artan bir sıra ile oluşturulur. Bir sütun için azalan bir dizin olan bir dizini tanımlamak için, alanın adından önce tire ekleyin.

Örneğin, **Index(fields=['headline', '-pub_date'])** **(headline, pub_date DESC)** ile SQL oluşturur. Dizin isteği MySQL'de desteklenmiyor. Bu durumda, azalan bir dizin normal bir dizin olarak oluşturulur.

<div data-bilget="genel" markdown="1">
### SQLite üzerinde sütun sıralamasını destekler

Sütun sıralaması, SQLite 3.3.0+ ve yalnızca bazı veritabanı dosya biçimleri için desteklenir. Konular için [SQLite belgelerine](https://www.sqlite.org/lang_createindex.html) bakın.
</div>

<hr>

## **name** {#name}

### **Index.name** {#django&#46;db&#46;models&#46;Index&#46;name}

İsim dizinindir. İsim yani **name** verilmemişse Django doğal olarak bir isim çıkaracaktır. Farklı veritabanları ile uyumluluk için dizin isimleri 30 karakterden uzun olamaz ve bir sayı (0-9) veya altçizgi ( _ ) ile başlaması gerekir.

<hr>

## **db_tablespace** {#db-tablespace}

### **Index.db_tablespace** {#django&#46;db&#46;models&#46;Index&#46;db_tablespace}
<div data-bilget="varsayılan" markdown="1">
**Django 2.0. da yeni**
</div>
Bu dizin için kullanılacak [veritabanı tablolarının](/en/2.0/topics/db/tablespaces/) adı. Tek alan dizinleri için, **db_tablespace** belirtilmemişse, alanının **db_tablespace**'inde dizin  oluşturulur.

[**Field.db_tablespace**](/en/2.0/ref/models/fields/#django.db.models.Field.db_tablespace) belirtilmemişse (veya dizin birden fazla alan kullanıyorsa), kalıbın sınıf Meta'sındaki [**db_tablespace**](/en/2.0/ref/models/options/#django.db.models.Options.db_tablespace) seçeneğinde belirtilen tablolama alanında oluşturulur. Bu tablolardan hiçbiri ayalanmazsa, dizin tabloyla aynı tablolama alanında oluşturulur.

<div data-bilget="genel" markdown="1">
### Ayrıca bakınız

PostgreSQL'e özgü dizinlerin bir listesi için bkz: [**django.contrib.postgres.indexes**](/en/2.0/ref/contrib/postgres/indexes/#module-django.contrib.postgres.indexes)
</div>

[**Kalıp alanı kaynakçası (Model field references)**](/en/2.0/ref/models/fields/) | [**Kalıp _meta API (Model _meta API)**](/en/2.0/ref/models/meta/)
{: .sayfalandırma}
