---
title: 'LazerCat: Play with your cat remotely!'
author: '**Andrew Kim and Rocco Scinto**'
date: '*EEC172 SQ24*'

subtitle: '<blockquote><b>EEC172 Final Project Webpage Example</b><br/>
Note to current students: this is an <i>example</i> webpage and
may not fulfill all stated requirements of the current quarter''s 
assignment.<br/>The website source is hosted 
<a href="https://github.com/ucd-eec172/project-website-example">on github</a>.
</blockquote>'

# Table of Contents

## <h2>Description</h2>

Hydroponics is a technique where p
Our source code can be found [here](https://github.com/kim15096/eec172-finalproject-lazercat).

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;padding-top:20px">
  <div style="display: inline-block; vertical-align: bottom;">
    <img src="./media/home_station.jpeg" style="width:auto;height:2in"/>
    <!-- <span class="caption"> </span> -->
  </div>
  <div style="display: inline-block; vertical-align: bottom;">
    <img src="./media/Image_002.jpg" style="width:auto;height:2in" />
    <!-- <span class="caption"> </span> -->
  </div>
</div>

## Video Demo

<div style="text-align:center;margin:auto;max-width:560px">
  <div style="padding-bottom:56.25%;position:relative;height:0;">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/yG8Bkn4AS34?si=KMJsmiFA9RZ1XC99" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  </div>
</div>

# Market Survey

There are two types of similar product on the market. The first one is

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;">
  <div style='display: inline-block; vertical-align: top;'>
    <img src="./media/Image_003.jpg" style="width:auto;height:200"/>
    <span class="caption">
      <a href="https://aerogarden.com/gardens/harvest-family/Harvest-2.0.html">AeroGarden Harvest 2.0</a>
      <ul style="text-align:left;">
      <li>Inexpensive ($90)</li>
      <li>Not Automated</li>
      <li>Small Scale</li>
      <li>No remote monitoring</li>
    </ul>
    </span>
  </div>
  <div style='display: inline-block; vertical-align: top;'>
    <img src="./media/Image_004.jpg" style="width:auto;height:200" />
    <span class="caption">
      <a href="https://www.hydroexperts.com.au/Autogrow-MultiGrow-Controller-All-In-One-Controller-8-Growing-Zones">Autogrow Multigrow</a>
      <ul style="text-align:left;">
      <li>Expensive ($4500)</li>
      <li>Fully Automated</li>
      <li>Huge Scale</li>
      <li>Cloud monitoring</li>
    </ul>
    </span>
  </div>
</div>

# Design

## System Architecture

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;">
  <div style="display:inline-block;vertical-align:top;flex:1 0 400px;">
    As shown in the system flowchart, we use one of the CC3200 boards for
    the closed feedback loop for maintaining concentration in the
    TDS reading and user-defined threshold to the user on an OLED through
    next synchronization.
  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 400px;">
    <div class="fig">
      <img src="./media/Image_005.jpg" style="width:90%;height:auto;" />
      <span class="caption">System Flowchart</span>
    </div>
  </div>
</div>

## Functional Specification

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;">
  <div style="display:inline-block;vertical-align:top;flex:1 0 300px;">
    Our system works based on the following state diagram. The device will
    thresholds and go back to the rest state. In each state, the device will
    periodically post the TDS.
  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 500px">
    <div class="fig">
      <img src="./media/Image_006.jpg" style="width:90%;height:auto;" />
      <span class="caption">State Diagram</span>
    </div>
  </div>
</div>

# Implementation

### CC3200-LAUNCHXL Evaluation Board

All control and logic was handled by two CC3200 microcontroller units,


## Functional Blocks: Master

### AWS IoT Core

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 200px'>
    The AWS IoT core allows our devices to communicate with each other
    asynchronously. The master device can update the desired thresholds, and
    the slave device will read them and synchronize them to the reported
    state. The slave device will also post the TDS and temperature readings
    periodically.
  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <div class="fig">
      <img src="./media/Image_007.jpg" style="width:auto;height:2.5in" />
      <span class="caption">Device Shadow JSON</span>
    </div>
  </div>
</div>

### OLED Display

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>
    On both Master and Slave devices, the user can view the current TDS and
    temperature of the plant solution on an OLED display. The user can also
    use the display to view and edit the TDS thresholds. The CC3200 uses the
    SPI bus to communicate with the display module.
  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <div class="fig">
      <img src="./media/Image_008.jpg" style="width:auto;height:2in" />
      <span class="caption">OLED Wiring Diagram</span>
    </div>
  </div>
</div>

### IR Receiver

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>
    On both 
  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <div class="fig">
      <img src="./media/Image_009.jpg" style="width:auto;height:2in" />
      <span class="caption">IR Receiver Wiring Diagram</span>
    </div>
  </div>
</div>

## Functional Blocks: Slave

The slave device contains all the functional blocks from the master
device, plus the following:

### Analog-To-Digital Converter (ADC) Board

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>
    The 
    The <a href="https://cdn-shop.adafruit.com/datasheets/ads1015.pdf">
    product datasheet</a> contains the necessary configuration values
    and register addresses for operation.
  </div>
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>
    <div class="fig">
      <img src="./media/Image_010.jpg" style="width:auto;height:2in" />
      <span class="caption">ADC Wiring Diagram</span>
    </div>
  </div>
</div>

### Thermistor

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 300px;'>
    Conductivity-based
  </div>
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>
    <div class="fig">
      <img src="./media/Image_011.jpg" style="width:auto;height:2in" />
      <span class="caption">Thermistor Circuit Diagram</span>
    </div>
  </div>
</div>

### TDS Sensor Board

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 500px'>
    In our first attempt to measure TDS, we used a simple two-probe analog
    setup with a voltage divider. We soon found out that this was a naïve

  </div>
  <div style='display: inline-block; vertical-align: top;flex:1 0 600px'>
    <div class="fig">
      <img src="./media/Image_012.jpg" style="width:auto;height:2in;padding-top:30px" />
      <span class="caption">TDS Sensor Wiring Diagram</span>
    </div>
  </div>
</div>

### Pumps and Control Circuit

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 500px'>
    The CC3200 is unable to provide sufficient power to drive the pumps,

    to flow in the motor. For each motor, there is a reverse-biased diode
    connected across it. This protects our circuit from current generated by
    the motor if it is spun from external force or inertia.
  </div>
  <div style='display: inline-block; vertical-align: top;flex:1 0 600px'>
    <div class="fig">
      <img src="./media/Image_013.jpg" style="width:auto;height:2in;padding-top:30px" />
      <span class="caption">Pump Circuit Diagrams</span>
    </div>
  </div>
</div>

# Challenges

The most significant challenge we faced while developing this prototype
was of inaccurate and inconsistent Electrical Conductivity (EC)
measurements. This occurred due to two reasons: probe channel
polarization and current limitations of GPIO pins.

## Probe Channel Polarization

Our first design for the probe was simply two copper rods, which would

time goes on.

## Current Limitation of GPIO Pins

Our next idea was to try using the GPIO pins to power the probe, since

two-probe implementation was not feasible.

## Solution to Challenges


the results were accurate within 3%, and would not drift by more than
0.5% over time.

# Future Work

Given more time, we had the idea of developing a web app to allow users
to control the device from their cell phone. Another idea we wanted to



# Finalized BOM

