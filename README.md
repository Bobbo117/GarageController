# Prototype Smart Garage System 

-  Closes garage door automatically if left open and garage is unoccupied
-  Disables garage door when vehicle hatch is open to prevent damage to hatch
-  No wire connection or modification to garage door motor control circuitry
-  Communicates via cellular ioT and ESPNOW wireless protocol; no dependence on WiFi router
## Sensors
-   Garage door open/closed
-   Garage motion
-   Garage temperature, humidity
-   Vehicle hatch open/closed
-   Ultrasonic vehicle presence detector
## Actuators
-   Kasa smart plug for garage motor
-   Garage Controller connects to garage remote to open or close garage door
-   Optional Raspberry pi 3B+ with Home Assistant for dashboard and added control
## Software features
-   Arduino C++, adapted from **Ambient** and **AmbientHUB**  software in the current group of repositories    
-   Cellphone notification when garage door is opened or closed
-   Remote control of garage door from cellphone
-   Reports status of all garage sensors via cellular network
    -   Car present or absent
    -   Door open or closed
    -   Vehicle hatch open or closed
    -   Motion in garage
    -   Light on/off
    -   Temperature/Humidity
    -   The software documents the pin connections for the sensors.


## Garage Monitor Components
-   [ESP32 development board](//www.amazon.com/MELIFE-Development-Dual-Mode-Microcontroller-Integrated/dp/B07Q576VWZ?th=1)
-   [PIR sensor](//www.amazon.com/DIYmall-HC-SR501-Motion-Infrared-Arduino/dp/B012ZZ4LPM)
-   [temperature/humidity sensor](//www.amazon.com/gp/product/B092495GZJ/ref=ox_sc_act_image_21?smid=A39S0U3UP1U7UG&th=1)
-   [photoresistor](https://www.amazon.com/eBoot-Photoresistor-Sensitive-Resistor-Dependent/dp/B01N7V536K/)
-   [OLED display](//www.amazon.com/Dorhea-Display-SSD1306-Self-Luminous-Raspberry/dp/B07WPCPM5H/?th=1)
-   shielded cable to garage door [magnetic reed switch.](//www.amazon.com/dp/B09BJLRK4S/)

    ![](media/ade95d8ab695bca9f659d096f5079013.jpeg)
    

-   Use Velcro to attach one side of the magnetic reed switch to the garage door rail, the other side of the switch to the garage door.


    ![](media/72bd16fbafb396f29f59f3b1e8627231.jpeg)


-   **GarageDoor.ino** software reports motion and garage door state, light level, temperature, and humidity wirelessly to the Garage Controller via ESPNOW . 

    ### Ultrasonic vehicle monitor reports whether vehicle is present or not:
    
    ![](media/45ec8d44794ab97698b2ebf1c525d678.jpeg)

-   [eSP32 development board](//www.amazon.com/MELIFE-Development-Dual-Mode-Microcontroller-Integrated/dp/B07Q576VWZ?th=1)[, ultrasonic sensor](//www.amazon.com/dp/B07VZBYSLX/ref=cm_sw_em_r_mt_dp_05X0C8N082YTTBTS895K?_encoding=UTF8&psc=1&pldnSite=1), bracket measure distance in inches.
-   When vehicle is absent, it reports 80 inches.
-   When vehicle is present, it reports 15 inches.
-   **UltrasonicTapeMeasure.ino** software reports to the Garage Controller via ESPNOW.
  
  ###  Vehicle Hatch Sensor reports hatch open or closed state:

![](media/67181e84636669b890651ea83edcb493.jpeg)

-   [Esp32 development board](//www.amazon.com/MELIFE-Development-Dual-Mode-Microcontroller-Integrated/dp/B07Q576VWZ?th=1)[, Reed Switch mounted just inside hatch.](//www.amazon.com/dp/B09BJLRK4S/)

    ![](media/a975849f84c5b57e345271f3d92f3f71.jpg)

-   **AutoHatch.ino** software reports hatch status to the Garage Controller via ESPNOW.
<br>
<br>
    [Kasa smart plug](//www.amazon.com/gp/product/B07RCNB2L3/ref=ox_sc_act_title_28?smid=ATVPDKIKX0DER&th=1) is controlled by the GarageController to disable power to garage motor when vehicle hatch is open:
    
  ![](media/164c67ccbf249880eb1e21511afdc2cf.jpeg)
<br>
<br>
### GarageController is [LillyGo TTGO sim 7000G](//www.amazon.com/LILYGO-Development-ESP32-WROVER-B-Battery-T-SIM7000G/dp/B099RQ7BSR), ioT SIM card, [relay](//www.amazon.com/gp/product/B079FJSYGY/ref=ox_sc_act_title_11?smid=A11A70Q280RHPK&th=1), [temperature/humidity sensor,](//www.amazon.com/gp/product/B092495GZJ/ref=ox_sc_act_image_21?smid=A39S0U3UP1U7UG&th=1) garage remote control, [OLED display:](//www.amazon.com/Dorhea-Display-SSD1306-Self-Luminous-Raspberry/dp/B07WPCPM5H/?th=1)
<br>
<br>
    
  ![](media/1e0b090703a91f05d0da345a1a7861db.jpeg)
<br>
<br>
- Open the remote control and tack 2 wires to the switch for connection to the relay.
  <br>
- **GarageControler.ino** software receives data wirelessly via ESPNOW from the sensors described above.
  <br>
- Garage door is opened or closed by activating the relay, which simulates pressing the button on the garage remote.
  <br>
  <br>
        -   Disables garage door if vehicle is present AND hatch is up by signaling the Kasa smart plug.
  <br>
  <br>
        -   Closes garage door if the vehicle is absent AND the garage door is open AND there has been no motion for an hour.


