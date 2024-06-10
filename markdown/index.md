---
title: 'LazerCat: Play with your cat remotely!'
author: '**By: Andrew Kim and Rocco Scinto**'
date: '*EEC172 SQ24*'
toc-title: 'Table of Contents'
abstract-title: '<h2>Description</h2>'
abstract: 'LazerCat is an innovative toy designed for cat owners, enabling them to interact with their pets remotely through a controlled laser pointer and live video feed. Utilizing AWS WebRTC and IoT for seamless cloud integration, the system consists of three main components: a home-based CC3200 module, an away CC3200 module, and a Raspberry Pi. The home module manages local motion detection and AWS-triggered laser control, while the away module allows users to send commands via an IR remote, displaying these commands on an OLED screen and forwarding them to AWS. The Raspberry Pi handles video streaming, activating upon motion detection or user request to provide live footage of the cat activities. Implementation goals range from basic laser activation and motion sensor functionality to advanced features like pre-programmed laser routines, sound playback, and real-time video streaming. The project aims to keep cats entertained and active, providing peace of mind to owners.
<br/><br/>
Our source code can be found 
<!-- replace this link -->
<a href="https://github.com/kim15096/eec172-finalproject-lazercat">
  here</a>.

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;padding-top:20px">
  <div style="display: inline-block; vertical-align: bottom;">
    <img src="./media/home_station.jpeg" style="width:auto;height:3in"/>
    <span class="caption">Home Station</span>
  </div>
  <div style="display: inline-block; vertical-align: bottom;">
    <img src="./media/away_station.jpg" style="width:auto;height:3in" />
    <span class="caption">Away Device</span>
  </div>
  <div style="display: inline-block; vertical-align: bottom;">
    <img src="./media/website.jpg" style="width:auto;height:3in" />
    <span class="caption">Website</span>
  </div>
</div>

<h2 style="margin-top: 30px;">Video Demo</h2>
<div style="text-align:center;margin:auto;max-width:100%;margin-bottom: 30px;">
  <div style="padding-bottom:56.25%;position:relative;height:0;">
    <iframe width="100%" max-width:"1080px" height="512px" src="https://www.youtube.com/embed/yG8Bkn4AS34?si=wD6o4UksnduuIuFD" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  </div>
</div>
'
--- 

# Market Survey

While there are existing products such as pet cameras and laser toys, our system offers distinct advantages and unique features. Unlike typical pet lasers, our system allows for manual control of the laser using physical remote controls. Users can navigate the laser manually using arrow keys or select from preset patterns such as Box, Zigzag, and Line modes with the 1, 2, and 3 keys. Additionally, our system boasts a cute and fun interactive GUI display on the remote device, enhancing user engagement and providing a more enjoyable experience. These features set our product apart in the market, offering greater interactivity and customization compared to conventional alternatives.

# Design
## System Architecture

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;flex-direction:column;">
  <div style="display:inline-block;vertical-align:top;flex:0 0 400px;">
    <div class="fig">
      <img src="./media/architecture.jpg" style="width:90%;height:auto;" />
      <span class="caption">System architecture featuring 3 core parts</span>
    </div>
  </div>
    <div style="display:inline-block;vertical-align:top;flex:1 0 400px; margin-top: 40px;">
    This project is comprised of three core systems, each playing a crucial role in the overall functionality and integration of the system. These systems include the Away Device, the Central System, and the Home Station. Each system incorporates distinct components and functionalities, which collectively ensure seamless operation and user interaction.

### Away Device


The Away Device is designed to be with the user at all times. It comprises the CC3200 microcontroller and a remote control. The CC3200, a powerful and versatile microcontroller, facilitates wireless communication and control. The remote control provides the user with the capability to interact with and manage the system from a distance, ensuring convenience and flexibility in operation. The user can also select and cancel lazer modes through the deployed front-end website.

### Central System

The Central System is powered by AWS and serves as the backbone of connectivity and data management. AWS enables the monitoring and updating of the IoT device, providing robust and reliable cloud-based services essential for real-time data processing and device management. In conjunction with AWS, the project's website acts as a comprehensive interface. The website not only provides a user-friendly platform for interacting with the system but also complements the OLED screen by offering additional control and monitoring capabilities. Moreover, the website facilitates near real-time streaming of camera footage from the Home System, enhancing the user's ability to remotely monitor and interact with their environment.

### Home Station


The Home Station is centered around a Raspberry Pi, which is responsible for controlling various functionalities including laser game modes, motion detection messaging, and video streaming. This system is critical for executing the interactive features of the project. The Raspberry Pi manages the laser game modes, allowing for different configurations and user-controlled activities. It also handles motion detection, sending alerts and messages based on detected movements. Additionally, the Raspberry Pi streams video footage, which is crucial for real-time monitoring and interaction.

