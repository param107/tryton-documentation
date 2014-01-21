Getting Started
===============


Setting up a development environment
------------------------------------
In the previous chapter you have seen how to install tryton client on your
machine. Now, lets start with setting up the development environment for
tryton. 

Steps for setting up a development environment for tryton.

1. First check Python is running. To do this you can simply type **python** in
   terminal and press **enter**.Then it will open the python interactive
   shell with "**>>>**". If it works same like this, it means you have
   python in you machine and you can simply leave Python with **exit()**
   command.
   
2. Next, Install Posgres database. Steps for installing Postgres can be
   found from `Postgres Installation <http://wiki.postgresql.org/wiki/Detailed_installation_guides/>`_
   Install the database and give the database user postgres a new
   password.

3. Set up the virtual environment and install tryton client and trytond.
You can directly install Tryton using pip command-line tool in your
virtualev.

.. code-block:: python

    $ pip install trytond
    $ pip install tryton
    $ pip install tryton_module_name

    Replace module_name with the name of the module you want to install





Hello World (in progress)
------------------------

Almost every Tryton functionality that you are going to develop on a daily basis is enclosed into the modules.
In order to get an idea of available modules you should take a look inside trytond/modules. You will view many folders
in the modules. Each one comprises one set of facilities that can be installed and will be available
to the end users. Some examples are company, country, currency and party.

In order to create your own module, your first step is to create a folder inside the aforementioned directory. In
this hello world tutorial we are going to create a simple library control module:

::

    TRYTOND_HOME/trytond/modules$ mkdir library

As our first step, we are going to create the following files:

* tryton.cfg
* __init__.py
* library.py

Let's see the contents and purpose of each one in detail:

tryton.cfg
~~~~~~~~~~

This file must be present at the root of your module's directory. It contains the version of the module and a list of the
xml files the module contains. Our module does not contain any xml files yet. It may also include a list of the modules it depends on.
Later, we will see the correct syntax to include those references.

::

    [tryton]
    version=2.8.0

In the example above, we are stating that this module is meant to be used with Tryton 2.8.0. You can check
the tryton.cfg files of the existing modules to view their respective structure.


\__init__.py
~~~~~~~~~~~~

This file must be present at the root of your module's directory. It serves two main purposes: it transforms your directory
into a Python visible package (according to Python general rules) ant it also registers in the *Pool*, the entity classes
of the module.

For now, you can think of the *Pool* as a "in memory synchronized image" of your database, because Tryton
follows the so called *active record* pattern. Tryton takes care of database table creation and of the mapping
between the in-memory representation of the entity and the respective columns in the database. It also takes care of the
synchronization of the data loaded in your in-memory entities and the persistent data on the database.

Whenever we are building a module in Tryton, we deal with a high-level, object-oriented representation of our entities.
Generally, we are free from writing explicit SQL or python-sql instructions, but in order for this *magic* to happen,
Tryton's *Pool* must be "aware" of the existence of your entity classes.

::

    from trytond.pool import Pool
    from .library import *

    def register():
        Pool.register(
            Book,
            module='library', type_='model'
        )

In the example above, we are registering the *Book* class into the *Pool*. Whenever the trytond service runs, it starts with
initializing every module that is installed (more on that in the coming lines), i.e., it performs the regular Python initialization
of packages. That means the execution of the code contained inside the __init__.py.

If you are unfamiliar with the package initialization, you can think of it as performing an analogous role as
the __init__ method inside a Python class, but, in this case, it performs initialization tasks semantically relative to
the whole package.

library.py
~~~~~~~~~~

This file must be present at the root of your module's directory. According to a domain model, it contains the entity classes.


If your domain model is a commercial enterprise, your domain model would contain entities such as *SaleOrder*, *Product*,
*Customer* and so on. Our tutorial here is proposing a library domain model, where you would expect to find *Book*, *Author*,
*Publisher*, etc. A domain model encompasses real world objects that your software solution is expected to deal with.

In our tutorial, we are going to have a simple Book model. It has some fields associated with it: *title*, *isbn*, *subject*,
*abstract*. Each field has a **Type**. This type determines many aspects and behaviours of the application. For instance,
a *Char* field will be created as a *Char Varying* column inside the database. A *Text* field will be displayed as a large
text box in the Tryton Client window and so on. In order to know every field avaliable, you can check the modules inside
TRYTOND_HOME/trytond/module/fields or you can consult the `API reference <http://doc.tryton.org/3.0/trytond/doc/ref/models/fields.html#ref-models-fields>`_.


