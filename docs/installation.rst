.. _installation:

Installation
============

LiveNeuro requires Python 3.10 or higher and the scientific Python stack used by
MNE-Python and Eelbrain. For the most reliable setup, create an environment with
``mamba`` or ``conda`` first, then install LiveNeuro with ``pip``.

Create An Environment
---------------------

Follow the Eelbrain installation guide for platform-specific dependencies:

* `Eelbrain installation guide <https://eelbrain.readthedocs.io/en/stable/installing.html>`_

For an existing conda-style environment, the minimum Eelbrain install is:

.. code-block:: bash

   mamba install -c conda-forge eelbrain

Install LiveNeuro
-----------------

Install the current development version directly from GitHub:

.. code-block:: bash

   pip install https://github.com/Eelbrain/LiveNeuro/archive/refs/heads/main.zip

Verify The Install
------------------

Run a short import check:

.. code-block:: python

   from liveneuro import LiveNeuro

   viz = LiveNeuro()
   print("LiveNeuro is ready.")

Then continue with :ref:`quick-start`.

.. _installation-troubleshooting:

Installation Troubleshooting
----------------------------

``ImportError: No module named 'liveneuro'``
   Confirm that the environment running Python is the same one where LiveNeuro
   was installed, then reinstall or upgrade:

   .. code-block:: bash

      pip install --upgrade "https://github.com/Eelbrain/LiveNeuro/archive/refs/heads/main.zip"

Missing Eelbrain or scientific dependencies
   Revisit the Eelbrain installation guide, or install Eelbrain from
   conda-forge:

   .. code-block:: bash

      mamba install -c conda-forge eelbrain

Plotly or Dash version conflicts
   Upgrade the interactive plotting dependencies inside the active environment:

   .. code-block:: bash

      pip install --upgrade plotly dash

For editable installs, tests, and repository structure, see the repository
``README.md``.
