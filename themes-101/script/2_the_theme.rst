#####################################
Themes 101 - Templates & Static Files
#####################################


Foreword
--------

Welcome to Themes 101. We learned in the last course how to install django CMS
from scratch. This part will cover the downloading, installation and
configuration of our `Freelancer <http://startbootstrap.com/template-overviews/freelancer/>`_
theme into our django CMS project. I strongly encourage to follow the instructions
in the previous video (Part 1) as we will be building further upon it.

Though the theme is intended to function as one-pager, bootstrap offers a veriety
of layout options. We will make use of those to split apart the layout into
multiple pages to better showcase template and placeholder configurations.

To achieve these goals, you will learn:

- how to split up a given theme and integrate it into django CMS
- how to make use of django CMS various concept to manage content
- how to configure the CMS for efficient content editing

As mentioned before is a recording without self updating features.
As such I might showvoutdated versions of Django or django CMS over time.
I will be adding youtube annotations on sections that are outdated or
different or release an update video later on depending on the severity of
the changes.


Downloading the Theme
---------------------

Let's head over to startbootstrap and download our `Freelancer <http://startbootstrap.com/template-overviews/freelancer/>`_
theme. After the download, unzip the file and place it somewhere closeby so we
can copy the content into our project.

- Rename ``templates/base.html`` to ``templates/base.bak.html``
- Copy the ``index.html`` from the themes folder into the projects ``templates/`` folder
- Rename ``index.html`` to ``base.html``

If you have the server running, visit your site through ``http://localhost:8000``
or run the server from within the ``/project`` directory using ``python manage.py runserver``.
You can also run the server using ``./manage.py runserver`` to skip typing
python all the time.

You will see your markup, very plain without any css or images. We still need to
add these. Per default Django provides a ``static`` folder where all
publicly accessible files should be added. Lets create it within our ``/project``
folder: ``project/static``. Next move all remaining files from the
theme into the static folder.

There are files we don't need, as we are not going to work with them in this tutorial
just simply remove:

- less/
- mail/
- LICENSE
- README.md


Static files
------------

Static files can be tricky to understand, the setup is a bit more involving and
tools such as ``djangocms-installe`` help you with that setup.

First lets add the following code underneath ``BASE_DIR`` within your
``mysite/settings.py`` file::

    DATA_DIR = os.path.join(BASE_DIR, 'data')

This will define our data directory where files on production will be stored.
To see how this works, look for ``'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),``
and replace it with ``'NAME': os.path.join(DATA_DIR, 'db.sqlite3'),``.

Now create a folder ``data`` within ``project/`` and move your ``db.sqlite3``
in there. You will see that after a refresh, everthing is still up and running.

In order to get static files working, we need to modify ``STATIC_ROOT``,
``MEDIA_ROOT`` and add ``STATICFILE_DIRS``::

    STATIC_ROOT = os.path.join(DATA_DIR, 'static')
    STATIC_URL = '/static/'

    MEDIA_ROOT = os.path.join(DATA_DIR, 'media')
    MEDIA_URL = '/media/'

    STATICFILES_DIRS = (
        os.path.join(BASE_DIR, 'static'),
    )

Again, STATIC_ROOT and STATIC_MEDIA do not define the location of your static
files. STATIC_URL and MEDIA_URL just define the name where your files will
later be available. Only STATICFILE_DIRS defines where your local files are
added. In this case ``project/static``.

You can now access all those static files by appending ``/static/...`` to the url,
for example ``http://localhost:8000/static/img/profile.png``.


Adapting Static Paths
---------------------

Our design still looks plain. We need to modify the template:

Add the following on the very top of your ``base.html`` file::

    {% load staticfiles %}

These are Django template loaders, we will use various loaders through this
tutorial provided from Django itself or through the django CMS. Without these
you mgiht encounter errors such as::

    TemplateSyntaxError at /en/
    Invalid block tag: 'static'

That indicates that you are using a tag such as {% trans "Close" %} without the
required load declaration.

In our case, staticfiles gives you ``{% static "" %}``. We use this to include
the staticfiles. You could just hardcode ``static/css/bootstrap.min.css`` within
the header of our ``base.html``. The benefit of static is, that you can move
your static folder around or use a CDN without changing all paths in our project.

To correctly apply this, just replace the following in your headers
<link href="..."> definition::

    {% static 'css/bootstrap.min.css' %}
    {% static 'css/freelancer.css' %}

We see that with just those two small changes, our template looks much better.
But there is still lot of replacements to do, let's go over the file and replace
all relevant static files to make use of ``{% static '' %}``.

...

Now let's see the result. Much better, have we forgotten something? Let's inspect
our network using the chrome developer tools. All good, and no error in the
console. We just finished integrating the theme - joking.


Structure
---------

We have our theme integrated and serving from our django CMS. But we cannot
start managing content yet, or influence the CMS through the toolbar. We
will learn shortly how to achieve this. But first let's cleanup a bit so
it is easier for us to reference the code.

Django allows the use of the ``{% include %}`` tag per default on all templates.
We want to use this to separate the header, footer and content from our
``base.html``. The cleaner and smaller templates are, the easier is it to
understand your project.

Let's separate the header and place it into ``templates/includes/header.html``.
Check if your template uses specific tags and also add them there. Each template
requires its own loader definition.

Do the same for the footer: ``templates/includes/footer.html``.

Confirm both have been removed. Yes or? Now let's reintegrate them using the include
tag: ``{% include "includes/header.html" %}``.

After this, we refresh and the sections are there again.

Let's do the same for the content, but this time we place it within
``templates/content.html``. We will end up with just::

    {% include "includes/header.html" %}
    {% include "content.html" %}
    {% include "includes/footer.html" %}

and the related staticfiles in the footer. base.html now looks way cleaner and it
is easy to recognise where what is located.


CMS Tags
--------

If you have a look at ``base.bak.html`` you might recognise some tags that are
not yet implemented in our template.

**render_block** loads assets defined within addons or inside your templates using
{% addtoblock "name" %}. It's important that the "css" and "js" are available
in your template. The occurence doesn't matter, it can fit your template. But
they are mandatory. You can also define additional namespaces for your personal
means (explain in more detail using an example).

**page_attribute** are attributes attached to your page. For now we are just
using "page_title" but we will add more later on.

**cms_toolbar** renders a CMS toolbar into your markup. It is imperative that
this tag is always added directly after the opening ``<body>`` tag.

**placeholder** define content editable areas. For now we use Djangos comment
functionality to disable it. We will learn about them in the next chapter.

NOTE:: Single line comments are done using``{# #}``. Multi line comments
       using ``{% comment %}{% endcomment %}``.

**block** is heavily used for Djangos template ineritance. Template inheritance
is a big thing in Django and can be compared to other template languages such
as smarty or mustache. We will digg into this later on as well.

Don't forget to add the additional template loaders. ``base.bak.html`` can now
be removed.

After a refresh, our site still looks in order and the toolbar is back on when
appending ``?edit`` to the url.


Menus
-----

Our page is still kinda static. We want to restor the menu entries so they load
dynamically from the CMS. Naturally, the navigation is located within
``includes/header.html``. Let's head over to that file and inspect the menu.
We want to replace it with the django CMS menu tag ``{% show_menu 0 0 0 0 %}``.

The menu tag requires ``cms_tags`` loader, we also need to add it to the top.


Last Steps
----------

- add meta tags
- show how we can improve the menu opening and closing
- layout templates?
