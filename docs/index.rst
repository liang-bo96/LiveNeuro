.. raw:: html

   <p style="text-align: right;">
     <a href="https://www.python.org/downloads/"><img src="https://img.shields.io/badge/python-3.10%2B-blue" alt="Python Version"></a>
     <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-green" alt="License"></a>
     <a href="https://liveneuro.readthedocs.io/en/latest/index.html"><img src="https://img.shields.io/badge/docs-ReadTheDocs-blue" alt="Documentation"></a>
   </p>

LiveNeuro
=========

.. image:: liveNeuron.png
   :alt: LiveNeuro visualization overview
   :align: center
   :width: 720px

**LiveNeuro** helps MNE-Python and Eelbrain users inspect volume source estimates
as interactive 2D brain projections with linked time-course plots.
It is built on Plotly and Dash, works in notebooks or a browser,
and accepts MNE volume vector source estimates and :class:`eelbrain.NDVar` objects.

Start Here
----------

* :ref:`installation`: create an environment and install LiveNeuro.
* :ref:`quick-start`: open the built-in sample visualization.
* :ref:`eelbrain-data-input`: plot an :class:`eelbrain.NDVar`.
* :ref:`mne-data-input`: plot an :class:`mne.VolVectorSourceEstimate`.
* :doc:`api_reference`: look up constructor parameters and public methods.

Highlights
----------

* Linked activity time course with click-to-navigate updates.
* Interactive sagittal, coronal, axial, and hemisphere projections.
* Vector-field arrows with scale and threshold controls.
* Notebook, JupyterLab, and external browser display modes.
* Static export for figures and presentations.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   installation
   user_guide
   api_reference

Indices and tables
------------------

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
