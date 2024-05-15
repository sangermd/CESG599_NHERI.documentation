Transfer Function example
================================

Introduction
***************************************************

Background
***************************************************
A transfer function is somewhat like a filter that is applied to an incoming wave to produce an output signal. It determines how each frequency in the input motion is amplified or suppressed, by the medium of wave travel. Considering a spring-mass system with an excitation motion at input from the foundation connected to the spring and the corresponding response motion of the connected mass in the inertial system. The response motion of the mass will be a composite factor of the elastic and the viscous damping forces which are inherently embedded in the transfer function that determines the output motion we will obtain. In our wave propagation study we also employ transfer functions as a tool to explain the factors that make our input wave motion different from our output wave obtained. Evaluating the transfer function mathematically involves converting our known input motion to a Fourier series. Each term of the Fourier series is multiplied by the transfer function to obtain the Fourier series of the output response. Resonance is a physical phenomenon that occurs when the natural frequency of vibration of particles in a body (in our case the layers) matches the frequency of the forcing function (our input motion). It is experienced as an infinite amplification of the model.

Objective
***************************************************
After this exercise you should be able to understand and obtain different transfer functions using Dr. Layer.

Things to Do
***************************************************
#. Open the Dr. Layer program. By default we get twelve layers. The top six layers are hardwired into the system with a velocity of specified as very fast. The bottom six layers are hardwired with a velocity of very slow.

#. Select all the layers to all have very slow values using the select all option.

#. On the top left hand corner of the menu box choose the plot box tool and apply a plot box at the top of the layers. Do the same at four arbitrary points along the soil layers. Note the height (:math:`H`) you place the plots.

    <insert image>

#. Push the time increment button for about 1 minute.

#. Obtain the angular frequency :math:`(2p/T)`, where :math:`T` is the period i.e. time it takes to complete one revolution.

#. Obtain the maximum displacements from the plots by clicking on the crest of the curves with your cursor.

#. Close all the plot boxes leaving only the one on the top layer.

#. For each combination you try calculate the amplification factor as shown in the calculation aid section below.

#. Increase the frequency of input motion by adjusting the slider on the top of your screen. Obtain the angular frequency as calculated above.

#. For each combination you try calculate the amplification factor as shown in the calculation aid section below.

#. Repeat the process above for different combinations changing layer parameters noting the first peak displacements in each case.

The relationship below for the transfer function is applicable to only standing waves, which are produced by the constructive interference of two waves travelling in opposite directions. The transfer function between two points on the top and bottom of a soil layer is given by

.. math::
    TF = \frac{1}{\cos(\frac{wH}{v_s})}

    AF = \frac{1}{|\cos(\frac{wH}{v_s})|}


Where

:math:`w`` = Angular frequency (2pf)

:math:`H` = distance between any two points in the layers under consideration.

:math:`V` = Velocity of wave travel within the soil layer.

:math:`TF` = Transfer function

:math:`AF` = Amplification function

    <insert image>


Observation
***************************************************
The transfer function is different for each combination of parameters employed and is seen to show a general trend consistent with the damping and stiffness characteristics of the medium of travel.

On Your Own
***************************************************
#. How can you explain what occurs around :math:`kH` = 50 units in Fig 1.2 above?

#. Make a plot of the Amplification factor against the product of the wave number :math:`k` :math:`(w/v)` and height :math:`H` in the first exercise and against the angular frequency :math:`(2p/t)` in the second exercise.

#. Further reading is suggested which shows how the transfer function is derived from the damping and elastic characteristics of a member.

#. What physical phenomenon occurs for an undamped system when the denominator of equation 1.2 approaches 0?
