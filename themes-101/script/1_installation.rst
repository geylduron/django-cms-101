#########################
Themes 101 - Installation
#########################


Foreword
--------

Welcome to Themes 101, a new format I'm starting in a set of django CMS 101
tutorials for beginners. I'm Angelo aka "finalangel" on IRC or GitHub, employd
at Divio AG and core developer of django CMS since 2011.

The main goal of this tutorial will be to setup django CMS and implement the `Freelancer
<http://startbootstrap.com/template-overviews/freelancer/>`_ Bootstrap theme [Showcase].

To achieve this objective, you will learn:

- how to install Django and the django CMS by hand and configure your setup
- how templates and static files are created, modified and maintained
- how content management is handled


Requirements
------------

The tutorials will be recorded on the OSX environments, as such installation
instructions might need adaptions according to your system.

You require a working Python and Django environment. If you are unsure about
your setup, please head over to `Python For Beginners <https://www.python.org/about/gettingstarted/>`_
and the very helpful `Quick install guide <https://docs.djangoproject.com/en/1.9/intro/install/>`_
from Django for additional information. You will figure out at the very beginning
of the first part of Themes 101 if you statisfy these requirements.

Anything that will strive away from the default installation, will be mentioned
specifically. I will be using:

- Python 2.7.5 (default OSX 10.11 version)
- Django 1.8.8 (latest 1.8.x LTS as of this recording)
- django CMS 3.2.0 (latest stable django CMS, though we will switch to develop)

In addition to this, I will be using `virtualenv <https://virtualenv.readthedocs.org/en/latest/>`_
to create an environment that has its own installation directories, that doesn't
share libraries with other projects or your system. This will prevent version
conflicts and errors that might happen between your local project and system
wide settings. There are also other alternatives available such as Docker.


Alternatives for the setup
--------------------------

This tutorial is based on a manual installation by hand. This will allow you to
get familiar with settings, configuration, requirements and maintanance from
a beginners perspective.

There are far simpler methods available to get started with such as the
`djangocms-installer <https://github.com/nephila/djangocms-installer>`_ that
serves as cookie cutter django CMS installation or Divio's own `Aldryn Cloud <aldryn.com>`_
for a near-complete hosted solution.


First steps
-----------

Let's see if your system fullfills the above mentioned requirements. Download the `repository
<https://github.com/divio/django-cms-101>`_ either through ``git clone``, if you
have `Git <https://help.github.com/articles/set-up-git/>`_ installed, or by downloading the
`ZIP file <https://github.com/divio/django-cms-101/archive/master.zip>`_ provided by
GitHub.

Once you have setup the repository, place it into your working space, open your
Terminal and ``cd`` to that folder. Next we need to run some commands:

#. cd ``themes-101/project``
#. run ``virtualenv env``
   or ``vf new django-cms-101`` for virtualfish
#. run ``source env/bin/activate``
   or ``vf activate django-cms-101`` for virtualfish
#. run ``pip install -r requirements.txt``

We want to use the latest development state of the CMS, this will showcase
how easy it is to install / uninstall requirements even to the bleeding edge
of a package. For this just run ``pip uninstall django-cms`` and download the
`django-cms <https://github.com/divio/django-cms>`_ repository simiar to django-cms-101.
Next install this version by running ``pip install -e ../path/to/repository/django-cms``.


Install Django
--------------

With this, we only have the requirements set and have no project available to us.
You can run ``pip freeze`` to see what is all available within your virtualenv.
You'll see packages such as ``Django==1.8.8``. Lets make use of that and install
our first Django application:

#. run ``django-admin startproject mysite``
#. move the files form within ``mysite/mysite/``  to ``mysite/``
#. run ``python manage.py migrate``
#. run ``python manage.py createsuperuser`` and create **admin** / **admin**
#. run ``python manage.py runserver``
#. visit `http://localhost:8000/ <http://localhost:8000/>`_ to test that everything works


Install django CMS
------------------

django CMS is based on Django. So you can make use of everything that Django
provides. You can easily implement your own Django apps into django CMS and
vice-versa. We will not cover this in that tutorial, but it is worth mentioning.

Let's install django CMS into our Django setup. Go to `docs.django-cms.org
<http://docs.django-cms.org/en/develop/>`_, click on **How-to guides**
and choose **Installing django CMS by hand**.

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

Wow, we have setup everything, lets check if our efforts bear fruit:

#. run ``python manage.py migrate``
#. run ``python manage.py runserver``
#. visit `http://localhost:8000/ <http://localhost:8000/>`_ to open django CMS

You know have a running django CMS installation. You will be greeted with the
django CMS installation screen once you have logged in using admin/admin.

Lets add our first page to get rid of the installation screen. This screen
will always appear if there are no pages around. If you have
accidently closed the modal, just open it again by clicking on the link provided
in the box.

We add ``Home`` as title and ``Hello World`` as example content. The content
field will always add a text plugin to the created page. Remember the
djangocms_text_ckeditor we installed, this will be the first plugin we use.

After this, make sure the page is published by clicking on "Publish page now".
This might not be available, which indicates that the page is already published.
Instead you will find the "View published" button. The first page on a fresh install
will always be published automatically for you. This will not be the case repeatedly.


Lets configure
--------------

We will be using the "Freelancer" theme through the entire tutorial. The theme
has the following menu structure::

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


Outro
-----

This concludes the first chapter of "Themes 101 - Installation". We have setup
django CMS and you can start discovering the various UI components of the toolbar
the Django admin and the file structure. In the next tutorial we will have a look
at static files and templates.

You'll find all mentioned links and resources within the description of this video.
If you have feedback or questions let me know in the comment section below.

Thank you for watching be sure to like, favorite, subscribe and share for more
videos on our channel.

'Till next time. Bye
