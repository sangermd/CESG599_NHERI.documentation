.. _case_2:

EEUQ - Transfer Function
========================

Author: Erick Martinez
----------------------

Introduction
------------

This page introduces the fundamental concept of transfer functions and their application in site response analysis. 
The project also examines the impact of uncertainty in input variables during this process, utilizing the SimCenter tool EE-UQ. 
Special attention is given to soil amplification factors. For more detailed information on transfer functions, users are encouraged to read :cite:`Kramer1996`.


.. This page describes the basic concept of transfer functions and their use in a site response analysis. Along with this, the uncertainty in this process will be investigated using EE-UQ, a SimCenter tool. For more details, the user is encouraged to read :cite:`Kramer1996`. 

A Jupyter Notebook for this example can be found within `DesignSafe PRJ-4604 <https://www.designsafe-ci.org/data/browser/projects/3318891375077944850-242ac118-0001-012/>`_.


.. note::
   This example was prepared on a Mac system. Differences in the UI between Mac and other systems are possible, but should not affect the outcome.


Problem Description
-------------------

A transfer function acts as a filter that can amplify or de-amplify an incoming wave from a medium to produce the output signal in another medium. To simplify the idea of a transfer function, a spring-mass system can be used. As a motion is applied on the mass connected to a spring; a responsive outgoing wave will then be propagated through the mass and the spring. This outgoing motion will be a composite factor of the stiffness and elastic damping forces found within the spring-mass system.

.. seealso::
   For more information, visit the `Free Vibrations of a Spring-Mass-Damper System <https://demonstrations.wolfram.com/FreeVibrationsOfASpringMassDamperSystem/>`_. 



This can be applied to earth systems in the form of ground motions. An example of this is an earthquake motion acting on a rock layer at a certain depth. This motion is then transferred through the soil profile and is reflected as a different motion at the surface. In order to determine the influence of a soil profile on the motion, three major components are required: thickness of layer (H), shear wave velocity (Vs), and damping ratio. 

It is important to ensure that this difference in motion is accounted for. When a structure is constructed, it has a specific resonance. Understanding this resonance is important because if an earthquake motion has similar peaks in frequency, causing amplification of the motion, the structure could have a significant and potentially disastrous behavior. While the incoming motion at the rock might have a different natural resonance, the motion at the surface might match that of the structure. The design of earthquake resistant structures relies on the accurate determination of transfer functions in sites. An example of this is shown below - where a motion was amplified from 0.03g to 0.15g due to the presence of soft clay.


.. figure:: ./images/case2_Resonance_Building_Example_TF.png
   :scale: 40%
   :align: center

   Fig. 1. Mexico City Earthquake Amplification.


As with any engineering properties, there will always be the presence of uncertainty. A layer might have differential thicknesses in certain regions, causing the height to be non-uniform. Shear wave velocity can change very quickly depending on depth and composition of the materials within the layer. Damping can also be affected by changes in stratigraphy and composition. To account for this, uncertainty must be incorporated into a transfer function analysis. This inclusion will aid in the accuracy and reliability of site response analyses. 


Solution Strategy
-----------------

Fourier Transform
^^^^^^^^^^^^^^^^^

In earth systems, this relationship between incoming and outgoing wave can be evaluated through mathematically converting an input motion, typically an acceleration-time history, to a Fourier series using the Fast Fourier Transform (FFT). In the Fourier space, the motion is then multiplied by the transfer function, resulting in the outgoing Fourier motion. This can then be converted back into various plots, such as acceleration-time history and spectral acceleration vs. period, that allow for analysis of the outgoing motion. An analysis of this ground motion can provide frequencies of interest where ground accelerations would be highest/lowest, which can aid in site response analysis and planning. 
	
.. figure:: ./images/case2_TF_Rock_to_Soil1.png
   :scale: 40%
   :align: center

   Fig. 2. Transfer Function from Rock to Soil.




Transfer Function Equation
^^^^^^^^^^^^^^^^^^^^^^^^^^

To calculate a transfer function, the equation below can be used. In a single layer soil profile, it is assumed that the impedance contrast between layers is zero. Using a tool like EE-UQ can help provide the ratio between the input and output motion and provides the uncertainty in those motions and variables.

    
.. figure:: ./images/case2_TF_Equation.png
   :scale: 40%
   :align: center

   Eqn. 1. Transfer Function Equation [Kramer, 1996]


