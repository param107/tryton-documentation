Basic Concepts
==============

TODO

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

Models Inheritence
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
To inherit an existing model (like Company), once just needs to
instantiate a class with the same _name attribute:

Extending Views
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Each inherit view must start with data tag.
** xpath **
* expr : the xpath expression to find a node in the inherited view.
* position : Define the position from the finded node, it can be before,
after, replacem, inside or replace_attributes which will change the
attributes.

** Example **

.. code-block:: xml
   :linenos:

    <configure
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
    </configure>


WebServices
-----------

TODO
