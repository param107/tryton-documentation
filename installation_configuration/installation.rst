Installation Procedure
=======================
You can directly install Tryton using pip command-line tool in your
virtualev.

.. code-block:: python

    $ pip install trytond
    $ pip install tryton
    $ pip install tryton_module_name

    Replace module_name with the name of the module you want to install

Preparing Application Servers
-----------------------------

TODO

Basic Database Configuraion
---------------------------

* Postgres is the recommended database engine for tryton
Install Posgres database. Steps for installing Postgres can be
found from `Postgres Installation <http://wiki.postgresql.org/wiki/Detailed_installation_guides/>`_
Install the database and give the database user postgres a new
password.
Installing from PyPI
--------------------
For installing tryton form Python Package Index, you can download from
`Trton PyPI Package <https://pypi.python.org/pypi/tryton/3.0.0/>`_
Download the tar.gz file of tryton and run the setup.py file to install
from PyPI.

Creating a Virtualenv
`````````````````````

`virtualenv folder_name`

Refer to `Virtualenvwrapper Docs <http://virtualenvwrapper.readthedocs.org/en/latest/>`_
for understanding and getting hands on virtualenv and virtualenvwrapper.

Create the virtualenv and activate the virtualenv you created.
Once you into the virtualenv. You can install tryton client and tryton
server using pip commands.