.. note::
   This equation changes based on the soil profile. Having multiple soil layers can lead to an impedance contrast. The equation also changes if the damping ratio is assumed to be zero. Kramer (1996) provides more information for the different instances.

Example
^^^^^^^

A typical transfer function would look similar to the one provided below. In the figure below, there are various peaks of natural resonance for the transfer function, which is where the motion will have the greatest amplification/de-amplification.

.. figure:: ./images/case2_TF_Nat_Freqs.png
   :scale: 60%
   :align: center

   Fig. 3. Transfer Function from Rock to Soil.


SimCenter Tool Used
-------------------


To understand transfer functions, there are many tools available. One of these tools is the SimCenter Transfer Function Tool (TFT). This tool introduces users to transfer functions by providing the output motion at a site given the motion, thickness of layers, shear wave velocities, and damping ratio. TFT allows for easy analysis of amplification/de-amplification of ground motions based on specific sites.

The Earthquake Engineering with Uncertainty Quantification Application (EE-UQ) is a SimCenter research application that also allows for site response predictions due to earthquake loading. In addition to basic transfer function quantification, it allows for the analysis of uncertainty in the predictions based on the uncertainty found within the input model, motion, etc. This workflow application allows the user to run analyses in the background and provides a simple user interface that facilitates its use.



Example Application
-------------------

Soil Profile
^^^^^^^^^^^^

In this example, we will analyze the amplification/deamplification effects of a ground motion caused by its propagation through the soil layer. The 10 meter soil layer has a shear wave velocity (Vs) of 500 m/s and a damping ratio of 3%.

	
.. figure:: ./images/case2_CESG599_TF_image1.png
    :scale: 50 %
    :align: center
    
    Fig. 4. Soil Profile & Material Properties.

Uncertainty
^^^^^^^^^^^^

Because of the presence of uncertainty in the soil properties, the transfer function will include uncertainty in its effects. Normal distribution values for each variable (H, Vs, damping) will be provided. This uncertainty will be quantified through multiple runs in EE-UQ and expressed as ratios of mean velocity and acceleration, along with standard deviation and skewness. 

The following normal distribution figures represent the uncertainty within each variable. 



.. figure:: ./images/case2_Combined_RV_1.png
    :scale: 40 %
    :align: center

    Fig. 5. Uncertainty in Each Variable (H, Vs, Damping).

Motion
^^^^^^

An earthquake motion will be applied to a rock, located at the bottom of a one-dimensional soil profile. The motion is shown below as an acceleration time history as well as a Fourier amplitude spectra (FAS).


.. figure:: ./images/case2_Input_Motion_TF.png
    :scale: 40 %
    :align: center

    Fig. 6. Input Ground Motion.

Pre-Workflow Python Script
^^^^^^^^^^^^^^^^^^^^^^^^^^
To complete a transfer function analysis in EE-UQ various Python files had to be generated. The following script calculates the transfer function of a soil layer and applies it to a given acceleration record.

.. raw:: html

    <details>
    <summary><u><b>Click to expand the full Transfer Function Example code</u></b></summary>
    <pre><code>

