Basic Concepts
==============

**Models** 


**class trytond.model.Model([id[,**kwargs]])**
This is the base class that every kind of model inherits. It defines
common attributes of all models.
For details description about Models in tryton refer to `Tryton Model Docs <http://doc.tryton.org/3.0/trytond/doc/ref/models/models.html/>`_
A complete library model is explained in the previous chapter.

**Views** 


The views are used to display records of an object to th user.
In tryton, objects can have several views, it is the action, that opens
the window, that tells which views must be used. The view are built from
XML that is stored in the databases with the object.ir.ui.view.
So generally, they are defined in xml files with this kind of xml:

.. code-block:: xml
   :linenos:

    <record model="ir.ui.view" id="view_id">
        <field name="model">model name</field>
        <field name="type">type name</field>
        <field name="inherit" ref="inherit_view_id"/>
    </record>

Active Records
--------------

TODO


Transactions
------------

TODO

Extending Tryton (Inheritance)
------------------------------

Tryton modules can be easily extended. Models and Views need to be
extended using Inheritence.

**Models Inheritence** : To inherit an existing model (like Company), one just needs to
instantiate a class with the same _name attribute:

.. code-block:: python

    class Company(ModelSQL, ModelView):
        _name = "company.company"
        company_iecode = fields.Char('Company Import Export Code')

        def __init__(self):
        super(Company, self).__init__()
    
    Company()

**Extending Views** : Each inherit view must start with data tag. **xpath**

* expr : the xpath expression to find a node in the inherited view.
* position : Define the position from the finded node, it can be before,
after, replacem, inside or replace_attributes which will change the
attributes.

**Example**

.. code-block:: xml
   :linenos:

    <data>
        <xpath
            expr="/form/notebook/page/separator[@name=&quot;signature&quot;]"
            position="before">
            <label name="company_code"/>
            <field name="company_code"/>
            <label name="company"/>
            <field name="company"/>
            <label name="employee_code"/>
            <field name="employee_code"/>
        </xpath>
    </data>

Importing Wizard
------------------------------------------------------------------
A Wizard describes a series of steps defined as trytond.wizard.State. The
wizard uses a trytond.wizard.Session to store data between states.
Detailed description of wizard can be found at `Official Tryton Docs <http://doc.tryton.org/2.4/trytond/doc/topics/wizard.html/>`_

The basics:
* Each wizard is a python class that subclasses trytond.wizard.Wizard.
* The states of the wizard are attributes that are instances of
trytond.wizard.State


.. code-block:: python

   from trytond.wizard import Wizard, StateView, StateTransition, Button
   
   class PrintLibraryReportStart(ModelView):
    'Print Library Report'
    __name__ = 'library.print_report.start'

    class PrintLibraryReport(Wizard):
    'Print Softex Report'
    __name__ = 'library.print_report'

    start = StateView(
        'softex.print_report.start', 'softex.print_view_form',
        [
            Button('Cancel', 'end', 'tryton-cancel'),
            Button('Print', 'print_', 'tryton-print', default=True),
        ]
    )
    print_ = StateAction('softex.report_invoice')

    def do_print_(self, action):
        data = {
            'library': self.start.book.id,
        }
        return action, data

    def transition_print_(self):
        return 'end'

Make sure you add the Wizard model name in __init.py__ and add the xml
files in tryton.cfg file.

Add the record tag for the wizard in library.xml

.. code-block:: xml

    <record model="ir.action.wizard" id="book_print">
        <field name="name">Print Library Book</field>
        <field name="wiz_name">library.print_report</field>
    </record>   

WebServices
-----------

TODO
