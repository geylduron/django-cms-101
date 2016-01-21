#########################
Themes 101 - Installation
#########################


Foreword
--------

The goal is to setup django CMS and implement the `Freelancer
<http://startbootstrap.com/template-overviews/freelancer/>`_.

Please ensure your system is setup to run Python and Django. Resources for this
can be found for Python within `Python For Beginners <https://www.python.org/about/gettingstarted/>`_
and Django within the `Quick install guide <https://docs.djangoproject.com/en/1.9/intro/install/>`_.

We are using Django 1.8.8 (latest LTS) and django CMS 3.2.x for this a tutorial
with Pythin 2.7 and OSX.

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

You can also simply install the cms locally using the latest develop state by
checking out the repository, ``pip uninstall django-cms`` and then
``pip install -e ../path/to/repository/django-cms``.


Install Django
--------------

#. run ``django-admin startproject mysite``
#. move the files form within ``mysite/mysite/``  to ``mysite/``
#. run ``python manage.py migrate``
#. run ``python manage.py createsuperuser`` and create **admin** / **admin**
#. run ``python manage.py runserver``
#. visit `http://localhost:8000/ <http://localhost:8000/>`_ to test that everything works


Install django CMS
------------------

We want to install it manually so you can get an overview of the entire installation
process. You can also use the `djangocms-installer <https://github.com/nephila/djangocms-installer>`_
or `Aldryn <aldryn.com>`_ to get started very quickly.

Go to `docs.django-cms.org <http://docs.django-cms.org/en/develop/>`_ and click
on **How-to guides** and choose **Installing django CMS by hand**.

Follow the provided instructions to achieve the *bare minimum*:

Adapt settings according to the `instructions
<http://docs.django-cms.org/en/develop/how_to/install.html#configuring-your-project-for-django-cms>`_

#. start by adding the required ``INSTALLED_APPS``.
   Be aware that ``django.contrib.messages`` is already available on the standard installation.
   Also make sure that ``djangocms_admin_style`` is before ``django.contrib.admin``
#. add the required ``MIDDLEWARE_CLASSES``, we need to be carefull here as this
   list can change, double check that all is available
#. add the required ``DIRS`` and ``context_processors`` to the ``TEMPLATES`` configuration
#. expand the ``context_processors`` to include the `Django defaults
   <https://docs.djangoproject.com/en/1.87/ref/settings/#template-context-processors>`_
#. add the required settings for ``STATIC_ROOT``, ``STATIC_URL``, ``MEDIA_ROOT`` and ``MEDIA_URL``
#. add the required ``CMS_TEMPLATES`` settings.
   For our purpose we add: ``CMS_TEMPLATES = (('base.html', 'Base'),)``
#. add the required languages to ``LANGUAGES``
#. add sites by adding ``django.contrib.sites`` to your ``INSTALLED_APPS``
   and add the ``SITE_ID = 1`` setting
#. change the default ``LANGUAGE_CODE`` value to ``en`` to match the django CMS ``LANGUAGES`` setting
#. Cleanup the file a bit (templates...)

One last step requires you to install a ``TextPlugin``. We will be using
`djangocms-text-ckeditor <https://github.com/divio/djangocms-text-ckeditor>`_
as default text plugin. Simply add ``djangocms_text_ckeditor`` to your
``INSTALLED_APPS``.

Next move to configure your urls, modify the ``project/mysite/urls.py`` to include
the `recommended url pattern <http://docs.django-cms.org/en/develop/how_to/install.html#url-configuration>`_.

Now we need to add some example templates. We continue to follow the documentation
to showcase the installation process and implement our theme later on.

#. add the example ``base.html`` template to ``project/templates``
#. skip ``template_1.html`` and ``template_2.html`` as we omitted them in ``CMS_TEMPLATES``


Running django CMS
------------------

#. run ``python manage.py migrate``
#. run ``python manage.py runserver``
#. visit `http://localhost:8000/ <http://localhost:8000/>`_ to open django CMS

You know have a running django CMS installation. You will be greeted with the
django CMS installation screen once you have logged in using admin/admin.

We need to add our first page to disable the installation screen. This screen
will always appear if there are no pages around. Let's add ``Home`` as title
and ``Hello World`` as example content. The content field will always add
a text plugin to the created page.

After this, make sure the page is published by clicking on "Publish page now".
This might not be available, which indicates that the page is already published.
Instead you will find the "View published" button.


Lets configure
--------------

We will be using the "Start Bootstrap" theme through the entire tutorial. The
theme has the following pagetree structure::

    - Portfolio
    - About
    - Contact

Let's add these pages through the CMS. For this we go to example.com > Pages and add
them. Let's publish all of them and ensure they are visible within the menu.
Disable the menu option for home, as our logo will later display that.

Next let's configure some settings. Go to example.com > Admin. As you can see
we currently have our site named "example.com" you can easily change this within
"Sites". django CMS provides multi site support, for now we only need one.
Let's get in there and change the "Display Name" to "Site" so we can reference
that menu entry easier. After hitting save and a refresh you'll already see
the result.

That's it from a configurations perspective, pretty simple or?

Note that the toolbar can always be enabled by appending ``/?edit`` to the url
or ``/?edit_off`` to disable it.
If you whish to disable the toolbar entirely, just got to Site > Disable Toolbar.
Certain states are stored within django CMS, such as if the side frame is open,
which pages you opened or if the toolbar is visible or not etc.