.. code-block:: python

    # ############################################################################################################
    # Title: Transfer Function Calculation
    # Description: This script calculates the transfer function of a soil layer and applies it to a given acceleration record.
    # Author: Pedro Arduino
    # UW Computational Geotechnical Group
    # Date: 2024
    # All Rights Reserved
    # ############################################################################################################

    # %%
    import numpy as np
    import json
    import matplotlib.pyplot as plt
    from numpy.fft import fft, ifft
    from scipy import integrate
    from respSpectra import resp_spectra

    class TFunctionClass:
        def __init__(self, damping, H, Vs):
            # Define the variables
            self.m_freq = None
            self.m_time = None
            self.m_acc = None
            self.m_absFft = None
            self.m_absSoilTF = None
            self.m_absIFft = None
            self.m_accT = None

            self.m_vel = None
            self.m_disp = None
            self.m_velT = None
            self.m_dispT = None

            # Define soil layer parameters
            self.m_damping = damping / 100.0 # damping from percentage to number
            self.m_H = H
            self.m_Vs = Vs

        
        def calculateResponse(self):
            SoilTF = np.empty_like(self.m_freq, dtype=np.complex_)
            absSoilTF = np.empty_like(self.m_freq, dtype=np.float_)
            
            # Compute the Fourier amplitude spectrum
            fas = fft(self.m_acc)
            # fas = fas[:self.nyquist_index]
            absfas = np.abs(fas)
            self.m_absFft = absfas
            
            # Compute transfer function of soil layer
            SoilTF = self.calcSoilTf()
            self.m_absSoilTF = np.abs(SoilTF)
            
            # Compute surface soil response
            ifas = fas * SoilTF
            absfas2 = np.abs(ifas)
            self.m_absIFft = absfas2
            accT = ifft(ifas)
            self.m_accT = accT.real  # Take only the real part


        def calcSoilTf(self):

            tf = []

            if self.m_freq is None:
                print("Frequency vector is not defined")    
            else:

                for f in self.m_freq:
                    """
                    * The uniform damped soil on rigid rock transfer function
                    *                             1
                    *  H = -------------------------------------------------
                    *       cos ( 2* PI * freq * H / (Vs(1+ i*damping))
                    """
                    kstar = 2.0 * np.pi * f / self.m_Vs - self.m_damping * 2.0 * np.pi * f / self.m_Vs * 1j
                    Vsstar = self.m_Vs + self.m_damping * self.m_Vs * 1j
                    tf.append(1.0 / np.cos(2.0 * np.pi * f * self.m_H / Vsstar))

            return tf

        def calculate_nat_freq(self):
            n_pt = len(self.m_freq)
            N_freq = []
            N_freqVal = []
            dfreq = self.m_freq[-1] / n_pt

            TF_tan = 1.0
            for i in range(1, len(self.m_freq)):
                TF_tan1 = (self.m_absSoilTF[i] - self.m_absSoilTF[i - 1]) / dfreq
                if TF_tan1 * TF_tan <= 0 and TF_tan > 0:
                    N_freq.append(self.m_freq[i])
                    N_freqVal.append(self.m_absSoilTF[i])
                TF_tan = TF_tan1
        
            return N_freq, N_freqVal

        def calculate_ratio(self):

            grav = 9.81 # m/s2
            dT = self.m_time[1] - self.m_time[0]
            accAux = [self.m_acc[ii]*grav for ii in range(len(self.m_acc))]
            self.m_vel = integrate.cumtrapz(accAux, dx=dT)
            # self.mvel = np.insert(self.m_vel, 0, 0.0)
            self.m_disp = integrate.cumtrapz(self.m_vel, dx=dT)
            # mdisp = np.insert(self.m_disp, 0, 0.0)

            self.m_velT = integrate.cumtrapz((self.m_accT*grav), dx=dT)
            # self.mvel = np.insert(self.m_vel, 0, 0.0)
            self.m_dispT = integrate.cumtrapz(self.m_velT, dx=dT)
            # mdisp = np.insert(self.m_disp, 0, 0.0)

            ratioA = abs(max(self.m_accT))/abs(max(self.m_acc))
            ratioV = abs(max(self.m_velT))/abs(max(self.m_vel))

            return ratioA, ratioV

        def sin_record(self, f):
            n_points = 2000
            self.m_dt = 0.02
            self.m_acc = [0] * n_points
            accel = []

            for s in range(n_points):
                accel.append(0.4 * np.sin(2 * f * np.pi * s * self.m_dt))

            self.m_acc = accel
            self.set_time()
            self.set_freq()

        def sweep_record(self):
            n_points = 8000
            self.m_dt = 0.002
            self.m_acc = [0] * n_points
            self.m_time = [0] * n_points

            for i in range(len(self.m_time)):
                time = i * self.m_dt
                self.m_time[i] = time
                self.m_acc[i] = np.sin(25.0 * time + 150.0 * (time * time / 2.0) / 16.0)

            self.set_freq()


        def load_file(self, file_name):
            
            self.m_filename = file_name
            
            try:
                with open(file_name, 'r') as file:
                    # Read file contents into a JSON object
                    jsonObj = json.load(file)
            except FileNotFoundError as e:
                print(f"Cannot read file {file_name}: {e}")
                return

            events = jsonObj.get("Events", [])

            if events:
                patterns = events[0].get("pattern", [])
                timeseries = events[0].get("timeSeries", [])
                pattern_type = patterns[0].get("type", "")
                tsname = patterns[0].get("timeSeries", "")

                units = events[0].get("units", {})
                acc_unit = 1.0
                acc_type = units.get("acc", "")
                if acc_type == "g":
                    acc_unit = 1.0
                elif acc_type == "m/s2":
                    acc_unit = 1.0 / 9.81
                elif acc_type in ["cm/s2", "gal", "Gal"]:
                    acc_unit = 1.0 / 981.0

                timeseries_data = timeseries[0].get("data", [])
                dT = timeseries[0].get("dT", 0.0)
                self.read_GM(timeseries_data, dT, acc_unit)
                

        def read_GM(self, acc_TH, dT, acc_unit):
            n_points = len(acc_TH)
            self.m_dt = dT
            # self.m_acc = [acc_TH[ii].toDouble() * acc_unit for ii in range(n_points)]
            self.m_acc = [acc_TH[ii] * acc_unit for ii in range(n_points)]

            if n_points % 2 == 0:
                self.m_acc.append(0.0)
            self.m_acc = np.array(self.m_acc) # Convert to numpy array

            self.set_time()
            self.set_freq()        


        def set_freq(self):

            if self.m_dt == 0:
                self.m_dt = 0.005
                nfreq = 1 / self.m_dt*10
                sample_freq = 1.0 / self.m_dt

            else:
                nfreq = len(self.m_acc)
                sample_freq = 1.0 / self.m_dt

            # self.m_freq = [0] * (len(self.m_acc) // 2 + 1)
            # self.m_freq = [0] * (len(self.m_acc))   # m_freq as a list
            self.m_freq = np.zeros(nfreq) # m_freq as a numpy array
            sample_freq = 1.0 / self.m_dt

            self.nyquist_freq = sample_freq / 2.0
            self.nyquist_index = int(len(self.m_freq) / 2)
            for i in range(len(self.m_freq)):
                self.m_freq[i] = i * sample_freq / len(self.m_acc)


        def set_time(self):
            # self.m_time = [0] * len(self.m_acc) # m_time as a list
            self.m_time = np.zeros(len(self.m_acc)) # m_time as a numpy array

            for i in range(len(self.m_time)):
                self.m_time[i] = i * self.m_dt


        def plot_acc(self):
            plt.figure()
            plt.plot(self.m_time, self.m_acc, 'b-', label='input')
            plt.plot(self.m_time, self.m_accT, 'r-', label='output')
            plt.xlabel('Time [sec]')
            plt.ylabel('Acc [g]')
            plt.legend()
            plt.show()

        def plot_fft(self):
            plt.figure()
            plt.plot(self.m_freq[:self.nyquist_index], self.m_absFft[:self.nyquist_index], 'b-', label='input')
            plt.plot(self.m_freq[:self.nyquist_index], self.m_absIFft[:self.nyquist_index], 'r-', label='output')
            plt.xlabel('Frequency [Hz]')
            plt.ylabel('Fourier Amplitude')
            plt.legend()
            plt.show()

        def plot_tf(self):
            plt.figure()
            plt.plot(self.m_freq[:self.nyquist_index], self.m_absSoilTF[:self.nyquist_index], 'b-')
            plt.xlabel('Frequency [Hz]')
            plt.ylabel('TF')
            plt.show()
            
        def plot_spectra(self):
            n_points = len(self.m_acc)
            accAux = [self.m_acc[ii]*9.81 for ii in range(n_points)]
            accTAux = [self.m_accT[ii]*9.81 for ii in range(n_points)]
            periods, psa = resp_spectra(self.m_time, accAux, 0.05)
            periodsT, psaT = resp_spectra(self.m_time, accTAux, 0.05)
            
            plt.figure()
            plt.plot(periods, psa, 'b-', label='input')
            plt.plot(periodsT, psaT, 'r-', label='output')
            plt.xlabel('Periods [s]')
            plt.ylabel('PSA [cm/s2]')
            plt.legend()
            plt.show()

    def main():
        # Define input parameters
        damping = 5.0  # damping ratio in %
        H = 20.0  # layer height in m
        Vs = 200.0  # shear wave velocity in m/s
        
        TF = TFunctionClass(damping, H, Vs)
        
        # Sinusoidal record
        f = 0.5  # frequency in Hz
        TF.sin_record(f)
        
        # Calculate response
        TF.calculateResponse()
        
        # Calculate ratios
        ratioA, ratioV = TF.calculate_ratio()
        print(f"Acceleration Ratio: {ratioA}")
        print(f"Velocity Ratio: {ratioV}")
        
        # Plot acceleration
        TF.plot_acc()
        
        # Plot Fourier Transform
        TF.plot_fft()
        
        # Plot Transfer Function
        TF.plot_tf()
        
        # Plot Spectra
        TF.plot_spectra()

    if __name__ == "__main__":
        main()

