**Prototype Smart Garage System**

Overview

-   Closes garage door automatically if left open and garage is unoccupied.
-   Disables garage door when SUV hatch is open to prevent damage to hatch.
-   Sensors
-   Garage door open/closed
-   Garage motion
-   Garage temperature, humidity
-   SUV hatch door open/closed
-   Ultrasonic auto presence detector
-   Actuators
-   Kasa smart plug for garage motor
-   Lillygo TT-GO Sim7000G Cellular module with wifi is central brain
-   Optional Raspberry pi 3B+ with Home Assistant for dashboard and added control
-   Software features
-   Cellphone notification when garage door is opened or closed
-   Remote control of garage door from cellphone
-   Displays status of all garage sensors via cellular network
    -   Car present or absent
    -   Door open or closed
    -   Car hatch open or closed
    -   Motion in garage
    -   Light on/off
    -   Temperature/Humidity

Implementation

-   Garage Monitor Components
-   ESP32, PIR sensor, temperature/humidity sensor, photodiode, OLED display, shielded cable to garage door reed switch.
-   

    ![](media/ade95d8ab695bca9f659d096f5079013.jpeg)

-   Use Velcro to attach one side of the reed switch to the garage door rail, the other side of the switch to the garage door.

    ![](media/72bd16fbafb396f29f59f3b1e8627231.jpeg)

-   Software: The software app GarageDoor.ino reports motion and garage door state as well as light level, temperature, and humidity wirelessly to the Lillygo module via ESPNOW . The software documents the pin connections for the sensors.
-   SUV Ultrasonic auto monitor

    ![](media/45ec8d44794ab97698b2ebf1c525d678.jpeg)

-   eSP32, ultrasonic sensor, bracket measure distance to floor in inches. When car is absent, it reports around 80 inches. When car is present, it reports 15 inches or so.
-   UltraSonic Software: The software app UltrasonicTapeMeasure.ino as mounted reports distance from ceiling of a reflective surface to the Garage Controller via ESPNOW . The software documents the pin connections for the ultrasonic sensor.
-   SUV Hatch Sensor

![](media/67181e84636669b890651ea83edcb493.jpeg)

-   Esp32, Reed Switch mounted just inside hatch.
-   

    ![](media/a975849f84c5b57e345271f3d92f3f71.jpg)

-   Hatch Software: AutoHatch.ino reports hatch status to the Garage Controller via ESPNOW . The software documents the pin connections for the reed switch.
-   Kasa smart plug for garage motor is controlled by GarageController

    ![](media/164c67ccbf249880eb1e21511afdc2cf.jpeg)

-   GarageController

    ![](media/1e0b090703a91f05d0da345a1a7861db.jpeg)

-   LillyGo TTGO sim 7000G, Relay, temperature/humidity sensor, garage remote. Open the remote control and tack 2 wires to the switch for connection to the relay.
-   Software: GarageControler.ino receives data wirelessly via ESPNOW from GarageDoor.ino, UltrasonicTapeMeasrure,ino, and Hatch.ino. It can open or close the garage door by activating the relay, which simulates pressing the button on the remote control.
    -   Disables garage door if car is present AND hatch is up by signaling the Kasa smart plug.
    -   Closes garage door if the car is absent AND the garage door is open AND there has been no motion for an hour.
