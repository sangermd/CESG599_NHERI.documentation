.. _case_1:

quoFEM - Settlements
================================

Author: Kendra Mutch
---------------------

Introduction
------------

This page describes basic concepts of computing settlement computations in QuoFEM. The calculation methods discussed include forward propagation, deterministic and Bayesian calibration, and sensitivity analysis. For more details, the user is encounged to read :cite:`Holtz and Kovacs 2011'

Project Description
-------------------

The goal of this project is to quantify settlement, parameters impacting settlement, and observe how uncertainty in various input parameters impact the ultimate settlement of a cohesive soil. These copmutations make use of the program QuoFEM. The following report discusses the features of the program, theory regarding the settlement and uncertainty concepts discussed above, as well as three example problems applying different features of QuoFEM.

The first example makes use of the Forward Propagation feature of QuoFEM, which allows one to apply uncertainty to input parameters (such as preconsolidation pressure, compression and recommpression index, void ratio, unit weight, etc.) to determine which paramter(s) impact the ultimate settlement most. In-situ testing, lab testing, and various models used to determine soil paramters may all contain uncertainty. Thus, it is important to consider uncertainty in geotechnical calculations, such as settlement, and not accept a single predicted value without accounting for uncertainty as completely true to reality. The forward propagation analysis will help us translate uncertainty in soil parameters to uncertainty in ultimate settlement, reducing chances of highly underpredicting or overpredicting settlement.

In the second example, Bayesian and Deterministic Calibration are used to optimize the value of an input parameter to yield a desired settlement. In other words, given a specific value of ultimate settlement, we can calculate the value of an unknown input paramter.

The final example applies the Sensitivity Analysis feature of QuoFEM to determine which input parameters impact the resulting ultimate settlement most. As the discussion of results will reveal, settlement shows a strong correlation with some soil parameters, and a weaker correlation with other paramters. By knowing which parameters settlement is most dependent on, one can allocate funds in site investigation or lab testing to use the most accurate methods for predicting such parameters. Or, one may simply be cautious that potential uncertainty in such parameters, especially compounded uncertainty of multiple parameters, could lead to high uncertainty in the predicted settlement, and perhaps a more conservative design should be implemented. 

All examples will use a python input script, paired with the Dakota uncertainty quantification tool in QuoFEM. These aspects of the project are discussed in further detail throughout the report.

The soil profile and problem scenario are the same for all three examples and are depicted in the image and table below.

.. figure:: ./images/ProblemScenarioP1.png

.. list-table:: Soil Profile Parameters
   :widths: 25 25 50
   :header-rows: 1

   * - Parameter
     - Mean Value
     - Coefficient of Variation (%)
   * - h1
     - 3 ft
     - 5
   * - h2
     - 25 ft
     - 5
   * - Cc
     - 0.75
     - 20
   * - eo
     - 1.54
     - 7
   * - Cr
     - 0.05
     - 20
   * - change in pressure
     - 200 psf
     - 50
   * - k
     - 10E-6 (cm/sec)
     - 200
   * - unit weight of fill
     - 130 pcf
     - 7
   * - height of fill
     - 5 ft
     - 2
     

Example One Solution Strategy - Forward Propagation
---------------------------------------------------

#. Open the QuoFEM. By default, the UQ method is Forward Propagation and the UQ Engine is Dakota. In this example, we will use these defaults. Specify a sample number of 200 and a seed number of 949. Ensure the **Parellel Execution** and the **Save Working dirs** boxes are checked.

#. Select the FEM tab. From the drop down menu, select Python. Navigate to the location of the **Input Script** and the **Parameters Script**.

#. Select the RV tab. Enter the random variables (listed in the table in the problem description). Selelct a normal distribution for each random variable, and enter the mean and standard deviation. Remember, the standard deviation must be calculated for each variable from the given coefficient of variation. The below formula may be used to convert coefficient of variation to the standard deviation.
   
#. In the EDP tab, specify the variable of interest as **Settlement** and assign it a **Length** of **1**.

#. Run the example either on your machine or in the cloud. For running in the cloud, see the **SimCenter Tool Used** section for additional details.


Example Two Solution Strategy - Bayesian and Deterministic Calibration
----------------------------------------------------------------------

#. Open QuoFEM. Change the UQ method to Bayesain Callibration and keep the default UQ Engine as Dakota.


Example Three Solution Strategy - Sensitivity Analysis
------------------------------------------------------
#. In the UQ tab, select **Sensitivity Analysis** as the UQ Method. From the UQ Engine drop down, select **SimCenterUQ**. In the Method drop down, select **Monte Carlo**. For the # of samples, enter 500, and for the seed number, enter 106.

#. Select the FEM tab. From the FEM drop down, select **Python**. Locate the file path for the Input Script and the Paramters Script.

#. In the RDV tab, enter the same random variables as the Forward Propagation example.

#. In the EDP tab, use the same inputs as the Forward Propagation example.

#. Choose to run the example either on your machine in the cloud. For running in the cloud, see the **SimCenter Tool Used** section for additional details.

SimCenter Tool Used
-------------------
QouFEM allows the integration of finite element and hazard compuatations with uncertainty quantification tools. There are five different tabs in QuoFEM; four input tabs and one results tab. The four input tabs are outlined below:

    * **UQ tab** - The UQ tab allows one to select the analysis method (Forward Propagation, Bayesian 
      Callibration, Sensitivity Analysis, etc.). Additionally, one can specify a statistics model and the number 
      of samples to run.

    * **FEM tab** - The FEM is where a python script is input, and a finite element method (such as Openseas) may 
      be selected. 

    * **RV tab** - The RV tab allows you define random variables and apply desired uncertainty and statistical 
      models (normal distribution, uniform distribution etc.) to each variable.
      
    * **EDP tab** - The EDP tab allows one to define quantities of interest. For example, the ultimate settlement.

After entering the inputs for your project, you may choose run the project on your machine by simply clicking **Run** or you may run the project in the cloud by selecting **Run at Design Safe**. If you choose to run your project in the cloud, you must login to you Design Safe account and specify a maximum run time. To ensure that your project does not expire while waiting in the que,, select a run time of at least 10 hours.

The results tab contains both a "Summary" page and a "Data Values" page. The "Summary" page contains a brief 
outline of the values computed. The "Data Values" page contains a more comprehensive set of results and figures.


Example Application
-------------------
There are various features within the "Data Values" page of the Results tab which may aid in analysis. Below is information about navigating the "Data Values" page to extract desired information:
    
    * **To View a Scatterplot of a Parameter vs. Run Number** - left click once on any column.
    
   * **To View a Cumulative Frequency Distribution for a Variable** - First left click once on the column for the 
     variable that you want to view a cumulative frequency distribution for. Then right click once on the same 
     column.
     
   * **To View a Histogram for a Variable** - After following the steps to display a cumulative frequency 
     distribution, left click on the same column once more to display the histogram.
     
   * **To View a Scatterplot of One Variable vs. another Variable** - Right click once on one of the variables. 
     This defines which variable will be on the x-axis. Then, left click once on the variable which you want 
     plotted on the y-axis.
   
   * **To Export the Data Table** - Select the Save Table icon above the data, and choose a location for saving 
     the table as a .csv file.


Remarks
-------


.. bibliography:: references.bib