.. raw:: html

    </code></pre>
    </details>


.. raw:: html

    <br><br>


This script performs post-processing by building response spectra from acceleration time history.

.. raw:: html

    <details>
    <summary><u><b>Click to expand the full Response Spectra Python code</u></b></summary>
    <pre><code>

.. code-block:: python

    #########################################################
    #
    # Postprocessing python script
    #
    # Copyright: UW Computational Mechanics Group
    #            Pedro Arduino
    #
    # Participants: Alborz Ghofrani
    #               Long Chen
    #
    #-------------------------------------------------------

    import numpy as np


    def resp_spectra(a, time, nstep):
        """
        This function builds response spectra from acceleration time history,
        a should be a numpy array,T and nStep should be integers.
        """
        
        # add initial zero value to acceleration and change units
        a = np.insert(a, 0, 0)
        # number of periods at which spectral values are to be computed
        nperiod = 100
        # define range of considered periods by power of 10
        minpower = -3.0
        maxpower = 1.0
        # create vector of considered periods
        p = np.logspace(minpower, maxpower, nperiod)
        # incremental circular frequency
        dw = 2.0 * np.pi / time
        # vector of circular freq
        w = np.arange(0, (nstep+1)*dw, dw)
        # fast fourier Horm of acceleration
        afft = np.fft.fft(a)
        # arbitrary stiffness value
        k = 1000.0
        # damping ratio
        damp = 0.05
        umax = np.zeros(nperiod)
        vmax = np.zeros(nperiod)
        amax = np.zeros(nperiod)
        # loop to compute spectral values at each period
        for j in range(0, nperiod):
            # compute mass and dashpot coeff to produce desired periods
            m = ((p[j]/(2*np.pi))**2)*k
            c = 2*damp*(k*m)**0.5
            h = np.zeros(nstep+2, dtype=complex)
            # compute transfer function 
            for l in range(0, int(nstep/2+1)):
                h[l] = 1./(-m*w[l]*w[l] + 1j*c*w[l] + k)
                # mirror image of Her function
                h[nstep+1-l] = np.conj(h[l])
            
            # compute displacement in frequency domain using Her function
            qfft = -m*afft
            u = np.zeros(nstep+1, dtype=complex)
            for l in range(0, nstep+1):
                u[l] = h[l]*qfft[l]
            
            # compute displacement in time domain (ignore imaginary part)
            utime = np.real(np.fft.ifft(u))
            
            # spectral displacement, velocity, and acceleration
            umax[j] = np.max(np.abs(utime))
            vmax[j] = (2*np.pi/p[j])*umax[j]
            amax[j] = (2*np.pi/p[j])*vmax[j]
        
        return p, umax, vmax, amax

