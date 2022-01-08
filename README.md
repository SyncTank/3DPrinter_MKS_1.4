# 3D Printer_MKS_1.4

Setting up MKS_GEN_V1.4 board, using the Anet A8 plus as a base for the modifications

<h2>Wiring</h2>
Comming from the base of a Anet A8 plus most of the wires you can just plug in. <br>

![image](https://user-images.githubusercontent.com/30980904/148625604-0b005473-f4f0-4b00-a7c4-4caa88d126ff.png)

![image](https://user-images.githubusercontent.com/30980904/148626779-cb2ca516-b31d-4285-9a10-1c3d6408c83b.png)


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

<h2>DataSheet | References</h2><br>

TMC2208 : https://www.trinamic.com/fileadmin/assets/Products/ICs_Documents/TMC220x_TMC2224_Datasheet_Rev1.10.pdf <br>

Pinouts for the MKS_GEN_V1.4<br>
![image](https://user-images.githubusercontent.com/30980904/148620455-03ec13d7-c8cf-4827-8ac7-2358e2bd6681.png)

TMC2208 Setup SKR
SKR : https://www.instructables.com/TMC2208-UART-on-BigTreeTechBIQU-SKR-V11-and-V13-Co/

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

