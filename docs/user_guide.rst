User Guide
==========

LiveNeuro turns source-space time series into an interactive Dash application:
a time-course plot controls one or more 2D brain projections, and each
projection updates as you move through time.

.. note::

   Examples in this guide assume notebook use, where
   :meth:`liveneuro.LiveNeuro.run` displays the app inline. See
   :ref:`run-modes` for browser, JupyterLab, and fixed-port options.

.. _quick-start:

Quick Start
-----------

Start with the built-in sample data to check that the app opens correctly:

.. code-block:: python

   from liveneuro import LiveNeuro

   viz = LiveNeuro()
   viz.run()

Use :meth:`liveneuro.LiveNeuro.export_images` when you want static output:

.. code-block:: python

   viz.export_images(output_dir="./plots", time_idx=30)

.. _mne-data-input:

Using MNE Data
--------------

For MNE users, the most direct input is a volume vector source estimate together
with the matching source space. The source estimate supplies the data and time
values; the source space supplies the 3D coordinates needed for projection.

.. code-block:: python

   from pathlib import Path

   import mne
   from mne.minimum_norm import apply_inverse, read_inverse_operator

   from liveneuro import LiveNeuro

   sample_dir = Path(mne.datasets.sample.data_path())
   data_dir = sample_dir / "MEG" / "sample"
   subjects_dir = sample_dir / "subjects"

   evoked = mne.read_evokeds(
       data_dir / "sample_audvis-ave.fif",
       condition="Left Auditory",
       baseline=(None, 0),
       proj=True,
   )
   inverse_operator = read_inverse_operator(
       data_dir / "sample_audvis-meg-vol-7-meg-inv.fif"
   )
   stc = apply_inverse(
       evoked,
       inverse_operator,
       lambda2=1.0 / 9.0,
       method="MNE",
       pick_ori="vector",
   )
   src = mne.read_source_spaces(
       subjects_dir / "sample" / "bem" / "volume-7mm-src.fif"
   )

   viz = LiveNeuro(y=stc, src=src)
   viz.run()

``src`` is required for MNE source estimates. LiveNeuro needs source-space
coordinates for the 2D projections, while the MNE source estimate stores vertex
ids and data values. Scalar MNE :class:`mne.VolSourceEstimate` objects are not
currently accepted directly; use a vector
:class:`mne.VolVectorSourceEstimate` or convert scalar source data to an
Eelbrain :class:`eelbrain.NDVar`.

.. _eelbrain-data-input:

Using Eelbrain Data
-------------------

LiveNeuro also accepts Eelbrain :class:`eelbrain.NDVar` objects:

.. code-block:: python

   from eelbrain import datasets
   from liveneuro import LiveNeuro

   data = datasets.get_mne_sample(src="vol", ori="vector")
   y = data["src"]

   viz = LiveNeuro(y=y)
   viz.run()

Expected dimensions are ``([case,] time, source, space)`` for vector data and
``([case,] time, source)`` for scalar data. If a case dimension is present,
LiveNeuro plots the case average.

Concepts
--------

Source estimates
   LiveNeuro visualizes source activity over time. For MNE inputs, pass a
   volume vector source estimate such as
   :class:`mne.VolVectorSourceEstimate`.

Source spaces
   The matching MNE :class:`mne.SourceSpaces` object provides the source
   coordinates. This is why MNE inputs use ``LiveNeuro(y=stc, src=src)``.

Vector and scalar data
   Vector data has a 3D orientation at each source and time point. LiveNeuro
   shows its magnitude as color and its orientation as arrows. Supported scalar
   :class:`eelbrain.NDVar` data is shown as magnitude only.

Time course and projections
   The time-course plot summarizes activity across sources. Clicking a time
   point updates each brain projection to that time.

.. _run-modes:

Running The Application
-----------------------

:meth:`liveneuro.LiveNeuro.run` starts the Dash application. The common modes are:

.. list-table::
   :header-rows: 1

   * - Mode
     - Use
     - Example
   * - ``external``
     - Browser tab from a script, shell, or notebook.
     - ``viz.run(mode="external")``
   * - ``inline``
     - Embedded output in a notebook.
     - ``viz.run(mode="inline")``
   * - ``jupyterlab``
     - Separate JupyterLab tab.
     - ``viz.run(mode="jupyterlab")``

Use a fixed port when you need a predictable URL:

.. code-block:: python

   viz.run(port=8888, mode="external")

.. _display-modes:

Display Modes
-------------

``display_mode`` controls which anatomical projections are shown and in what
order. The default is ``"lyr"``: left hemisphere, coronal, right hemisphere.

.. list-table::
   :header-rows: 1

   * - Value
     - Views
     - Typical use
   * - ``"lyr"``
     - Left, coronal, right
     - Default hemisphere comparison.
   * - ``"lr"``
     - Left and right
     - Compact hemisphere view.
   * - ``"ortho"``
     - Sagittal, coronal, axial
     - MNE-style orthogonal overview.
   * - ``"x"``, ``"y"``, ``"z"``
     - One sagittal, coronal, or axial view
     - Focused inspection.
   * - ``"lyrz"`` or ``"lzry"``
     - Four views
     - Broader anatomical coverage.