The integration and coordination of these three systems involve the interaction of several Finite State Machines (FSMs). The complexity of managing multiple FSMs to ensure seamless communication and functionality across the Away, Central, and Home Systems posed significant challenges. These FSMs are responsible for maintaining the state and behavior of each component, ensuring they respond appropriately to user inputs and system events.
  </div>
</div>

## Functional Specification

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;flex-direction:column;align-items:center;">
  <div style="display:inline-block;vertical-align:top;flex:1 0 300px;margin-bottom:0;">
    In these diagrams we can see the state machines we designed. These three machines all start in idle modes and wait on user input. For the away CC3200 the input is the IR controller. For the home station and the Raspberry Pi the user initiates contact through the website in order to start streaming video or to configure the laser modes.
  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 500px;margin-top:0;">
    <div class="fig">
      <img src="./media/states.png" style="width:90%;height:auto;" />
      <span class="caption">State diagrams for our machines</span>
    </div>
  </div>
</div>


# Implementation
## Home Station
The CC3200 and Raspberry Pi work in tandem to provide the user with a fun experience. We see that each machine is in an idle state awaiting a stimulus from the user or a motion detector. This will then initiate the machines to enter their operation loops where the Raspberry Pi is streaming video and the CC3200 is controlling the servo as per the user's instructions.



### IR Sensor
<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;">
  <div style="display:inline-block;vertical-align:top;flex:1 0 300px;">
    To allow the user to receive notifications vis SNS over AWS that their cat was nearby the home station we utilize a mounted HC-SR501 IR sensor. This sensor is configured to have a very long delay which is adjustable on the device. This prevented the detector from going off too often. The IR sensor is wired to the CC3200 and utilizes a GPIO interupt so signal that the IR sensor has been triggered. 
  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 500px">
    <div class="fig">
      <img src="./media/motionsensor.JPG" style="width:90%;height:auto;" />
      <span class="caption">Home station with motion sensor on bottom left</span>
    </div>
  </div>
</div>



### Lazer
The laser is on a 2 axis servo system. The servos themselves use are controlled by PWM and in order to control them we had to implement our own version of the 20mS period 1%-10% duty cycle of PWM to communicate with these servos. By associating the percentage of duty cyle with the position of the servo we were able to utilize servo structs in our code to control each of them independently and in a coordinated manner so as to facilitate game modes for the user to select. 

### Mounting

<div style="display:flex;flex-wrap:wrap;justify-content:space-evenly;">
  <div style="display:inline-block;vertical-align:top;flex:1 0 300px;">
  The mountings seen in the home station were the combination of various 3D models. (See references at bottom) I created a central plane for the home station and added a custom protoboard PCB. This was more robust and compact when compared to the breadboard. I then added 2.5mm mounting for the CC3200 and the Raspi. From there I took a design for a ball and socket, scaled to a size to fit our needs and printed the mounts for the camera and the laser. The camera is also enclosed in a 3D printed container. We fear that the raspi container is not well vented enough, but that could be easily addressed in a beta version. The laser itself is held in place by a 3D printed module that allows for wiring to be routed and freedom of movement for both servos. 
  </div>
  <div style="display:inline-block;vertical-align:top;flex:0 0 500px">
    <div class="fig">
      <img src="./media/PCB.JPG" style="width:90%;height:auto;" />
      <span class="caption">PCB for the homestation CC3200 to integrate devices</span>
    </div>
  </div>
</div>


## Central Servers
### AWS IoT Core
### AWS Kinesis WebRTC
### Custom Web server
## Away Device
The away station is a CC3200 controlled assembly. The intention is that if the user does not have acces to the website to control the laser, they can instead utilize the IR remote control with this assembly. The user is able to send messages over IR NEC protocol to the CC3200 where the user can switch between various games mode and settings. The user can see their controller inputs on the CC3200 in real time with the integrated OLED screen. 




### Setting up IR Receiver

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>
    The key component in the users' ability to communicate with the CC3200 is the Vishay made IR receiver. Using the NEC code IR protocol we were able to utilize GPIO interrupts on the CC3200 to detect message wave forms as they were broadcast by an IR remote control. By parsing these messages from the user it can allow for laser control and menu surfing through the game modes.   
  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <div class="fig">
      <img src="./media/away_circuit.jpg" style="width:auto;height:2in" />
      <span class="caption">IR Receiver Wiring Diagram</span>
    </div>
  </div>
</div>

### Setting up OLED

