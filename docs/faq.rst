.. include:: globals.txt
.. _faq:


===
FAQ
===

.. contents::
   :local:

.. _south_support:

South support ?
---------------
South support has been removed after version 1.0
when Django <1.6 support has been removed as well.

If needed add these lines to your ``models.py``::


   from south.modelsinspector import add_introspection_rules
   add_introspection_rules([], ["^concurrency\.fields\.IntegerVersionField"])


.. _update_fields:


How is managed `update_fields`
------------------------------

It is possible to use `save(update_fields=...)` parameter without interfree with
the concurrency check algorithm

.. code-block:: python

    x1 = MyModel.objects.create(name='abc')
    x2 = MyModel.objects.get(pk=x1.pk)

    x1.save()
    x2.save(update_fields=['username'])  # raise RecordModifiedError

anyway this will NOT raise any error

.. code-block:: python

    x1 = MyModel.objects.create(name='abc')
    x2 = MyModel.objects.get(pk=x1.pk)

    x1.save(update_fields=['username'])  # skip update version number
    x2.save()  # saved


.. note:: `TriggerVersionField` will be always updated


