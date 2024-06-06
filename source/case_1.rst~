.. _case_1:

quoFEM - Settlements
================================

Author: Kendra Mutch
---------------------

Introduction
------------

The goal of this project is to quantify settlement, parameters impacting settlement, and observe how uncertainty in input parameters impacts the ultimate settlement of a cohesive soil. These calculations will be performed through use of QuoFEM. For more details, the user is encounged to read *Holtz and Kovacs 2011*.

Project Description
-------------------

Soil settlement is characterized by a change in the effective stress of soil, often driven by either a change in the ground water table, placement of fill/surchage load, or dissipation of excess pore water pressure. While a minimal amount of settlement is expected and may not prove hazardous, larger magnitudes of settlment, or differential settlement, can be detrimental to the integretity and functionality of a super-structure. Settlement of cohesive soil is especially hazardous, as the small pore space in fine grained soil restricts water from draining quickly through the voids. As a result, cohesive soil may continue to settle for a long period of time following the placement of a structure. Granular soil exhibits a significantly lower settlement hazard, as water tends to drain rapidly through the large pore space in the soil, meaning, much of the settlement of coarse grained soil is complete before construction begins. This project focuses on the hazard pertaining to the settlement of cohesive soil. 

When computing settlement, it is important to consider uncertainty and not accept a single predicted value as completely true to reality, as in-situ testing, lab testing, and various models used to determine soil paramters all contain uncertainty. Additionally, soil may differ vastly throughout a project site, with only a few samples taken to represent the whole site. This project uses the program QuoFEM to integrate standard settlement equations with uncertainty quantifiction tools.

The example problems in this project will utilize the scenario, soil profile, and paramters depicted below:

**Scenario:**
*A site adjacent to San Francisco Bay is underlain by San Francisco Bay Mud. The site is to be readied for development by placement of 5ft of fill material, and the ultimamte settlement of the fill is of interest. The site conditions, shown below, indicate the presence of a crust of desiccated Bay Mud with thickness, h1, which is not expected to consolidate noticeably. The clay is underlain by a dense gravel, which will also not consolidate.*

.. figure:: ./images/case1_settlementProblem.png
    :scale: 55 %
    :align: center
    :figclass: align-center>

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
     

Solution Strategy
-----------------
The magnitude of settlement may be predicted with the below equations:

#. If soil is normally consolidated, σp' = σo':

    .. math::
        H_{ult} = \frac{C_c}{1+e_o}log(\frac{σ_f'}{σ_o'})H_o
    
    
#. If soil is over consolidated, σp' > σo' and σo' + Δσ' < or = σp':

    .. math::
        H_{ult} = \frac{C_r}{1+e_o}log(\frac{σ_f'}{σ_o'})H_o


#. If soil is over consolidated, σp' > σo' and σo' + Δσ' > σp':

    .. math::
        H_{ult} = \frac{C_r}{1+e_o}log(\frac{σ_f'}{σ_o'})H_o + \frac{C_c}{1+e_o}log(\frac{σ_f'}{σ_o'})H_o

Where:

    .. math::
        H_{ult} = Ultimate Settlement
    
    .. math::
        C_c = Commpression Index
    
    .. math::        
        e_o = Void Ratio
    
    .. math::
        C_r = Recompression Index
        
    .. math::
        σ_f' = Final Vertical Effective Stress
        
    .. math::    
        σ_o' = Initial Vertical Effective Stress
        
    .. math::
        σ_p' = Preconsolidation Pressure
        
    .. math::
        Δσ' = Change in Vertical Effective Stress
        
    .. math::
        H_o = Thickness of Compressible Layer

Though these equations provide a starting point for predicting settlement, they don't capture uncertainty. To account for uncertainty, methods such as Forward Propagation, parameter calibration, and Sensitivity Analysis integrate standard equations with uncertainty quantification. 

Forward Propagation allows us to determine how uncertainty in soil parameters translates to uncertainty in ultimate settlement. This analysis method enables us to understand the effect of compounding uncertainty.