.. raw:: html

    </code></pre>
    </details>

.. raw:: html

    <br><br>

Workflow in EE-UQ
^^^^^^^^^^^^^^^^^

The procedure for performing a transfer function analysis is shown below. 

A forward propagation problem will be performed. The UQ engine to be used is Dakota with parallel execution and saved working directories. The Latin Hypercube Sampling (LHS) method will be used with 10 samples and a seed of 913. The UQ tab should look similar to the one below.


.. figure:: ./images/case2_UQTab_Workflow_TF.png
    :scale: 30 %
    :align: center

    Fig. 7. Uncertainty Quantification.

The General Information (GI) tab will not be utilized in this example since no structure will be used.

For the simulation (SIM tab), the input script will be loaded using a CustomPy Model. Along with this, the number of response nodes will be 1 with a spatial dimension of 2. Each node will have 3 degrees of freedom (DOF) and the profile will have damping ratio of 2%. The centroid node value will be 1.


.. figure:: ./images/case2_SimTab_TF.png
    :scale: 30 %
    :align: center

    Fig. 8. Simulations.

In the Event (EVT) tab, a Multiple SimCenter load generator will be used. The motion of interest will be uploaded here as a JSON file and will have a factor of 1.

In the Finite Element Modeling (FEM) tab, select a CustomPy-Simulation.

In the Engineering Demand Parameter (EDP) tab, select a user defined generator. The response parameters will be the ratio of acceleration spectra and velocity spectra from the propagation from rock to the soil.


.. figure:: ./images/case2_EDPTab_Workflow_TF.png
    :scale: 30 %
    :align: center

    Fig. 9. Engineering Demand Parameters. 


The Random Variables (RV) tab is where the values of H, Vs, and damping are implemented in the analysis. The values seen above are to be input here. A normal distribution will be used for all of these variables.


.. figure:: ./images/case2_RVTab_Workflow_TF.png
    :scale: 30 %
    :align: center

    Fig. 10. Random Variables.


