.. _case_6:

R2D - Landslides
================

Author: Luis Angel Guerrero Hoyos
---------------------

Introduction
------------

This page describes a preliminarly approach for landslide risk assesment using SimCenter tools (R2D).


Problem Description
-------------------

Studying landslide assessment using Newmark analysis and ground motions is a multidisciplinary effort that integrates engineering, geology, and environmental science. It provides a comprehensive approach to understanding and mitigating the risks associated with landslides, ultimately leading to safer communities and more resilient infrastructure. Here we would like to analyze in a regional scale, what would be the response across a study area located in Seattle, WA, US. The idea is to estimate the predicted displacement in the study area if an earthquake with certain intensity measure (Arias Intensity :math:`I_a`) were to occur. This study integrates the infinite slope approach to estimate the static factor of safety so then the Critical Acceleration :math:`a_c` can be estimated. Finally with these parameters a Newmark displacement could be estimated using Randall W. Jibson correlation :cite:`Jibson1993`.


Solution Strategy
-----------------

Jibson proposed model is used to calculate the Newmark displacement as follows:

.. math::
    log(Dn) = 1.460\:log(I_a)-6.642\:a_c+1.546\:\:\:\:\:\:\:\:\:\:\:\:\:(1)

Where:

:math:`Dn` = Newmark Displacement [cm].

:math:`I_a` = Areas Intensity [m/s].

:math:`a_c` = Critical Acceleration [g].

Also, 

.. math::
    a_c = (FS_{static}-1)\:sin(\beta)\:\:\:\:\:\:\:\:\:\:\:\:\:(2)

Where:

.. math::
    FS_{static} = \frac{2c}{\gamma\:z\:sin(\beta)} + cot(\beta)\:tan(\phi)\:\:\:\:\:\:\:\:\:\:\:\:\:(3)

:math:`\beta` = Slope Angle [°].

:math:`FS_{static}` = Static Factor of Safety [-].

:math:`c` = Cohesion [kPa].

:math:`\gamma` = Unit Weight [kN/m³].

:math:`\phi` = Friction Angle [°].

:math:`I_a` = Intensity measure time-history motion specific. 

In order to estimate the imput parameters of equation 1 the following steps could be done:

#. Create a slope raster for a selected area: this can be created using a DTM model for the area of interest which can be downloaded from the `USGS portal. <https://apps.nationalmap.gov/lidar-explorer/#/>`_ 

#. Use QGIS built-in functions to perform the slope calculations. See QGIS `slope documentation. <https://docs.qgis.org/3.34/en/docs/training_manual/rasters/terrain_analysis.html#follow-along-calculating-the-slope>`_ 

To be continued...
-------------------
Dr. Layer's operation is controlled via menu commands (with associated keyboard accelerators), manipulation tools, scaling buttons, the load tool bar, and time control buttons. The program displays the results of its calculations visually in the form of animated displacements, and also in the form of dynamically generated time history plots. There are also mechanisms for getting numerical values.

.. figure:: ./images/case1.png
    :scale: 30 %
    :align: center
    :figclass: align-center


SimCenter Tool Used
-------------------

blablabla

.. list-table:: Title
   :widths: 25 25 50
   :header-rows: 1

   * - Heading row 1, column 1
     - Heading row 1, column 2
     - Heading row 1, column 3
   * - Row 1, column 1
     -
     - Row 1, column 3
   * - Row 2, column 1
     - Row 2, column 2
     - Row 2, column 3

Time can be controlled using either the keyboard or the time control buttons:

* To run time **forward**: Press and hold the 'g' key or click and hold the time forward button: <insert icon>.

* To reset time to **zero**: Type the '0' key or click on the time reset button: <insert icon>.

* The current analysis time is **displayed** in the feedback pane at the bottom of the screen.

* The analysis time step size can be controlled via the Time Step menu (there are combinations of material properties and time steps that intentionally lead to unstable results, so beware).

* The display time step can be controlled via the Animation Speed menu. Internally, this command controls how many analysis time steps are computed between screen updates.


Example Application
-------------------

Dr. Layer's tool palette is illustrated below (Windows version: the Mac version is similar but grouped a bit differently):

<insert tool palette image>

* The **Arrow Tool** is used to select and manipulate objects.

* The **Panner** and **Camera Orbit Tools** are used to change the viewing point and camera orientation via clicking and dragging.

* The **Plot Box Tool** is used to create one of the various types of plot boxes: 

    * **Displacement Time History plots** are created by clicking on the relevant layer. The top node in the layer is used as the plotting target.

    * **Fast Fourier Transform (FFT) plots** of a displacement history can be created by clicking on the time history plot.

    * **Stress-strain plots** can be created by control-clicking (i.e., holding down the control key while clicking) on the desired layer.


 These controls are self-explanatory in regards to their functions. Note the following, however:

.. note::
    The scaling buttons will continue to scale as long as they are held down. It is not necessary to click multiple times to get this effect.


Remarks
-------

* To adjust the **plotting scales**, use the small expansion/contraction triangular buttons on the plot for the horizontal scale, and the plot scale buttons on the `Scale Button Toolbar <#scaling-buttons>`_ for the vertical scale. 

.. note::
    You will notice that all plots scale together. This is so that plots of a given type can be compared visually without any misleading differences in scale factors.

* To adjust the **horizontal offset** of a plot, click in the plot and drag horizontally to scroll back and forth.

.. note::
    In general, plots will automatically scroll as necessary as time is running. Once you have manually scrolled a plot, though, the automatic scrolling will cease until time is reset to zero.

* Plot boxes can be added or removed at any time, but they only accumulate data beginning from the time they are installed, with the exception of FFT plots, which always plot the according to the data accumulated in the target time history. FFT plots can use up to the first 1024 points in a time history.


.. warning:: 
    Plotting FFT's will slow down the animation speed significantly, especially as the length of the time histories increase.

.. bibliography:: references.bib