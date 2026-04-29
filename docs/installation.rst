Installation
============

Requirements
------------

LiveNeuro requires Python 3.8 or higher and an environment with Eelbrain
available. The recommended procedure is to create an environment through
``mamba`` following the official Eelbrain installation guide, then install
LiveNeuro with ``pip``:

https://eelbrain.readthedocs.io/en/stable/installing.html

Install from GitHub
-------------------

Install directly from GitHub:

.. code-block:: bash

   pip install https://github.com/liang-bo96/LiveNeuro/archive/refs/heads/main.zip

Verify Installation
-------------------

To verify the installation, run:

.. code-block:: python

   from liveneuro import LiveNeuro
   print("LiveNeuro installed successfully!")

Troubleshooting
---------------

Common Issues
^^^^^^^^^^^^^

**ImportError: No module named 'liveneuro'**

Make sure you have installed the package correctly. Try:

.. code-block:: bash

   pip install --upgrade "https://github.com/liang-bo96/LiveNeuro/archive/refs/heads/main.zip"

**Plotly/Dash version conflicts**

If you encounter version conflicts, try:

.. code-block:: bash

   pip install --upgrade plotly dash

**Missing dependencies**

If Eelbrain is missing or not importable, revisit the Eelbrain installation
guide for platform-specific environment setup.

.. code-block:: bash

   mamba install -c conda-forge eelbrain

For editable installs, test commands, and repository structure, see the
repository ``README.md``.
