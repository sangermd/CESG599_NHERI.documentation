.. _case_1:

quoFEM - Settlements
================================

Author: Kendra Mutch
---------------------

Introduction
------------

This page describes basic concepts of forward propagation and Bayesian calibration using QuoFEM. For more details, the user is encounged to read :cite:`Kramer1996`.

Project Description
-------------------

QuoFEM allows you to incorporate uncertainty and parameter callibration with finite element and hazard analysis. This project includes two examples, demonstrating how QuoFEM may be utilizied for settlement calculations. The first example makes use of the Forward Propagation feature of QuoFEM, which allows one to apply uncertainty to input parameters (such as preconsolidation pressure, compresison and recommpression index, void ratio, unit weight, etc.) to determine which paramter(s) impact the ultimate settlement most. In the second example, Bayesian Callibration is used to optimize the value of an input parameter to yield a desired settlement. Both examples will use a python input script paired with the Dakota uncertainty quantification tool in QuoFEM.

The soil profile and problem scenario is shown below.

.. figure:: ./images/Problem Scenario P1.png
.. figure:: ./images/Problem Scenario P2.png

Program Overview
----------------
There are five different tabs in QuoFEM; four input tabs and one results tab. The four input tabs are outlined below:

	UQ tab - The UQ tab allows one to select the analysis method (Forward Propagation, Bayesian Callibration, etc.). Additionally, one can     specify a statistics model and the number of samples to run.
	FEM tab - The FEM is where a python script is inputed, and a finite element method (such as Openseas) may be selected. 
	RV tab - The RV tab allows you define random variables and apply desired uncertainty and statistical models (normal distribution,           uniform distribution etc.) to each variable.
    EDP tab - The EDP tab allows one to define quantities of interest. In these examples, the quantity of interest is the settlement being     calculated.

Example One Solution Strategy - Forward Propagation
---------------------------------------------------

#. Open the QuoFEM. By default, the UQ method is Forward Propagation and the UQ Engine is Dakota. In this example, we will use these defaults. Specify a sample and seed number as shown below.

.. figure:: ./images/Forward Propagation UQ Tab.png

end of my edits to date


#. Select all the layers to all have very slow values using the select all option.

#. On the top left hand corner of the menu box choose the plot box tool and apply a plot box at the top of the layers. Do the same at four arbitrary points along the soil layers. Note the height (:math:`H`) you place the plots.

    <insert image>

#. Push the time increment button for about 1 minute.

#. Obtain the angular frequency :math:`(2p/T)`, where :math:`T` is the period i.e. time it takes to complete one revolution.

#. Obtain the maximum displacements from the plots by clicking on the crest of the curves with your cursor.

.. math::
    TF = \frac{1}{\cos(\frac{wH}{v_s})}

    AF = \frac{1}{|\cos(\frac{wH}{v_s})|}


Where

:math:`w` = Angular frequency (2pf)

:math:`H` = distance between any two points in the layers under consideration.

:math:`V` = Velocity of wave travel within the soil layer.

:math:`TF` = Transfer function

:math:`AF` = Amplification function


Dr. Layer's operation is controlled via menu commands (with associated keyboard accelerators), manipulation tools, scaling buttons, the load tool bar, and time control buttons. The program displays the results of its calculations visually in the form of animated displacements, and also in the form of dynamically generated time history plots. There are also mechanisms for getting numerical values.

.. figure:: ./images/case1.png
    :scale: 30 %
    :align: center
    :figclass: align-center

Example Two Solution Strategy - Bayesian Callibration
-----------------------------------------------------

#. Open the QuoFEM. Change the UQ method to Bayesain Callibration and keep the default UQ Engine as Dakota.


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