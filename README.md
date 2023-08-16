**Prototype Smart Garage System**

I. Overview

1.  Closes garage door automatically if left open and garage is unoccupied.
2.  Disables garage door when SUV hatch is open to prevent damage to hatch.
3.  Sensors
    a.  Garage door open/closed
    b.  Garage motion
    c.  Garage temperature, humidity
    d.  SUV hatch door open/closed
    e.  Ultrasonic auto presence detector
4.  Actuators
    a.  Kasa smart plug for garage motor
    b.  Lillygo TT-GO Sim7000G Cellular module with wifi is central brain
    c.  Optional Raspberry pi 3B+ with Home Assistant for dashboard and added control
5.  Software features
    a.  Cellphone notification when garage door is opened or closed
    b.  Remote control of garage door from cellphone
    c.  Displays status of all garage sensors via cellular network
        1.  Car present or absent
        2.  Door open or closed
        3.  Car hatch open or closed
        4.  Motion in garage
        5.  Light on/off
        6.  Temperature/Humidity

II. Implementation

1.  Garage Monitor Components
    a.  ESP32, PIR sensor, temperature/humidity sensor, photodiode, OLED display, shielded cable to garage door reed switch.

        ![](media/bca56718512cff94e3ae9f505154e238.jpeg)

    b.  Use Velcro to attach one side of the reed switch to the garage door rail, the other side of the switch to the garage door.

        ![](media/d6b159c0feb38c88f7262479bc8c30d0.jpeg)

    c.  Software: The software app GarageDoor.ino reports motion and garage door state as well as light level, temperature, and humidity wirelessly to the Lillygo module via ESPNOW . The software documents the pin connections for the sensors.
2.  SUV Ultrasonic auto monitor

    ![](media/155fedb500b74103b4504253aa0dc17b.jpeg)

    a.  eSP32, ultrasonic sensor, bracket measure distance to floor in inches. When car is absent, it reports around 80 inches. When car is present, it reports 15 inches or so.
    b.  UltraSonic Software: The software app UltrasonicTapeMeasure.ino as mounted reports distance from ceiling of a reflective surface to the Garage Controller via ESPNOW . The software documents the pin connections for         the ultrasonic sensor.
3.  SUV Hatch Sensor

    ![](media/e0e813020a1122cb4e3432c0207e45a9.jpeg)

    a.  Esp32, Reed Switch mounted just inside hatch.

        ![](media/24cee41256db20ced5ef12ec1b009278.jpeg)

    b. Hatch Software: AutoHatch.ino reports hatch status to the Garage Controller via ESPNOW . The software documents the pin connections for the reed switch.

4.  Kasa smart plug for garage motor is controlled by GarageController

    ![](media/75db965b27232d53a1344054d6c27997.jpeg)

5.  GarageController

    ![](media/fb1f43d364c5d00a4e6e22351b78055e.jpeg)

    a.  LillyGo TTGO sim 7000G, Relay, temperature/humidity sensor, garage remote. Open the remote control and tack 2 wires to the switch for connection to the relay.
    b.  Software: GarageControler.ino receives data wirelessly via ESPNOW from GarageDoor.ino, UltrasonicTapeMeasrure,ino, and Hatch.ino. It can open or close the garage door by activating the relay, which simulates pressing the button on the remote control.
		i. Disables garage door if car is present AND hatch is up by signaling the Kasa smart plug.

               ii. Closes garage door if the car is absent AND the garage door is open AND there has been no motion for an hour.
