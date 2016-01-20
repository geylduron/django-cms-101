#########################
Themes 101 - Installation
#########################


Foreword
--------

The goal is to setup django CMS and django-cms-explorer with the bare minimum
to have a starting project to develop our new `Freelancer
<http://startbootstrap.com/template-overviews/freelancer/>`_ theme with it.

Please ensure your system is setup to run Python and Django. Resources for this
can be found for Python within `Python For Beginners <https://www.python.org/about/gettingstarted/>`_
and Django within the `Quick install guide <https://docs.djangoproject.com/en/1.9/intro/install/>`_.

If you have problems understanding ``middlewares`` or certain mechanics within Django
and Python, I recommend starting to `Learn Python the hard way <http://learnpythonthehardway.org/>`_
or studying the `Django documentation <https://www.djangoproject.com/start/>_`.

We are also using `virtualenv <https://virtualenv.readthedocs.org/en/latest/>`_ to create an
environment that has its own installation directories, that doesn't share libraries
with other projects or your system. There are other alternatives such as Docker,
for this tutorial we follow a simple path.

You can check if your system does run Django once we test the installation locally
on the next two steps:


Setup the project locally
-------------------------

#. cd ``themes-101/project``
#. run ``virtualenv env``
   or ``vf new django-cms-101`` for virtualfish
#. run ``source env/bin/activate``
   or ``vf activate django-cms-101`` for virtualfish
#. run ``pip install -r requirements.txt``


Install Django
--------------

#. run ``django-admin startproject mysite``
#. move the files form within ``mysite/mysite/``  to ``mysite/``
#. run ``python manage.py migrate``
#. run ``python manage.py runserver``
#. visit `http://localhost:8000/ <http://localhost:8000/>`_ to test that everything works


Install django CMS
------------------

Go to `docs.django-cms.org <http://docs.django-cms.org/en/develop/>`_ and click
on **How-to guides** and choose **Installing django CMS by hand**.

Follow the provided instructions to achieve the *bare minimum*:

#. adapt settings according to the `instructions
   <http://docs.django-cms.org/en/develop/how_to/install.html#configuring-your-project-for-django-cms>`_
   by adding the required ``INSTALLED_APPS``.
   Be aware that ``django.contrib.messages`` is already available on the standard installation.
   Also make sure that ``djangocms_admin_style`` is before ``django.contrib.admin``
#. add the required ``MIDDLEWARE_CLASSES``
#. add the required ``DIRS`` and ``context_processors`` to the ``TEMPLATES`` configuration
#. add the required settings for ``STATIC_ROOT``, ``STATIC_URL``, ``MEDIA_ROOT`` and ``MEDIA_URL``
#. add the required ``CMS_TEMPLATES`` settings.
   For our purpose we add: ``CMS_TEMPLATES = (('base.html', 'Base'),)``
#. add the required languages to ``LANGUAGES``
#. add sites by adding ``django.contrib.sites`` to your ``INSTALLED_APPS``
   and add the ``SITE_ID = 1`` setting
#. change the default ``LANGUAGE_CODE`` value to ``en`` to match the django CMS ``LANGUAGES`` setting

Next move to configure your urls, modify the ``project/mysite/urls.py`` to include
the `recommended url pattern <http://docs.django-cms.org/en/develop/how_to/install.html#url-configuration>`_.

Now we need to add some example templates. We continue to follow the documentation
to showcase the installation process and implement our theme later on.

#. add the example ``base.html`` template to ``project/mysite/templates``
#. skip ``template_1.html`` and ``template_2.html`` as we omitted them in ``CMS_TEMPLATES``


Running django CMS
------------------

#. run ``python manage.py migrate``
#. run ``python manage.py runserver``
