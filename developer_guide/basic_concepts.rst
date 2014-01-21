Basic Concepts
==============

**Models** 


**class trytond.model.Model([id[,**kwargs]])**
This is the base class that every kind of model inherits. It defines
common attributes of all models.
For details description about Models in tryton refer to `Tryton Model Docs <http://doc.tryton.org/3.0/trytond/doc/ref/models/models.html/>`_


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
        <!--field name="inherit" ref="inherit_view_id"/-->
        <field name="arch" type="xml">
            <![CDATA[
            View XML ...
            ]]>
        </field>
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

**Models Inheritence** : To inherit an existing model (like Company), once just needs to
instantiate a class with the same _name attribute:

.. code-block:: python

    class Company(ModelSQL, ModelView):
        _name = "company.company"
        company_iecode = fields.Char('Company Import Export Code')

        def __init__(self):
        super(Company, self).__init__()
    
    Company()

**Extending Views** : Each inherit view must start with data tag.
** xpath **
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


WebServices
-----------

TODO
