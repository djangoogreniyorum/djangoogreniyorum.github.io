---
layout: general
title: Kurulum - Django Öğreniyorum
---
# Django Nasıl Yüklenir?

Bu belge sizi Django'ya götürecek, ve birlikte çalışmaya başlatacak.

## Python Yükleyin

Python ağ çatısı olmak için Django'ya ihtiyaç duyar. Ayrıntılar için [Django ile hangi Python sürümünü kullanabilirim?](#) konusuna bakın.

Python'un en son sürümünü [https://www.python.org/downloads/](https://www.python.org/downloads/) adresinden indirebilir ya da işletim sisteminizin paket yöneticisinden yükleyebilirsiniz.

<div data-bilget="genel" markdown="1">
### Jython'da Django

[Jython](#) (Python, Java platformu için bir uygulamadır.) Bu yüzden Python 3 ile uyumlu değildir, bu yüzden Django ≥ 2.0 Jython'da çalıştırılamaz.
</div>

<div data-bilget="genel" markdown="1">
### Python on Windows

If you are just starting with Django and using Windows, you may find <a href="#">How to install Django on Windows</a> useful.
</div>

<hr>

## Install Apache and mod_wsgi

If you just want to experiment with Django, skip ahead to the next section; Django includes a lightweight web server you can use for testing, so you won’t need to set up Apache until you’re ready to deploy Django in production.

If you want to use Django on a production site, use <a href="https://httpd.apache.org/">Apache</a> with <a href="http://www.modwsgi.org/">mod_wsgi</a>. mod_wsgi can operate in one of two modes: an embedded mode and a daemon mode. In embedded mode, mod_wsgi is similar to mod_perl – it embeds Python within Apache and loads Python code into memory when the server starts. Code stays in memory throughout the life of an Apache process, which leads to significant performance gains over other server arrangements. In daemon mode, mod_wsgi spawns an independent daemon process that handles requests. The daemon process can run as a different user than the Web server, possibly leading to improved security, and the daemon process can be restarted without restarting the entire Apache Web server, possibly making refreshing your codebase more seamless. Consult the mod_wsgi documentation to determine which mode is right for your setup. Make sure you have Apache installed, with the mod_wsgi module activated. Django will work with any version of Apache that supports mod_wsgi.

See [How to use Django with mod_wsgi](#) for information on how to configure mod_wsgi once you have it installed.

If you can’t use mod_wsgi for some reason, fear not: Django supports many other deployment options. One is [uWSGI](#); it works very well with [nginx](#). Additionally, Django follows the WSGI spec ([PEP 3333](https://www.python.org/dev/peps/pep-3333)), which allows it to run on a variety of server platforms.

<hr>
## Get your database running

If you plan to use Django’s database API functionality, you’ll need to make sure a database server is running. Django supports many different database servers and is officially supported with [PostgreSQL](https://www.postgresql.org/), [MySQL](https://www.mysql.com/), [Oracle](https://www.oracle.com/) and [SQLite](https://www.sqlite.org/).

If you are developing a simple project or something you don’t plan to deploy in a production environment, SQLite is generally the simplest option as it doesn’t require running a separate server. However, SQLite has many differences from other databases, so if you are working on something substantial, it’s recommended to develop with the same database as you plan on using in production.

In addition to the officially supported databases, there are [backends provided by 3rd parties](#) that allow you to use other databases with Django.

In addition to a database backend, you’ll need to make sure your Python database bindings are installed.


- If you’re using PostgreSQL, you’ll need the [psycopg2](http://initd.org/psycopg/) package. Refer to the [PostgreSQL](#) notes for further details.
- If you’re using MySQL, you’ll need a [DB API driver](#) like mysqlclient. See [notes for the MySQL backend](#) for details.
- If you’re using SQLite you might want to read the [SQLite backend notes](#).
- If you’re using Oracle, you’ll need a copy of [cx_Oracle](https://oracle.github.io/python-cx_Oracle/), but please read the [notes for the Oracle backend](#) for details regarding supported versions of both Oracle and [cx_Oracle](https://oracle.github.io/python-cx_Oracle/).
- If you’re using an unofficial 3rd party backend, please consult the documentation provided for any additional requirements.

If you plan to use Django’s manage.py migrate command to automatically create database tables for your models (after first installing Django and creating a project), you’ll need to ensure that Django has permission to create and alter tables in the database you’re using; if you plan to manually create the tables, you can simply grant Django SELECT, INSERT, UPDATE and DELETE permissions. After creating a database user with these permissions, you’ll specify the details in your project’s settings file, see DATABASES for details.

If you’re using Django’s <a href="#">testing framework</a> to test database queries, Django will need permission to create a test database.

<hr>

## Remove any old versions of Django

If you are upgrading your installation of Django from a previous version, you will need to uninstall the old Django version before installing the new version.

If you installed Django using [pip](https://pip.pypa.io/) or easy_install previously, installing with pip or easy_install again will automatically take care of the old version, so you don’t need to do it yourself.

If you previously installed Django using python setup.py install, uninstalling is as simple as deleting the django directory from your Python site-packages. To find the directory you need to remove, you can run the following at your shell prompt (not the interactive Python prompt):

<pre data-gnl="1 1p"><code class="language-python">
$ python -c "import django; print(django.__path__)"
</code></pre>

<hr>

## Install the Django code

Installation instructions are slightly different depending on whether you’re installing a distribution-specific package, downloading the latest official release, or fetching the latest development version.

It’s easy, no matter which way you choose.

### Installing an official release with pip

This is the recommended way to install Django.

- Install [pip](https://pip.pypa.io/). The easiest is to use the [standalone pip installer](https://pip.pypa.io/en/latest/installing/#installing-with-get-pip-py). If your distribution already has pip installed, you might need to update it if it’s outdated. If it’s outdated, you’ll know because installation won’t work.
- Take a look at [virtualenv](https://virtualenv.pypa.io/) and [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/). These tools provide isolated Python environments, which are more practical than installing packages systemwide. They also allow installing packages without administrator privileges. The [contributing tutorial](#) walks through how to create a virtualenv.
- After you’ve created and activated a virtual environment, enter the command pip install Django at the shell prompt.

<hr>

## Installing a distribution-specific package

Check the [distribution specific notes](#) to see if your platform/distribution provides official Django packages/installers. Distribution-provided packages will typically allow for automatic installation of dependencies and easy upgrade paths; however, these packages will rarely contain the latest release of Django.

<hr>

## Installing the development version
<div data-bilget="genel" markdown="1">

### Tracking Django development

If you decide to use the latest development version of Django, you’ll want to pay close attention to [the development timeline](#), and you’ll want to keep an eye on the [release notes for the upcoming release](#). This will help you stay on top of any new features you might want to use, as well as any changes you’ll need to make to your code when updating your copy of Django. (For stable releases, any necessary changes are documented in the release notes.)
</div>

If you’d like to be able to update your Django code occasionally with the latest bug fixes and improvements, follow these instructions:

- Make sure that you have [Git](https://git-scm.com/) installed and that you can run its commands from a shell. (Enter git help at a shell prompt to test this.)
- Check out Django’s main development branch like so:

<pre data-gnl="1 1p"><code class="language-python">
$ git clone https://github.com/django/django.git
</code></pre>

This will create a directory django in your current directory.

- Make sure that the Python interpreter can load Django’s code. The most convenient way to do this is to use [virtualenv](https://virtualenv.pypa.io/), [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/), and [pip](https://pip.pypa.io/). The [contributing tutorial](#) walks through how to create a virtualenv.
- After setting up and activating the virtualenv, run the following command:

<pre data-gnl="1 1p"><code class="language-python">
$ pip install -e django/
</code></pre>

This will make Django’s code importable, and will also make the django-admin utility command available. In other words, you’re all set!

When you want to update your copy of the Django source code, just run the command git pull from within the django directory. When you do this, Git will automatically download any changes.
