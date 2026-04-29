API Reference
=============

This page contains the complete API reference for LiveNeuro.

Main Class
----------

.. autoclass:: liveneuro.LiveNeuro
   :members: run, export_images
   :exclude-members: __init__
   :show-inheritance:

   The main visualization class for interactive 2D brain projections with activity time-course plots.

Sample Data Module
------------------

.. autofunction:: liveneuro.create_sample_brain_data

Data Format
-----------

Input Data Expectations
^^^^^^^^^^^^^^^^^^^^^^^^

**Vector Data** (with direction and magnitude):

* Eelbrain NDVar with dimensions: ``([case,] time, source, space)``
* Space dimension: 3D components (Right, Anterior, Superior)
* Built-in MNE sample data object returned by ``create_sample_brain_data()``

**Scalar Data** (magnitude only):

* Eelbrain NDVar with dimensions: ``([case,] time, source)``
* Single value per source at each time point
* Sample-data object returned by ``create_sample_brain_data(has_vector_data=False)``

**Built-in Sample Data:**

* 1589 sources in volumetric source space
* 76 time points (-100ms to 400ms)
* Vector data (3D current dipoles)

Exceptions
----------

The library may raise the following exceptions:

**ValueError**
  * Invalid ``display_mode`` string
  * Invalid ``layout_mode`` (not "vertical" or "horizontal" and not registered in LAYOUTS)
  * Invalid parameter values

**ImportError**
  * Missing required dependencies
  * Eelbrain not installed when using eelbrain-specific features

**RuntimeError**
  * Data processing errors
  * Visualization rendering errors
