.. _sqspi_timing:

Timing
######

When working with sQSPI, you should be aware of the associated timing parameters. 

.. contents::
   :local:
   :depth: 2

Timing parameters
*****************

.. tabs::

   .. tab:: **nRF54L15**

    .. list-table::
      :widths: 20 20 10 10 10 10
      :header-rows: 1

      * - Symbol
        - Description
        - Min
        - Typ
        - Max
        - Units
      * - F\ :sub:`sQSPI,SCLK`
        - SCLK frequency
        -
        -
        - 64
        - MHz
      * - DC\ :sub:`sQSPI,SCLK`
        - SCLK duty cycle
        - 
        - 50
        -
        - %
      * - t\ :sub:`DOV`
        - Data out valid
        -
        -
        - 2.2
        - ns
      * - t\ :sub:`SDI`
        - Data in setup time
        -
        -
        - 11.7
        - ns

   .. tab:: **nRF54H20**

    .. list-table::
      :widths: 20 20 10 10 10 10
      :header-rows: 1

      * - Symbol
        - Description
        - Min
        - Typ
        - Max
        - Units
      * - F\ :sub:`sQSPI,SCLK`
        - SCLK frequency
        -
        -
        - 80
        - MHz
      * - DC\ :sub:`sQSPI,SCLK`
        - SCLK duty cycle
        -
        - 50
        -
        - %
      * - t\ :sub:`DOV`
        - Data out valid
        -
        -
        -
        - ns
      * - t\ :sub:`SDI`
        - Data in setup time
        -
        -
        -
        - ns

.. image:: images/sqspi_timing.png
  :alt: sQSPI timing diagram 1

.. image:: images/sqspi_timing_1.png
  :alt: sQSPI timing diagram 2
