PiPan Cam
=========

A set of scripts used to control a pan-tilt camera setup on Raspberry Pi running <a href="https://github.com/sarfata/pi-blaster/" target="_blank">pi-blaster</a> and optionally with <a href="https://github.com/ccrisan/motioneye" target="_blank">motioneye</a>.

![Setup example 1](/setup_example1.jpg)
![Setup example 2](/setup_example2.jpg)


Prerequisites
=============
 - <a href="https://www.raspberrypi.org/products/" target="_blank">Raspberry Pi</a> with a Camera (eg. <a href="https://www.raspberrypi.org/products/camera-module-v2/" target="_blank">this</a>)
 - a pan-tilt setup: platform (<i><a href="https://www.google.com/search?q=Pan%2FTilt+Camera+Platform" target="_blank">you google it</a></i>) and servos (eg. <i><a href="https://www.google.com/search?q=SG90+servo" target="_blank">SG90<a/></i>)
 - installed <a href="https://github.com/sarfata/pi-blaster/" target="_blank">pi-blaster</a>

Features and usage
==================
There are two main scripts, which do the work ...
<br/>Their job is to move the servos while watching limiting values and to keep track of the current position.

pipan_set.sh
------------
This script sets a specific position of the camera according to passed arguments in PWM %.

<b>But first change the script with your defaults:</b>
<br/>(You have to set the pins used for your servos and determine the limiting values where servos can reach - simply by shifting small steps.)

     ENABLED=1              # enables pi-blaster calls (set to 0 for testing)
     myfile="./pipandata"   # file storing current position
     vertical="0.13"        # default vertical position
     horizontal="0.2"       # default horizontal position
     v_pin=18               # pin for vertical servo
     h_pin=17               # pin for horizontal servo
     v_max="0.2"            # vertical max value (down for me)
     v_min="0.055"          # vertical min value (up for me)
     h_max="0.285"          # horizontal max value (left for me)
     h_min="0.06"           # horizontal min value (right for me)

<b>Example</b>

     sh pipan_set.sh 0.12 0.2

pipan_step.sh
-------------
This script moves the camera by steps according to passed arguments.
<br/>Use positive and negative whole numbers.

<b>But first change the script with your defaults:</b>
<br/><i>(two more than in previous script)</i>

     v_step="0.005"         # one vertical step
     h_step="0.005"         # one horizontal step

<b>Example</b>

     sh pipan_step.sh 2 -5 

<i>That would move my camera two steps down and five steps right.</i>

Notes
-----
Don't forget to make the scripts executable:

     chmod +x sript_name.sh

You'll need <b>bc</b> installed:

     sudo apt-get install bc

If you run into problems with starting pi-blaster like <a href="https://github.com/sarfata/pi-blaster/issues/68" target="_blank">this</a>, <a href="https://github.com/sarfata/pi-blaster/issues/72" target="_blank">this</a>, or <a href="https://github.com/sarfata/pi-blaster/issues/71" target="_blank">this</a> ... You'll have to create the default config with:

     touch /etc/default/pi-blaster

Usage with motioneye
--------------------
Current (2016/07/06) version of <a href="https://github.com/ccrisan/motioneye" target="_blank">motioneye</a> only offers a limited way of interfacing external/custom commands... via action buttons - see <a href="https://github.com/ccrisan/motioneye/wiki/Action-Buttons" target="_blank">here</a>.
<br/>And you can use this to call your scripts, which move the camera simply by clicking buttons within the camaera view :)

I've set that up using the scripts in [examples](/examples/) folder like this:

     sudo cp <.yourfolder.>/PiPan_Cam/examples/pipan_go_home.sh /etc/motioneye/lock_1
     sudo cp <.yourfolder.>/PiPan_Cam/examples/pipan_step_left.sh /etc/motioneye/light_on_1
     sudo cp <.yourfolder.>/PiPan_Cam/examples/pipan_step_right.sh /etc/motioneye/light_off_1
     sudo cp <.yourfolder.>/PiPan_Cam/examples/pipan_step_down.sh /etc/motioneye/alarm_on_1
     sudo cp <.yourfolder.>/PiPan_Cam/examples/pipan_step_up.sh /etc/motioneye/alarm_off_1

License
=======
MIT License (MIT) - see [here](LICENSE.txt)

Change Log
==========
**v 1.0 - Jul 06, 2016**

 - Initial version

Future plans / TODOs
--------------------

 -  moving by degrees and to a specific degree
 