::

    from trytond.model import ModelView, ModelSQL, fields

    # list of all classes in the file
    __all__ = ['Book']


    class Book(ModelSQL, ModelView):
        # description
        'Book'
        # Internal class name. Always used as a reference inside Tryton
        # default: <modules name> + . + <class name> on Tryton
        # and on database <modules name> + _ + <class name>
        __name__ = 'library.book'
        title = fields.Char('Title', required=True)
        isbn = fields.Char('ISBN')
        subject = fields.Char('Subject')
        abstract = fields.Text('Abstract')

In our example we have defined four fields in the class. Tryton will automatically create a table in the database called
**library_book**, consisting of **nine** columns: the four defined above and another five that are present on every column
of the database:

* id
* create_date
* write_date
* create_uid
* write_uid

The first column is the **surrogate primary key** of the table. The following ones are self-explanatory, and are created
for auditing purposes. In general, we should not worry about those columns, because Tryton takes care of them for us.

After creating the directory and the three files above, the trytond server should be started using the following flags:

::

    TRYTOND_HOME/trytond/bin/trytond -d NAME_OF_THE_DATABASE -i library


The -d flag indicates the name of the database and the -i flag indicates that the module library should be installed.

When you login into the Tryton client after the above procedure, you are not going to see any changes yet, because till now,
we have netiher defined the windows (views in Tryton's parlance), nor we have defined the menus or actions to open those windows.

If you access the defined database, you are going to see the the aforementioned table created.

.. note::
What we have done so far: We have created a module, we have installed that module inside Tryton server, we have defined an entity class and Tryton has created the corresponding table in the database for us. All that with no more than 20 LOC total! Awesome!


Creating Menus
--------------

Now we have to make the user interface for our module. We need to create a menu, a menu item and the windows to be able
to input and access data.

First we are going to create, on the root of our module, a **library.xml** file. This file must be listed on the **tryton.cfg**
file, as we have mentioned before. So edit it:

::

    [tryton]
    version=2.8.0

    xml:
        library.xml

Next, lets edit the library.xml file so it will contain the declaration of our menu and its respective menu item (submenu):

library.xml
~~~~~~~~~~~
::

    <?xml version="1.0"?>
    <tryton>
        <data>
            <menuitem name="Library" sequence="0" id="menu_library"/>
            <menuitem name="Books" parent="menu_library" id="menu_books"/>
        </data>
    </tryton>

Observe that this file is a *regular* xml file. So it starts with the ordinary xml version declaration at the top, and
it has as its master element the *tryton* element, followed by a *data* element. The other elements will all be children of
*data*

In the xml file above we have declared two *menuitems*. The first one, named *Library* will be placed on the root menu of
Tryton client. Observe that it has, besides the name attribute, a sequence, that indicates the position of the menu, and
an id, that must be **unique**. This id will identify this element to the rest of the software. It will be placed on the
root menu because it has no parents.

The second *menuitem*, named *Books* has another element: a *parent* element, which points to the id of the former menu
(*id="menu_library"*), indicating that it is going to be nested on the first one.

Let's update the Tryton Server, installing the new modifications:

::

    TRYTOND_HOME/trytond/bin/trytond -d NAME_OF_THE_DATABASE -u library

Notice, now, that we have changed the flag from **-i** (install) to **-u** (update) to be in accordance with the fact that
the module is already installed and only need to be updated.

Let's also restart the Tryton client now. Remember to start it with the **-d** (development) flag, so it can update the
cache and show the changes we have just made:

::

    TRYTON_HOME/tryton/bin/tryton -d

When you log in again on the client, you are going to see that the menu *Library* and the submenu *Books* have been created.

But the menus do nothing yet. We have only declared the **existence** of the menus, but we have not yet declared the **actions**
those menus execute.

What we are going to do now is to create an action that will be triggered by the submenu *Books*. The first menu *Library*
will trigger no action, because we want it to be only a summary menu. The books menu, though, will open the windows where
we are going to input and browse the books records.