<div style="display:flex;flex-wrap:wrap;justify-content:space-between;">
  <div style='display: inline-block; vertical-align: top;flex:1 0 400px'>
    To give the user a GUI we utilized this OLED screen. By connecting the OLED to the CC3200 via an SPI bus we were able to refesh and update the screen responsively to users' input.
  </div>
  <div style='display: inline-block; vertical-align: top;flex:0 0 400px'>
    <div class="fig">
      <img src="./media/OLED.png" style="width:auto;height:2in" />
      <span class="caption">OLED wiring to implment SPI</span>
    </div>
  </div>
</div>

# Challenges

The final project presented numerous challenges, primarily due to the complexity of integrating a custom web server with AWS IoT and AWS WebRTC to function seamlessly together. The logistics of combining these components were particularly daunting, requiring extensive troubleshooting and fine-tuning to ensure they worked harmoniously.

Implementing live streaming with AWS Kinesis WebRTC was notably difficult, especially since it was our first attempt. Initially, we focused on using a computer camera as the master device. Once we managed to get this working, we transitioned to implementing it on a Raspberry Pi, which introduced new challenges. Ensuring minimal delay and acceptable resolution was critical. Additionally, configuring the Raspberry Pi to work with the PiCam required dealing with outdated versions and setting GStreamer parameters for optimal performance.

Another significant challenge was setting the Raspberry Pi to run the necessary Python script on boot for a plug-and-play experience. This task involved resolving numerous issues, which we eventually overcame using cron jobs to automate the process. This was essential for the smooth operation of our project.

A major challenge was selecting the appropriate libraries and SDKs for the custom web server to detect updates to the device shadow. Instead of using polling GET requests, we successfully implemented a solution using MQTT topics and subscriptions. This approach required a deep understanding of MQTT protocols and AWS IoT functionalities, making it a complex but rewarding task.

An attempt was made to implement a manual laser mode using real-time updates from a TV remote. However, this approach was unsuccessful due to inherent delays in motor responses to inputs, which could not be mitigated. Despite this, we learned valuable lessons about the limitations of the hardware we were using.

Overall, the project involved overcoming a series of complex technical challenges. Despite the difficulties, we were able to find solutions for most of the issues, resulting in a functional and integrated system. The experience has been incredibly educational, highlighting the importance of persistence and problem-solving in tackling sophisticated technical projects.

# Future Work

Due to the limitations of the motors, we were unable to fully implement the manual control mode of the laser. The inherent delays in motor responses prevented the system from achieving the desired real-time control. To overcome this, future work will focus on minimizing stream delay to enhance the responsiveness of the manual control system. This could involve optimizing the current setup or exploring alternative hardware solutions that offer faster response times. Additionally, improving the control algorithms to ensure smoother and more precise movements will be a priority. These enhancements will make the manual control mode more reliable and user-friendly.

Another significant area for future development is the implementation of an advanced laser mode that moves in random speeds and directions. This feature would add a dynamic and unpredictable element to the system, increasing its versatility and potential applications. Developing this mode will require sophisticated algorithms that can generate random patterns while ensuring the laser moves safely and effectively within its operational environment. The integration of sensors to monitor the laser's position and adjust its movement in real-time could further enhance this feature.

To support these improvements, further work will also involve optimizing the live streaming capabilities to reduce latency. Achieving minimal stream delay is crucial for both the manual control and the new random movement mode. This might include fine-tuning the AWS Kinesis WebRTC settings, upgrading network infrastructure, or adopting more efficient video compression techniques.

# Bill of Materials

<!-- you can convert google sheet cells to html for free using a converter
  like https://tabletomarkdown.com/convert-spreadsheet-to-html/ -->

