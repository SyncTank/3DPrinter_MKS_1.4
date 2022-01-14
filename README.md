# 3D Printer_MKS_1.4

Setting up MKS_GEN_V1.4 board, using the Anet A8 plus as a base for the modifications

<h2>Wiring</h2>
Comming from the base of a Anet A8 plus most of the wires you can just plug in. (Exceptions being End-Stop, Touch Sensors) <br><br>

![image](https://user-images.githubusercontent.com/30980904/149030575-46fe47b8-741a-4e4f-95e5-59bbe15cdc8c.png) <br>

<h3>Get the Sensor/Data line to the left with ground in the middle and V5 to the right.</h3><br>

![image](https://user-images.githubusercontent.com/30980904/148625604-0b005473-f4f0-4b00-a7c4-4caa88d126ff.png) <br>


![image](https://user-images.githubusercontent.com/30980904/148626779-cb2ca516-b31d-4285-9a10-1c3d6408c83b.png) <br>

<h3>I do this electrical tape and some jumper wires, alternatively you can remove the clipper and reclip them</h3> <br>

![image](https://user-images.githubusercontent.com/30980904/149034906-d9aa0882-5dd5-4037-8c3b-9f7f954371af.png)<br>

![image](https://user-images.githubusercontent.com/30980904/149039056-f6eee359-02f1-4bb9-8110-7a92ec691701.png)<br>

Next put in the jumper cap in the correct spot following this diagram <br>

![image](https://user-images.githubusercontent.com/30980904/149259311-b2efae1d-323f-4a7c-92b7-359e493dd743.png) <br>

For TMC2208 chips on the MKS_GEN_V1.4 its two cap on each stepper for 1/16.<br>

![image](https://user-images.githubusercontent.com/30980904/149259755-fc521aa8-2047-4d75-a4dc-a35ea7c08fa1.png) <br>

<h2>Stepper Driver (TMC2208 )</h2>

For whatever chip you get set the potentiometer to the vRef you will be working with on your motors. (Safety) <br>
This is done with the screw looking like insert on the chip, first with the printer off place the chip in the pins <br>
correct orientation is required otherwise you will burn the chip. Reference your documents if not its usually GND facing motor and pins inwards<br>

https://www.youtube.com/watch?v=k3Uc1F5jgVQ&t=35s

![image](https://user-images.githubusercontent.com/30980904/149042785-a5fbd2d1-5640-4d53-907b-e51a33d29fad.png) <br>

<h3>Potentiometer</h3> <br>

![image](https://user-images.githubusercontent.com/30980904/149039989-af1e1c65-7070-47f0-b46b-936a336d074c.png) <br>

To do this find the Rated Current of your motor which is done by googling and finding the spec sheet. <br>
Most of the time you will have a label on the motor google this i.e <br>

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

If your using SKR board reference the SKR in datasheets as that board is the only one I know that supports single wire UArt <br>
For the rest of us connect two jumper wires on the PDN & UART connection on the board. <br>

![image](https://user-images.githubusercontent.com/30980904/149041512-5acfe1b2-9c58-4d05-a1d6-510ca2934222.png) <br>

One of the wires will need a 1k Ohm resistor on it, this can be done with protoboards, jumpers, or soldering. (Done for each stepper) <br>

![image](https://user-images.githubusercontent.com/30980904/149041622-a651139b-7dcf-4e8d-bca0-567cb96744ee.png) <br>

The <b>resistor</b> one will be our Tx (Transmit) pin, the other is the Rx (receiver) pin. <br>
You can start connecting the pins follow this diagram. <br>

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
Next go to the EndStop section and enable <b>USE_ZMAX_PLUG</b> then scroll down to <b>Z_MIN_PROBE_ENDSTOP_INVERTING</b> and make this false<br>

![image](https://user-images.githubusercontent.com/30980904/149428840-0a2cf3cd-d36f-402d-bce9-22998e30c2f4.png) <br>

![image](https://user-images.githubusercontent.com/30980904/149428994-39f80dd0-9620-4ec5-a8fa-8fe0f5889f2c.png) <br>

Make sure <b>Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN</b> is enable, use can also use set the pin yourself by scrolling down <br>

![image](https://user-images.githubusercontent.com/30980904/149429297-c5870602-200b-482f-b2a4-98cc87a4c6d2.png) <br>

Now search BLTOUCH enable <br>

![image](https://user-images.githubusercontent.com/30980904/149430682-41b8840f-1016-4129-b50f-fc74bec7ed4c.png) <br>




<h2>DataSheet | References</h2><br>

TMC2208 : https://www.trinamic.com/fileadmin/assets/Products/ICs_Documents/TMC220x_TMC2224_Datasheet_Rev1.10.pdf <br>

https://www.youtube.com/watch?v=7VHwcEroHPk&t=550s <br>

Pinouts for the MKS_GEN_V1.4<br>
![image](https://user-images.githubusercontent.com/30980904/148620455-03ec13d7-c8cf-4827-8ac7-2358e2bd6681.png)

TMC2208 Setup SKR
SKR : https://www.instructables.com/TMC2208-UART-on-BigTreeTechBIQU-SKR-V11-and-V13-Co/ <br>
UART : https://www.youtube.com/watch?v=sSO3Xd0a0Z8 <br>

ANet A8 : <br>
https://caggius.wordpress.com/anet-a8-hardware-specifications/ <br>

https://caggius.wordpress.com/tmc-steppers-myths-and-configuration/#:~:text=Maximum%20Peak%20Current%20for%20the%20ANet%20Motor%20is,%28x1.414%29%20and%20you%20get%20a%20VRef%20%3D%200.74V <br>

https://www.instructables.com/Anet-A8-Plus-Update-Board-to-MKS-GEN-14-TMC2208-3D/ <br>

https://www.instructables.com/UART-This-Serial-Control-of-Stepper-Motors-With-th/ <br>

https://github.com/bigtreetech/BIGTREETECH-TMC2208-V3.0 <br>

Marlin : <br>
https://marlinfw.org/docs/configuration/configuration.html <br>

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

