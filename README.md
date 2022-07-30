# 3D Printer_MKS_1.4

Setting up MKS_GEN_V1.4 board, using the Anet A8 plus as a base for the modifications

<h2>Wiring</h2>
Comming from the base of a Anet A8 plus most of the wires you can just plug in. (Exceptions being End-Stop, Touch Sensors) <br><br>

![image](https://user-images.githubusercontent.com/30980904/149030575-46fe47b8-741a-4e4f-95e5-59bbe15cdc8c.png) <br>

<h3>Get the Sensor/Data line to the left with ground in the middle and V5 to the right.</h3><br>

![image](https://user-images.githubusercontent.com/30980904/148625604-0b005473-f4f0-4b00-a7c4-4caa88d126ff.png) <br>


![image](https://user-images.githubusercontent.com/30980904/148626779-cb2ca516-b31d-4285-9a10-1c3d6408c83b.png) <br>

<h3>Do this with electrical tape and some jumper wires, alternatively you can remove the clipper and reclip them</h3> <br>

![image](https://user-images.githubusercontent.com/30980904/149034906-d9aa0882-5dd5-4037-8c3b-9f7f954371af.png)<br>

![image](https://user-images.githubusercontent.com/30980904/149039056-f6eee359-02f1-4bb9-8110-7a92ec691701.png)<br>

Next put in the jumper cap in the correct spot following this diagram <br>

![image](https://user-images.githubusercontent.com/30980904/149259311-b2efae1d-323f-4a7c-92b7-359e493dd743.png) <br>

For TMC2208 chips on the MKS_GEN_V1.4 its two cap on each stepper for 1/16.<br>

![image](https://user-images.githubusercontent.com/30980904/149259755-fc521aa8-2047-4d75-a4dc-a35ea7c08fa1.png) <br>

<h2>Stepper Driver (TMC2208 )</h2>

For each chip you get set the potentiometer to the vRef you will be working with on your motors. (Safety) <br>
This is done with the screw looking like insert on the chip, to get a measurement on the potentiometer place the chip in the correct orientation otherwise you will burn the chip and then measure it with a multimeter. Reference documents for orientation, usually its GND facing motor and DIR inwards.<br>
[TMC2208](https://www.youtube.com/watch?v=k3Uc1F5jgVQ&t=35s)<br>

<h3>Potentiometer</h3> <br>

![image](https://user-images.githubusercontent.com/30980904/149039989-af1e1c65-7070-47f0-b46b-936a336d074c.png) <br>

<h3>Setting VRef</h3> <br>
To find the Rated Current of your motor google the label on the motor to find the specsheet. <br>
From this you should be able to obtain the Rated current which is what we will need to set the VRef. <br>

![image](https://user-images.githubusercontent.com/30980904/149040225-6ea72dca-b8be-4748-8faa-166fdb6b2d62.png) <br>

Some boards may have you do more for example on the ANet A8 board it sets the A4988 chip for X & Y to .65V or Imax of 740mA <br>
TMC drivers use rms current not max(peak) so Irms is 740mA/1.414 = 523mA <br>
With this feed 523mA into calculator (x1.414) and you get VRef = .74v <br>
ANet uses a single A4988 for both Z Motors with a parallel splitter onboard so it doubles the current and therefore VRef value <br>
a separate Z2 step diver get you past this step.<br>

<h3>For Step/dir (legacy) </h3>
Set the VRef on the TMC's potentiometer and when you define the stepper driver write it as such <br>
#define X_DRIVER_TYPE TMC2208_STANDALONE // Stand enables the old method <br>

<h3> TMC2208 UART </h3>

If you want the UArt feature reference the chip document otherwise connect the two pins with J2.<br>
This is already done with the chip used in this project BIGTREE TMC2208 V3.0 UART <br>

![image](https://user-images.githubusercontent.com/30980904/149041022-8b46c902-cba5-4044-9e05-ca0190959511.png) <br>

If your using the SKR board, know that you only need one wire for the connection to the pinout with Tx & Rx being the same. <br>
For other boards connect one wire with two connection, one for Tx ( Resistor ) and another for Rx. <br> 
This will <b>enable</b> the UART feature from the chip, connect them to the correct pinouts and your done. <br>

![image](https://user-images.githubusercontent.com/30980904/150111765-d5d03267-5f8c-4f49-9cb0-d6d93637d3b3.png) <br>


One of the wires will need a 1k Ohm resistor on it, this can be done with protoboards, jumpers, or soldering. (Done for each stepper) <br>

![image](https://user-images.githubusercontent.com/30980904/149041622-a651139b-7dcf-4e8d-bca0-567cb96744ee.png) <br>

![image](https://user-images.githubusercontent.com/30980904/150138023-ce755e9e-ab0a-4931-a513-d219371912fc.png) <br>


The <b>resistor</b> one will be our Tx (Transmit) pin, the other is the Rx (receiver) pin. <br>
You can start connecting the pins following the diagram below. <br>

![image](https://user-images.githubusercontent.com/30980904/150113930-3652386c-aa59-4f24-9dee-09270f4ac809.png) <br>

![image](https://user-images.githubusercontent.com/30980904/149041968-9279b412-681b-43ed-90e9-9c073a80363d.png) <br>

Wiring is now done Firmware time.<br>

<h2>Firmware</h2>

Flashing the firmware on the board to get the things we want to work. <br>

First you will need : <br>
Visual Studio Code (Recommended) https://code.visualstudio.com/Download <br>or<br>Arduino https://www.arduino.cc/<br>

<h2>Visual Studio Code Only</h2>
Install the following extensions <br>

![image](https://user-images.githubusercontent.com/30980904/148621488-9139d4b5-7ca0-43f8-bbb5-770ceb5e1a88.png)

![image](https://user-images.githubusercontent.com/30980904/148621516-867daab0-b568-4bf7-860c-d65ba8a6890f.png)
<br>

<h2>Arduino Only</h2>

Make sure the Arduino board you have is an Arduino UNO varient. The mega2560 will not work nor other types of arduino board versions.<br>
This one worked for me; mileage will very. <br> 
Elegoo UNO : https://www.amazon.com/dp/B01EWOE0UU?psc=1&ref=ppx_yo2_dt_b_product_details <br>

If using a TMC2208 chip goto Tools->Manage Libraries now search for TMC2208 and install the driver. <br>

Additional setup required but is board dependent reference this : <br>
https://youtu.be/RQIizXtf9oo?t=226 <br>

<h2>Both</h2>

With that all you need now is to get marlin and your base config.<br>
Marlin : https://marlinfw.org/ <br>

Look for your printer and download the folder, we need Configuration.h,  Configuration_adv.h,  _Bootscreen.h <br> 
Configurations Files : https://github.com/MarlinFirmware/Configurations/tree/import-2.0.x/config/examples<br>
ANet A8 plus :  https://github.com/MarlinFirmware/Configurations/tree/import-2.0.x/config/examples/Anet/A8plus <br>

Extract the base Marlin folder and then copy and paste/replace those Config files in the Marlin folder within the Marlin-2.0.x. <br>
After that open up Visual Studio Code and select the Marlin-2.0.x | base Marlin folder.<br>
Or double click the Marlin.ino to open into the arduino project. <br>

With all this done we are now ready to start messing with the Firmware; Plug in the printer cable make the changes then build/upload.<br>

<h2>Code Changes</h2>

<h3>Configuration.h</h3><br>

Search for <b>motherboard</b> and replace that with your motherboard's name.<br><br>

![image](https://user-images.githubusercontent.com/30980904/149241950-2d1d91a7-fa3c-4ae9-bf72-66760ab0621c.png) <br>

To find your motherboard name its in boards.h <br>

![image](https://user-images.githubusercontent.com/30980904/149257657-c0a41dd6-0708-4cf8-91ef-4e9fc58d1934.png) <br>
 
Or alternatively, you can go to the marlin's board page and find it there. https://marlinfw.org/docs/development/boards.html <br>
Next change the <b>Baudrate</b> to 250000; its the best case for most boards.<br>

![image](https://user-images.githubusercontent.com/30980904/149261840-00b132d0-1ce3-4740-869a-3bc1706bdfb0.png) <br>

Right below you can change the Custom_Machine_name to whatever you want its optional. <br>
Next go to the EndStop section and <b>enable</b> <b>USE_ZMAX_PLUG</b>. <br>

![image](https://user-images.githubusercontent.com/30980904/149428840-0a2cf3cd-d36f-402d-bce9-22998e30c2f4.png) <br>

Scroll down to <b>Z_MIN_PROBE_ENDSTOP_INVERTING</b> and make this <b>false</b>.<br>
If your <b>Z_MIN_ENDSTOP_INVERTING</b> is <b>true</b> then your going to set it <b>false</b>, this variable mimics the Z_MIN_PROBE<br>

![image](https://user-images.githubusercontent.com/30980904/149433706-51b1e4db-758d-493a-84bb-3c1d6d785ede.png) <br>

![image](https://user-images.githubusercontent.com/30980904/149433499-fbf48357-c99f-481a-baca-edfe71c3f35c.png) <br>

Make sure <b>Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN</b> is <b>enable</b>, use can also use set the pin yourself by scrolling down <br>

![image](https://user-images.githubusercontent.com/30980904/149429297-c5870602-200b-482f-b2a4-98cc87a4c6d2.png) <br>

Now search BLTOUCH <b>enable</b> it <br>

![image](https://user-images.githubusercontent.com/30980904/149430682-41b8840f-1016-4129-b50f-fc74bec7ed4c.png) <br>

You can put in the offset to the 3d Touch sensor in the section below, but is optional since you can do this step later. <br>
There is a required min distance from the nozzle so check your manuel on the 3d touch sensor.<br>

![image](https://user-images.githubusercontent.com/30980904/149431715-45606b82-58f8-4ea5-8271-600609351802.png) <br>

Next <b>enable</b> <b> AUTO_BED_LEVELING_BILINEAR</b> then go to section who uses the <b>GRID_MAX_POINTS_X</b> and change that to 5. <br>

![image](https://user-images.githubusercontent.com/30980904/149432072-eda94531-079b-403c-b6cf-bff7f4897a91.png) <br>

![image](https://user-images.githubusercontent.com/30980904/149432424-b4d39c47-665f-4259-83f2-4639db8b552c.png) <br>

Optional but recommended, <b>enable</b> <b>Z_SAFE_HOMING</b> <br>

![image](https://user-images.githubusercontent.com/30980904/149432664-0e918e6f-d52f-4c84-9f0a-e12df2d71d3b.png) <br>

The settings to save the nozzle distance & leveling is next, search for both <b>EEPROM_SETTINGS</b> & <b>RESTORE_LEVELING_AFTER_G28</b> <br>

![image](https://user-images.githubusercontent.com/30980904/149433072-3554516d-d18a-4ea0-bc50-85123bb43dc0.png) <br>

![image](https://user-images.githubusercontent.com/30980904/149433182-141aa872-ffac-4327-b468-7d5069943a25.png) <br>

Moving on from the 3D touch sensor, now we do the step drivers, search for <b>stepper drivers</b> and change the driver type. <br>
Done by replacing the type with the chip you have, the one used here is the TMC2208 you also add the 5th driver here if you have it. <br> 

![image](https://user-images.githubusercontent.com/30980904/149440401-b183c181-43c9-4c23-a99c-70622dc0ec64.png) <br>

To use the TMC2208 with the UArt feature we need to <b>disable ENDSTOP_INTERRUPTS_FEATURE</b> read more [here](https://github.com/MarlinFirmware/Marlin/issues/9221).<br> The TLDR : ENDSTOP_INTERRUPTS_FEATURE and the TMC2208 both use pin change interrupts solution is to define the TMC2208 on the hardware serial ports, but I won't cover that. <br>

![image](https://user-images.githubusercontent.com/30980904/149444205-8718e870-29f6-42fd-acb9-fb56f908bb74.png) <br>

Now search for the display <b>MKS_MINI_12864_V3</b> used in project, and <b>enable</b> it, then search and <b>enable NEOPIXEL_LED</b>.<br> 

![image](https://user-images.githubusercontent.com/30980904/149444323-192b5392-7e36-4a10-b1a1-8e4ad925bbba.png) <br>

![image](https://user-images.githubusercontent.com/30980904/149444367-ac84df0f-5459-4e2e-a102-ae7d2a48def5.png) <br>

Make sure to <b>disable</b> the old display used otherwise you will get an error.
<br>Since we are coming from ANET we need to <b>disable</b> the display and its delay. 
<br>LCD display is <b>ANET_FULL_GRAPHICS_LCD</b>. <br>

![image](https://user-images.githubusercontent.com/30980904/149444891-648ebd83-10dc-4b05-bdcf-942618380e70.png) <br>

With this the basic build is done on config. Build and make sure you have no errors, next open up Configuration_adv.h.<br>

<h3>Configuration_adv.h</h3>
<h4><b>Optional SDCARD</b></h4>
First <b>enable SDCARD_CONNECTION</b> and define it, the one used here ( NeoPixel ) is on the LCD. <br><br>

![image](https://user-images.githubusercontent.com/30980904/149446757-a82dd7ae-3442-40bd-a407-86a6a929e2f6.png) <br>

Search for <b>tmc_smart</b> and scroll down to AXIS_IS_TMC(X).<br>
This is where we can edit the stepper driver using firmware since we have UART.<br>
In the current section of the Axis input the new current, since we use 5 drivers its the same across the board.<br>
Set X,Y,Z,E0(Z2) Current and Microsteps. <br>

![image](https://user-images.githubusercontent.com/30980904/149712652-4f0bd6d5-c1fd-42d8-97da-770e7ceb7062.png) <br>

<h4><b>Optional</b></h4>

<b>Enable</b> stealthchop for less noise from the motors. <br>

![image](https://user-images.githubusercontent.com/30980904/149713047-1bf5ce15-d90e-431f-9098-bc56bf9c8db9.png) <br>

<b>Enable</b> <b>HYBRID_THRESHOLD</b> for stealthchop to only active during certain speeds.<br>

![image](https://user-images.githubusercontent.com/30980904/149713315-d73889d0-f168-49ff-bfe6-2759b465471f.png) <br>

For debugged on the printer <b>enable</b> <b>TMC_DEBUG</b> & <b>MONITOR_DRIVER_STATUS</b>. <br>
Depending on your TMC2208 driver you can have a issue with the MONITOR_DRIVER_STATUS, if so the only option so far is to disable it.<br>

![image](https://user-images.githubusercontent.com/30980904/149713466-418ae3a4-660c-4a11-bcf0-f369d66185c2.png) <br>

![image](https://user-images.githubusercontent.com/30980904/149713532-544a719e-d3bb-4fc2-b90f-f3d49b37d907.png) <br>

<h4><b>Final</b></h4>

The MKS_GEN_V1.4 uses the <b>pins_RAMPS.h</b> config for its pin layout, so open it up and search for <b>Z2_SERIAL_TX_PIN</b> <br>
Your going to have to assign these pins to the pinouts you want to use. I use <b>58</b> for <b>TX</b> and <b>57</b> for <b>RX</b>. <br>

![image](https://user-images.githubusercontent.com/30980904/149715070-dfaf0622-032a-466e-850d-6e2c133a6507.png) <br>

<h2>Congratulations</h2>

Nice! You just finished both the wiring and firmware, now just build/compile and load it into your board. <br>

<h2>DataSheet | References</h2><br>

TMC2208 : <br>
https://www.trinamic.com/fileadmin/assets/Products/ICs_Documents/TMC220x_TMC2224_Datasheet_Rev1.10.pdf <br>

https://cdn-3d.niceshops.com/upload/file/TMC2208-V3.0_manual.pdf <br>

https://www.youtube.com/watch?v=7VHwcEroHPk&t=550s <br>

DataSheet: https://trinamic.com/fileadmin/assets/Products/ICs_Documents/TMC220x_TMC2224_datasheet_Rev1.09.pdf <br>

Pinouts for the MKS_GEN_V1.4<br>
![image](https://user-images.githubusercontent.com/30980904/148620455-03ec13d7-c8cf-4827-8ac7-2358e2bd6681.png) <br>

![image](https://user-images.githubusercontent.com/30980904/150173916-ae232595-a13a-4cb8-b68f-a0d6332b4edf.png) <br>

https://reprap.org/wiki/MKS_GEN

https://www.roboter-bausatz.de/media/pdf/0b/f0/a7/MKS-Gen-DataSheet.pdf <br>

TMC2208 Setup SKR
SKR : https://www.instructables.com/TMC2208-UART-on-BigTreeTechBIQU-SKR-V11-and-V13-Co/ <br>
UART : https://www.youtube.com/watch?v=sSO3Xd0a0Z8 <br>
BigTree TMC: https://github.com/bigtreetech/BIGTREETECH-TMC2208-V3.0/blob/master/TMC2208-V3.0%20manual.pdf <br>

ANet A8 : <br>
https://caggius.wordpress.com/anet-a8-hardware-specifications/ <br>

https://caggius.wordpress.com/tmc-steppers-myths-and-configuration/#:~:text=Maximum%20Peak%20Current%20for%20the%20ANet%20Motor%20is,%28x1.414%29%20and%20you%20get%20a%20VRef%20%3D%200.74V <br>

https://www.instructables.com/Anet-A8-Plus-Update-Board-to-MKS-GEN-14-TMC2208-3D/ <br>

https://www.instructables.com/UART-This-Serial-Control-of-Stepper-Motors-With-th/ <br>

https://github.com/bigtreetech/BIGTREETECH-TMC2208-V3.0 <br>

Marlin : <br>
https://marlinfw.org/docs/configuration/configuration.html <br>

https://github.com/MarlinFirmware/Marlin/issues/9221 <br>

<h1> Items from amazon </h1>

<h3>Main</h3>

Anet A8 plus : <br>
https://www.amazon.com/dp/B07MVVNXH4?psc=1&ref=ppx_yo2_dt_b_product_details

KINGPRINT MKS_GEN_V1.4 (Mega2560) : <br>
https://www.amazon.com/gp/product/B072ZZ3YQW/ref=ask_ql_qh_dp_hza

BIGTREE TMC2208 V3.0 UART Stepper Motors Driver  : <br>
https://www.amazon.com/dp/B07WGCF3L2?ref=ppx_yo2_dt_b_product_details&th=1 <br>
DataSheet <br>
https://github.com/bigtreetech/BIGTREETECH-TMC2208-V3.0/blob/master/TMC2208-V3.0%20manual.pdf <br>

LCD Makerbase MKS Mini12864 V3 : <br>
https://www.amazon.com/dp/B09BKX8XC8?ref=ppx_yo2_dt_b_product_details&th=1

<h3>Extra</h3> 

Printer Cable : <br>
https://www.amazon.com/UGREEN-Printer-Scanner-Brother-Lexmark/dp/B00P0FO1P0/ref=sr_1_3?crid=26CNRILLD87Q8&keywords=printer%2Bcable&qid=1641602219&sprefix=printer%2Bcabl%2Caps%2C103&sr=8-3&th=1

3D Touch Probe : <br>
https://www.amazon.com/dp/B0821314T9?psc=1&ref=ppx_yo2_dt_b_product_details

Jumper Wires : <br>
https://www.amazon.com/dp/B07GD2BWPY?ref=ppx_yo2_dt_b_product_details&th=1

Heat Shrink Tubing : <br>
https://www.amazon.com/dp/B01MFA3OFA?psc=1&ref=ppx_yo2_dt_b_product_details

Ohm Resistors : <br>
https://www.amazon.com/dp/B08FD1XVL6?psc=1&ref=ppx_yo2_dt_b_product_details

Multimeter : <br>
https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/ref=sr_1_2?crid=9ZEU2UXD28D6&keywords=multimeter&qid=1641597638&sprefix=multimeter%2Caps%2C94&sr=8-2