<table style="border-collapse:collapse;">
<thead>
  <tr>
    <th><p>No.</p></th>
    <th><p>Part Name</p></th>
    <th><p>Description</p></th>
    <th><p>Qty</p></th>
    <th><p>Supplier / Manufacturer</p></th>
    <th><p>Unit Cost</p></th>
    <th><p>Total Part Cost</p></th>
    <th><p>Purpose</p></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><p>1</p></td>
    <td><p>CC3200-LAUNCHXL</p></td>
    <td><p>MCU Board</p></td>
    <td><p>2</p></td>
    <td><p>Texas Instruments</p></td>
    <td><p>$66.00</p></td>
    <td><p>$132.00</p></td>
    <td><p>Controlling home, away (remote) devices</p></td>
  </tr>
  <tr>
    <td><p>2</p></td>
    <td><p>Adafruit 1431 OLED</p></td>
    <td><p>128x128 RGB OLED Display using SPI protocol</p></td>
    <td><p>1</p></td>
    <td><p>Adafruit</p></td>
    <td><p>$40.00</p></td>
    <td><p>$40.90</p></td>
    <td><p>Display GUI for selecting lazer modes</p></td>
  </tr>
  <tr>
    <td><p>3</p></td>
    <td><p>TV Remote</p></td>
    <td><p>AT&T remote controller</p></td>
    <td><p>1</p></td>
    <td><p>AT&T</p></td>
    <td><p>$41.00</p></td>
    <td><p>$41.00</p></td>
    <td><p>For selecting lazer modes</p></td>
  </tr>
  <tr>
    <td><p>4</p></td>
    <td><p>Remote IR Reciever</p></td>
    <td><p>6mm Silicone Tube: 1 meter length</p></td>
    <td><p>1</p></td>
    <td><p>Vishay Semiconductor</p></td>
    <td><p>$1.40</p></td>
    <td><p>$1.40</p></td>
    <td><p>For receiving IR signals from TV remote</p></td>
  </tr>
  <tr>
    <td><p>5</p></td>
    <td><p>330 Ohm Resistor</p></td>
    <td><p>Resistor to setup with OLED screen</p></td>
    <td><p>1</p></td>
    <td><p>Digikey</p></td>
    <td><p>$0.29</p></td>
    <td><p>$0.29</p></td>
    <td><p>For setting up circuit with OLED</p></td>
  </tr>
  <tr>
    <td><p>6</p></td>
    <td><p>10uF Capacitor</p></td>
    <td><p>Capacitor to setup with OLED screen</p></td>
    <td><p>1</p></td>
    <td><p>Digikey</p></td>
    <td><p>$0.10</p></td>
    <td><p>$0.10</p></td>
    <td><p>For setting up circuit with OLED</p></td>
  </tr>
  <tr>
    <td><p>7</p></td>
    <td><p>Raspberry Pi 3</p></td>
    <td><p>Microprocessor, Model B+</p></td>
    <td><p>1</p></td>
    <td><p>Raspberry Pi Foundation</p></td>
    <td><p>$35.00</p></td>
    <td><p>$35.00</p></td>
    <td><p>For live streaming</p></td>
  </tr>
  <tr>
    <td><p>8</p></td>
    <td><p>Pi Camera</p></td>
    <td><p>Camera to be used with Raspberry Pi</p></td>
    <td><p>1</p></td>
    <td><p>BBTO</p></td>
    <td><p>$4.99</p></td>
    <td><p>$4.99</p></td>
    <td><p>For live streaming</p></td>
  </tr>
  <tr>
    <td><p>9</p></td>
    <td><p>Servo Motors</p></td>
    <td><p>Sg90 9g Micro Servo Motors</p></td>
    <td><p>2</p></td>
    <td><p>Miuzei</p></td>
    <td><p>$1.87</p></td>
    <td><p>$1.87</p></td>
    <td><p>To control the direction of the lazer beam</p></td>
  </tr>
  <tr>
    <td><p>10</p></td>
    <td><p>HC-SR501 IR Sensor</p></td>
    <td><p>Motion detection sensor</p></td>
    <td><p>1</p></td>
    <td><p>Stemedu</p></td>
    <td><p>$1.70</p></td>
    <td><p>$1.70</p></td>
    <td><p>Detect for cat's presence</p></td>
  </tr>
  <tr>
    <td><p>11</p></td>
    <td><p>Lazer</p></td>
    <td><p>Lazer module, 3.3V</p></td>
    <td><p>1</p></td>
    <td><p>HiLetgo</p></td>
    <td><p>$1.40</p></td>
    <td><p>$1.40</p></td>
    <td><p>For shooting lazer beam</p></td>
  </tr>
  <tr>
    <td colspan="3">
      <p>Total Parts</p></td>
    <td><p>13</p></td>
    <td colspan="2">
      <p>Total Cost</p></td>
    <td><p>$261.62</p></td>
    <td></td>
  </tr>
  <tr>
    <td colspan="3">
      <p>Total Parts (Excluding Provided)</p></td>
    <td><p>6</p></td>
    <td colspan="2">
      <p>Total Cost (Exluding Provided)</p></td>
    <td><p>$46.83</p></td>
    <td></td>
  </tr>
</tbody>
</table>




## References for 3D Models
Thank you to everyone in the open source community who continues to put out significant and useful work for others to build off of.
- [Raspi camera case](https://www.thingiverse.com/thing:2199829)
- [Motion sensor case](https://www.thingiverse.com/thing:291270)
- [Raspi Case](https://www.thingiverse.com/thing:587676)
- [CC3200 holes and layout](https://www.thingiverse.com/thing:3251774)
- [Servo holders](https://www.thingiverse.com/thing:3045553)
- [Ball and sockets](https://www.thingiverse.com/thing:2069128)