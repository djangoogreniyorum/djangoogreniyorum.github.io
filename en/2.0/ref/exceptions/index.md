---
layout: general
title: Django İstisnaları - Django Öğreniyorum
---
# Django İstisnaları {#django-exceptions}

Django, bazı istisnaların yanı sıra standarPython istisnalarını da yükseltmektedir.

## Django Çekirdek İstisnaları {#module-django&#46;core&#46;exceptions}

Django çekirdek istisna sınıfları **django.core.exceptions**'da tanımlanmıştır.

### AppRegistryNotReady (Uygulama Kayıt Defteri Hazır Değil) {#appregistrynotready}

#### exception AppRgistryNotReady [| **kaynak**](/en/2.0/_modules/django/core/exceptions/#AppRegistryNotReady) {#django.core.exceptions.AppRegistryNotReady}

ORM'yi başlatan [uygulama yükleme işlemi](/en/2.0/ref/applications/#app-loading-process) tamamlanmadan önce kalıpları kullanmaya çalışırken bu istisna ortaya çıkar.

<hr>

### ObjectDoesNotExist {#objectdoesnotexist}

#### exception AppRgistryNotReady [| **kaynak**](/en/2.0/_modules/django/core/exceptions/#ObjectDoesNotExist) {#django.core.exceptions.ObjectDoesNotExist}

[DoesNotExist](/en/2.0/ref/models/instances/#django.db.models.Model.DoesNotExist) istisnaları için taban sınıf; bir **try/eccept** **ObjectDoesNotExist** için tüm kalıplar için [DoesNotExist](/en/2.0/ref/models/instances/#django.db.models.Model.DoesNotExist) istisnalarını yakalar.

[ObjectDoesNotExist](/en/2.0/ref/exceptions/#django.core.exceptions.ObjectDoesNotExist) ve [DoesNotExist](/en/2.0/ref/models/instances/#django.db.models.Model.DoesNotExist) hakkında daha fazla bilgi için [get()](/en/2.0/ref/models/querysets/#django.db.models.query.QuerySet.get) işlevine bakın.

devamı yakında.
