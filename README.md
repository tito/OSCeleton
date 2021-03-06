OSCeleton
=========

What is this?
-------------

As the title says, it's just a small program that takes kinect
skeleton data from the OpenNI framework and spits out the coordinates
of the skeleton's joints via OSC messages. These can can then be used
on your language / framework of choice.


How do I use it?
----------------

### First you need to install the OpenNI driver, framework, and middleware
#### Windows
Get avin's hacked Primesense PSDK driver for kinect:
[https://github.com/avin2/SensorKinect](https://github.com/avin2/SensorKinect)
Folow his instructions for installing the OpenNI framwork, the driver,
and the NITE middleware.

#### Linux
There's a great post about how to get OpenNI working in linux  here:
[http://www.keyboardmods.com/2010/12/howto-kinect-openninite-skeleton.html](http://www.keyboardmods.com/2010/12/howto-kinect-openninite-skeleton.html)

#### After OpenNI is working
Then you can run one of the precompiled binaries in the "bin"
directory or compile your own: Just type "make" on linux or osx; on
windows you can use the VC++ express .sln file.

If you run the executable without any arguments, it will send the OSC
messagens in the default format to localhost on port 7110.
To learn about the OSC message format, continue reading below or check
out our processing examples at
[https://github.com/Sensebloom/OSCeleton-examples](https://github.com/Sensebloom/OSCeleton-examples)

#### Other stuff
Another fun way to test OSCeleton is to use the awesome animata
skeletal animation software by the Kitchen Budapest guys. You can get
it at:
[http://animata.kibu.hu/](http://animata.kibu.hu/)

Animata needs its OSC messages in a very specific format, so you must
run OSCeleton like this:
    OSCeleton -d 2 -n 0 -mx 320 -my 240

(This will be deprecated, I'm introducing a simpler option soon for
animata compatibility)

If your animation is going crazy try to play with -mx and -my values,
and -ox and -oy values too ;)

To get a complete list of available options run OSCeleton -h.


OSC Message format
------------------

### New user detected - no skeleton available yet. This is a good time
### for you to ask the user to do the calibration pose:

    Address pattern: "/new_user"
    Type tag: "i"
    i: A numeric ID attributed to the new user.


### New skeleton detected - The calibration was finished successfully,
### joint coordinate messages for this user will be incoming soon ;):

    Address pattern: "/new_skel"
    Type tag: "i"
    i: ID of the user whose skeleton is detected.


### Lost user - we have lost the user with the following id:

    Address pattern: "/lost_user"
    Type tag: "i"
    i: The ID of the lost user. (This ID will be free for reuse from now on)


### Joint message - message with the coordinates of each skeleton
### joint:

    Address pattern: "/joint"
    Type tag: "sifff"
    s: Joint name, check out the full list of joints below.
    i: The ID of the user.
    f: X coordinate of joint in interval [0.0, 1.0]
    f: Y coordinate of joint in interval [0.0, 1.0]
    f: Z coordinate of joint in interval [0.0, 7.0]

If you use "-d 2" option, the typetag will be "siff", and no Z
coordinate will be sent. (This will be deprecated in favor of a
simpler option for animata compatibility)

If you use "-n 0" option, the typetag will be "sfff", and no user ID,
new_user, new_skel, lost_user messages will be sent. (This will be
deprecated in favor of a simpler option for animata compatibility)


### Full list of joints

* head
* neck
* torso

* r_collar #not working yet
* r_shoulder
* r_elbow #not working yet
* r_wrist #not working yet
* r_hand
* r_finger #not working yet

* l_collar #not working yet
* l_shoulder
* l_elbow #not working yet
* l_wrist #not working yet
* l_hand
* l_finger #not working yet

* r_hip
* r_knee
* r_ankle
* r_foot

* l_hip
* l_knee
* l_ankle
* l_foot


Other
-----

### Death threats and other stuff can be sent to:
<info@sensebloom.com>

Have fun!
