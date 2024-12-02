Usage
=====

.. _installation:

Installation
------------
To set up and run the RAG App, follow these steps:

1. **Clone the repository**:
   Clone the repository using the following command:

.. code-block:: console

   git clone https://github.com/chegri1/apprag
2. **Install the dependencies**:
Install the required Python libraries by running:
.. code-block:: console

   pip install -r requirements.txt

3. **Run the app**:
To run the app, execute the following command:
.. code-block:: console

   streamlit run apprag.py

Creating recipes
----------------

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. autofunction:: lumache.get_random_ingredients

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']

