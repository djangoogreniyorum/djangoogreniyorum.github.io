---
layout: general
title: Notlar Django Öğreniyorum
---
# Notlar

Django ile yeni bir projeye başladığınızda bazı şeyleri hatırlamakta zorluk çekeriz. Sürekli tekrar edilmediği için bunu nasıl yapmıştık diye kendi kendinize ürettiğiniz sorularla baş başa kalırsınız. İşte bu durumlarda ve daha fazlasında faydası olsun diye alınan notları burada paylaşıyoruz.


## Hazırlık aşamasında
- Python yüklü mü?
    - değilse resmi python sitesine gidip dilediğiniz sürümü indirip kurun.

## Sanal değişken ortamı virtualenv

Farklı python sürümlerinde veya eklenti durumlarında tek merkezdeki araçları kullanmak sağlıklı olmayabilir. Bu nedenle virtualenv yapısı kullanılmalıdır.

## init nedir?


# Yap

- Python yüklü değilse resmi sitesinden dilediğin sürümü indirip kur.
- İstediğin isimde bir dizin oluştur.
- Kabukla virtualenv oluştur. :::: virtualenv venv
    - Eğer kurulu değilse :::: pip install virtualenv
    - Etinleştirme: venv\Scripts\activate
- Django projesi oluştur. :::: django-admin startproject benimsite .
    - Django kurulu değilse :::: pip install django