Parameter calibration, allows one to determine an unknown soil paramter, given a value of ultimate settlement. Two examples of parameter calibartion are discussed in the **Example Applications** section. One example utilizes Bayesian Calibration, while another example utilizes Deterministic Calibration.

Finally, Sensitivity Analysis allows us to determine which input parameters impact the resulting ultimate settlement most. Sensitivity Analysis may be performed in both Python and QuoFEM. A Python script performing Sensitivity Analysis may be found here. This script produces a **tornado diagram** (as depicted below), a visual representation of the change in magnitude of settlement resulting from the application of uncertainty to a single variable at a time.

.. figure:: ./images/case1_TornadoDiagram.png
    :scale: 50 %
    :align: center
    :figclass: align-center>

SimCenter Tool Used
-------------------
QouFEM allows the integration of finite element and hazard compuatations with uncertainty quantification tools. There are five different tabs in QuoFEM; four input tabs and one results tab. The four input tabs are outlined below:

    * **UQ tab** - The UQ tab allows one to select the analysis method (Forward Propagation, Bayesian 
      Callibration, Sensitivity Analysis, etc.). Additionally, one can specify a statistics model and the number 
      of samples to run.

    * **FEM tab** - The FEM is where a python script is input, and a finite element method (such as Openseas) may 
      be selected. 

    * **RV tab** - The RV tab allows you define random variables and apply desired uncertainty and statistic distributions 
      (normal distribution, uniform distribution etc.) to each variable.
      
    * **EDP tab** - The EDP tab allows one to define quantities of interest to compute (i.e., ultimate settlement).
    
    .. figure:: ./images/case1_InputResultsTabs.png
        :scale: 100 %
        :align: center
        :figclass: align-center>

After entering parameters in the input tabs, one may choose run the project on their machine by simply clicking **Run** or to run the project in the cloud by selecting **Run at Design Safe**. When choosing to run a project in the cloud, one must login to Design Safe and specify a maximum run time. To ensure that the project does not expire while waiting in the que, select a run time of at least 10 hours.

The results tab contains both a **Summary** page and a **Data Values** page. The **Summary** page contains a brief 
outline of the values computed. The **Data Values** page contains a more comprehensive set of results and figures. There are various features within the **Data Values** page of the **Results** tab which may aid in analysis. Below is information about navigating the **Data Values** page to extract desired information:
    
   * **To View a Scatterplot of a Parameter vs. Run Number** - left click once on any column.
    
   * **To View a Cumulative Frequency Distribution for a Variable** - First left click once on the column for the 
     variable that you want to view a cumulative frequency distribution for. Then right click once on the same 
     column.
     
   * **To View a Histogram for a Variable** - After following the steps to display a cumulative frequency 
     distribution, left click on the same column once more to display the histogram.
     
   * **To View a Scatterplot of One Variable vs. Another Variable** - Right click once on one of the variables. 
     This defines which variable will be on the x-axis. Then, left click once on the variable which you want 
     plotted on the y-axis.
   
   * **To Export the Data Table** - Select the Save Table icon above the data, and choose a location for saving 
     the table as a .csv file.


Example Applications
--------------------

Example One - Forward Propagation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
#. Open QuoFEM. By default, the **UQ method** is **Forward Propagation** and the **UQ Engine** is **Dakota**. In this example, we will use these defaults. Specify a **Sample Number** of 200 and a **Seed Number** of 949. Ensure the **Parellel Execution** and the **Save Working dirs** boxes are checked.

#. Select the **FEM** tab. From the drop down menu, select **Python**. Navigate to the location of the **Input Script** and the **Parameters Script**. Both Python scripts are available at the below links:
    
    * *settlement.py*
    * *params.py*