Letters determine display order, so ``"xz"`` and ``"zx"`` use the same two
views in a different order.

.. _layout-modes:

Layout Modes
------------

``layout_mode`` controls where the time course appears relative to the brain
views. The implementation default is ``"horizontal"``.

.. list-table::
   :header-rows: 1

   * - Value
     - Layout
     - Good for
   * - ``"horizontal"``
     - Time course on the left, brain views on the right.
     - Wide screens, notebooks, and side-by-side comparison.
   * - ``"vertical"``
     - Time course above the brain views.
     - More horizontal space per brain projection.

Example:

.. code-block:: python

   viz = LiveNeuro(display_mode="lyrz", layout_mode="horizontal")

.. _visual-controls:

Visual Controls
---------------

Arrow controls
^^^^^^^^^^^^^^

For vector data, arrows show orientation and color shows magnitude.
``arrow_scale`` changes arrow length; ``arrow_threshold`` hides lower-magnitude
vectors.

.. code-block:: python

   viz = LiveNeuro(
       arrow_scale=0.7,
       arrow_threshold="auto",
   )

Use ``arrow_threshold="auto"`` to hide vectors below 10% of the maximum
magnitude. Use ``None`` when you want to see every vector.

Color mapping
^^^^^^^^^^^^^

``cmap`` accepts Plotly colorscale names or custom colorscale lists:

.. code-block:: python

   viz = LiveNeuro(cmap="Viridis")

LiveNeuro uses a consistent color range across views and time points. Set
``vmin`` and ``vmax`` when you need fixed limits across figures:

.. code-block:: python

   viz = LiveNeuro(vmin=-2.0, vmax=2.0)

See `Plotly built-in colorscales <https://plotly.com/python/builtin-colorscales/>`_
for available color names.

Time-course detail
^^^^^^^^^^^^^^^^^^

By default, the time-course plot shows a sampled set of source traces plus mean
and maximum activity. Use ``show_max_only=True`` for a cleaner summary:

.. code-block:: python

   viz = LiveNeuro(show_max_only=True)

Interacting With The Visualization
----------------------------------

* Hover over brain projections to inspect activity at the current time point.
* Click the time-course plot to update all brain views.
* Use mouse wheel zoom, box zoom, pan, and double-click reset from the Plotly
  toolbar.

.. _export-images:

Exporting Images
----------------

:meth:`liveneuro.LiveNeuro.export_images` saves each brain projection and the
time-course plot as separate image files:

.. code-block:: python

   result = viz.export_images(
       output_dir="./images",
       time_idx=30,
       format="png",
   )

Supported formats include ``"png"``, ``"jpg"``, ``"svg"``, and ``"pdf"``.
Kaleido is required for static export and is included in the package
dependencies.

.. _performance:

Performance
-----------

For larger datasets, reduce the number of rendered traces and arrows before
reducing the data itself:

.. code-block:: python

   viz = LiveNeuro(
       display_mode="lr",
       arrow_scale=0.7,
       arrow_threshold="auto",
       show_max_only=True,
   )

These settings render fewer brain views, hide low-magnitude arrows, and keep the
time course focused on summary traces.

.. _troubleshooting:

Troubleshooting
---------------

Installation problems
   See :ref:`installation-troubleshooting`.

Missing or invalid ``src`` with MNE data
   MNE source estimates store data and vertex ids, but LiveNeuro also needs the
   matching source-space coordinates. Pass the source space used to create the
   estimate: ``LiveNeuro(y=stc, src=src)``. See :ref:`mne-data-input`.

Scalar MNE :class:`mne.VolSourceEstimate` input fails
   LiveNeuro currently accepts MNE :class:`mne.VolVectorSourceEstimate` objects
   directly, not scalar :class:`mne.VolSourceEstimate` objects. Convert scalar
   source data to an Eelbrain :class:`eelbrain.NDVar` or use a vector source
   estimate.

Invalid ``display_mode`` or ``layout_mode``
   Check :ref:`display-modes` and :ref:`layout-modes` for supported values.

The browser URL changes on each run
   Pass a fixed port, for example ``viz.run(port=8888, mode="external")``.

Arrows are too dense
   Use ``arrow_threshold="auto"`` and reduce ``arrow_scale``.

The time-course plot is too cluttered
   Use ``show_max_only=True``.

Export fails
   Confirm that the output directory is writable and that Kaleido is installed
   in the active environment.

More diagnostic output
   Run with ``debug=True``:

   .. code-block:: python

      viz.run(debug=True)

For constructor parameters and method signatures, see
:class:`liveneuro.LiveNeuro` in the :doc:`api_reference`.