The user can opt for running the analysis on their local device or in DesignSafe. 


Results
^^^^^^^
When the run is completed, the mean values of ratioA and ratioV, as well as uncertainty values,should be provided. These values show the ratio of average amplification/de-amplification in acceleration in velocity of the ground motion at the rock and the motion at the surface. The positive value of the ratio shows amplification occurred due to the propagation of the motion through the soil layer. 


.. figure:: ./images/case2_Results_Workflow_TF.png
    :scale: 30 %
    :align: center

    Fig. 11. Results


Because the input variables (H, Vs, damping, motions) each have uncertainty, that uncertainty is carried on to the transfer function analysis. EE-UQ allows for uncertainty quantification which allows for an analysis of which variables might be most important or what the "worst-case scenario" could be when designing. The normalized normal distribution for the acceleration and velocity amplification ratios are shown below.


.. figure:: ./images/case2_Normalized_RatioA_histogram.png
    :scale: 60 %
    :align: center

    Fig. 12. Normalized Acceleration Amplification Factor Histogram

.. figure:: ./images/case2_Normalized_RatioV_histogram.png
    :scale: 60 %
    :align: center

    Fig. 13. Normalized Velocity Amplification Factor Histogram


Due to the infinite possibilities of variability the three main variables (H, Vs, Damping) can have, we see that the normal distribution is not well suited for this analysis, specifically. EE-UQ allows for other methods of uncertainty quantification. Below is a Gaussian Mixture Model. This method is effective in measuring the probability of certain subpopulations within a larger population.


.. figure:: ./images/case2_Gaussian_Mixture_RatioA_histogram.png
    :scale: 60 %
    :align: center

    Fig. 14. Gaussian Mixture Model - Acceleration Amplification Ratio.


.. figure:: ./images/case2_Gaussian_Mixture_RatioV_histogram.png
    :scale: 60 %
    :align: center

    Fig. 15. Gaussian Mixture Model - Velocity Amplification Ratio.

.. note::
   This situation is specific only to this example; normal distributions could very well suit another example.



By extrapolating the values from EE-UQ, the shape of the transfer function can be determined. The natural frequencies of the first 4 peaks in the transfer function are also shown below. 


.. figure:: ./images/case2_TF_Nat_Freqs.png
    :scale: 70 %
    :align: center

    Fig. 16. Transfer Function.




.. raw:: html

   <div style="display: flex; justify-content: center;">

.. table:: Table 1. Natural Frequencies in the Transfer Function
    :widths: auto

    +------------+---------------------------------------------+
    | Peak       | Amplification Factor at Natural Frequencies |
    +============+=============================================+
    | 1          | 20.49                                       |
    +------------+---------------------------------------------+
    | 2          | 7.03                                        |
    +------------+---------------------------------------------+
    | 3          | 4.20                                        |
    +------------+---------------------------------------------+
    | 4          | 2.98                                        |
    +------------+---------------------------------------------+

.. raw:: html

   </div>



With the transfer function plotted, the input motion can be transformed using the transfer function to reflect the motion at the surface. The figure below reflects the large amplification that occurred. The value of the highest acceleration increased from ~0.4g in the rock to ~1.25g in the soil. This amplification is also reflected in other frequencies.  


.. figure:: ./images/case2_Full_Results_TF.png
    :scale: 40 %
    :align: center

    Fig. 17. Amplification of Ground Motion.

The spectral acceleration spectra can be also determined for each of the motions. These spectra can be used to determine if a structure will be affected by the amplification. A single story structure (~0.1 second period) might be at risk due to this amplification. Any periods with a large amplification ratios should be further analyzed to ensure the safety of the structure and site.


.. figure:: ./images/case2_SpectralAcc_Results_TF.png
    :scale: 40 %
    :align: center

    Fig. 18. Amplification in Spectral Acceleration.


Remarks
-------
I'd like to thank everyone at SimCenter, specifically Sang-ri Yi, Frank McKenna, Jinyan Zhao, Aakash Bangalore Satish, and Barbaros Cetiner, for all of their effort and assistance they provided during the entire quarter. Navigating these tools and creating examples for them would've been a lot more stressful without their help. 

Transfer function is one of my favorite topics in geotechnical engineering. I'd really like to continue working with site response and performance based design so being able to create this example along with my class was great.

Finally, I'd like to thank Prof. Arduino who made all of this possible. His determination and motivation was contagious throughout the academic quarter. There are many great professors but there is only one Pedro Arduino. 