#. Select the **RV** tab. Enter the random variables (listed in the table in the problem description). Select **Normal Distribution** for each random variable, and enter the mean and standard deviation. The standard deviation must be calculated for each variable from the given coefficient of variation. The below table shows values which should be input for each random variable.

    .. list-table:: Random Variables
       :widths: 25 25 50 50
       :header-rows: 1

       * - Variable Name
         - Distribution
         - Mean Value
         - Standard Deviation
       * - h1
         - Normal Distribution
         - 3
         - 0.15
       * - h2
         - Normal Distribution
         - 25
         - 1.25
       * - Cc
         - Normal Distribution
         - 0.75
         - 0.15
       * - Cr
         - Normal Distribution
         - 0.05
         - 0.01
       * - eo
         - Normal Distribution
         - 1.54
         - 0.1078
       * - Δσ'
         - Normal Distribution
         - 200
         - 100
       * - k
         - Normal Distribution
         - 0.000001
         - 0.000002
       * - unit weight of fill
         - Normal Distribution
         - 130
         - 9.1
       * - height of fill
         - Normal Distribution
         - 5
         - 0.1
#. In the **EDP** tab, specify the variable of interest as **Settlement** and assign it a **Length** of **1**.

#. Run the example either on your machine or in the cloud. For running in the cloud, see the **SimCenter Tool Used** section for additional details.

The results for Forward Propagation are outlined below:

.. figure:: ./images/case1_ForwardPropagationResults.png
    :scale: 100 %
    :align: center
    :figclass: align-center>

Example Two - Parameter Calibration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Deterministic Calibration
^^^^^^^^^^^^^^^^^^^^^^^^^
For this scenario, deterministic calibration can’t find an optimal value, and Bayesian Calibration must be used.

Bayesian Calibration
^^^^^^^^^^^^^^^^^^^^
#. Open QuoFEM. In the **UQ** tab, change the **UQ method** to **Bayesain Callibration** and change the **UQ Engine** to **UCSD-UQ**. For the model, select **Non-hierarchical**. Enter a **Sample** number of 500 and **Seed** number of 85. For the **Calibration Data File**, navigate to **data_2.txt**. This text file may be downloaded at the below link:

    * *data_2.txt*

#. In the **FEM** tab, navigate to the location of the **Input Script** and **Parameter Script**. The Bayesian Calibration Python scripts may be downloaded at the below links:

    * *Settlement_2.py*
    * *params.py*
    
#. In the **RV** tab, enter the same random variables as the Forward Propagation example.

#. In the EDP tab, add two variables of interset. The first variable is **settlement** with a **Length** of **1**, and the second variable is a **dummy** variable with a **Length** of 1.

#. Choose to run the example either on your machine in the cloud. For running in the cloud, see the **SimCenter Tool Used** section for additional details.

The results for Bayesian Calibration are outlined below:

.. figure:: ./images/case1_BayesianResults.png
    :scale: 100 %
    :align: center
    :figclass: align-center>

.. figure:: ./images/case1_BayesianResults2.png
    :scale: 100 %
    :align: center
    :figclass: align-center>

Example Three - Sensitivity Analysis
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. In the UQ tab, select **Sensitivity Analysis** as the **UQ Method**. From the **UQ Engine** drop down, select **SimCenterUQ**. In the Method drop down, select **Monte Carlo**. For the **Number of samples**, enter 500, and for the **Seed Number**, enter 106.

#. Select the **FEM** tab. From the **FEM** drop down, select **Python**. Locate the file path for the **Input Script** and the **Paramters Script**. Both Python scripts are available at the below links.

    * *Input Script.py*
    * *Parameters Script.py*

#. In the **RV** tab, enter the same random variables as the Forward Propagation example.

#. In the **EDP** tab, use the same inputs as the Forward Propagation example.

#. Choose to run the example either on your machine in the cloud. For running in the cloud, see the **SimCenter Tool Used** section for additional details.

The results for the Sensitivity Analysis in QuoFEM are outlined below. Uncertainty in preconsolidation pressure and compression index translate to the greatest uncertainty in the predicted settlement.

.. figure:: ./images/case1_Sensitivity2.png
    :scale: 100 %
    :align: center
    :figclass: align-center>

.. figure:: ./images/case1_Sensitivity.png
    :scale: 100 %
    :align: center
    :figclass: align-center>

Remarks
-------
By accounting for uncertainty in settlement, chances of highly underpredicting or overpredicting settlement are reduced. 


    [Hol2011] R. D. Holtz and W. D. Kovacs. *An Introduction to Geotechnical Engineering*. Pearson, 2011. ISBN 978-0137011322.
