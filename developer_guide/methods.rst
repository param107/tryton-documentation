Methods
=====================================================================

Static Methods
---------------------------------------------------------------
Static Methods are used adding default values to the fields. In the below
example a static method is created to set the default value for the
startdate field.

.. code-block:: python
   
   @staticmethod
   def default_startdate():
       return startdate.today()


Parse Methods
---------------------------------------------------------------
Parse Methods are basically passing data on to reports. Example of parse
method is shown below. In this methods company.company method is parsed to
get the data and generate the reports.


.. code-block:: python

    @classmethod
    def parse(cls, report, objects, data, localcontext):
        Company = Pool().get('company.company')

        company = Company(data['company'])

        localcontext.update({
            'company': company,
        })

        return super(SoftexReport, cls).parse(
            report, objects, data, localcontext
        )



