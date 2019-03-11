# SAFMC_2019
Documentation of software problems 

# 1. Sensors documented 
**ST VL53L0X / VL53L1X Lidar**
- TLDR: DOes not work with Pxf but it works with pixHawk
- Useful Link: http://ardupilot.org/copter/docs/common-vl53l0x-lidar.html
**Benewake TFmini / TFmini Plus lidar**
- TLDR: works with both Pxf and pixHawk
- Useful Link: http://ardupilot.org/copter/docs/common-benewake-tfmini-lidar.html 
  Note: Don't trust the RNGFND_TYPE. 
- Useful Link: https://www.elecrow.com/download/TF-MINI-LIDAR-USER-MANUAL.pdf
  Page 24-26 Talks about connecting the lidar to pixHawk 
  Note: It would be useful to test with their GUI with an FTDI. Personally, the arduino code did not work (I tried it on 2 arduino unos)
  (https://www.lazada.sg/products/33v-55v-ft232rl-ftdi-usb-to-ttl-serial-adapter-module-for-arduino-mini-port-i286641088-s459934864.html?spm=a2o42.searchlist.list.5.5fbc1dbbA28R3Y&search=1)[link to FTDI] feel free to search for another FTDI if needed 
  
# 2. ST VL53L0X / VL53L1X Lidar
## 2.1 Connecting with pixHawk
**Preparing the pixHawk**
1. At the point of writing this, I tried the firmware version of 3.7. It can be found (here)[http://firmware.ardupilot.org/Copter/2019-03/2019-03-11-05:03/Pixhawk1/]. I flashed this firmware of PixHawk1, yes 1 on both the pixHawk1 and the pixHawk2 
  - Note: I downloaded the .apj file and used mission planner to upgrade the custom firmware. `Initial set-up` $\rarr$ `Install Firmware` $\rarr$ `Load Custom Firmware`
2. Connect according to the link provided on the Ardupilot documentation 
3. Setting the parameters 
```
RNGFND_TYPE = 16 (VL53L0X)
RNGFND_ADDR = 41 (I2C Address of lidar in decimal). The sensor’s default I2C address is 0x29 hexademical which is 41 in decimal.
RNGFND_SCALING = 1
RNGFND_MIN_CM = 5
RNGFND_MAX_CM = 120 for the VL53L0X, 360 for the VL54L1X. This is the distance in cm that the rangefinder can reliably read.
RNGFND_GNDCLEAR = 10 or more accurately the distance in cm from the range finder to the ground when the vehicle is landed. This value depends on how you have mounted the rangefinder.
```
Note that an additional parameter that I changed was `EK2_ALT_SOURCE`. I changed it to value of 1 which is for `Use Range Finder`
4. Write the parameters and disconnect the pixHawk and connect again
5. In the `Flight Data` tab, navigate to `Status` and look for `sonarrange` and see the values change 
6. This method worked for both the pixHawk1 and pixHawk2(cube)

## 2.2 Connecting with pixFalcon 
1. At the point of writing this, in the Ardupilot firmware page, there was no .apj firmware file to download. I used 3.6.7 available on mission planner 
2. I connected the lidar to the I2C port similar to how it was connected for the pixHawk. 
Link to pin-out: https://www.rcgroups.com/forums/showthread.php?2469975-Mini-PX4-PIXFALCON/page23
Link to Image: https://www.getfpv.com/holybro-pixfalcon-osd-gps-combo.html#gallery-3
![](https://www.getfpv.com/holybro-pixfalcon-osd-gps-combo.html#gallery-3)
3. I then keyed in the same params into the pixHawk
4. To ensure that I did not key in anything wrongly, I loaded the parameters from the pixHawk to the pixFalcon (This means saving the params locally and then choosing compare parameters)
5. Write the parameters and disconnect the pixHawk and connect again
6. In the `Flight Data` tab, navigate to `Status` and look for `sonarrange` which did not produce any results for me 

# 3. Benewake TFmini / TFmini Plus lidar (SERIAL)
## 3.1 Connecting with pixHawk
**Preparing the pixHawk**
1. At the point of writing this, I tried the firmware version of 3.7. It can be found (here)[http://firmware.ardupilot.org/Copter/2019-03/2019-03-11-05:03/Pixhawk1/]. I flashed this firmware of PixHawk1, yes 1 on both the pixHawk1 and the pixHawk2 
  - Note: I downloaded the .apj file and used mission planner to upgrade the custom firmware. `Initial set-up` $\rarr$ `Install Firmware` $\rarr$ `Load Custom Firmware`
2. Connect according to the link provided on the Ardupilot documentation (Electronics)
Note: Tx to Rx and vice versa 
3. Setting the parameters 
```
AVOID_MARGIN = 2
SERIAL2_PROTOCOL = 9 
SERIAL2_BAUD = 115
RNGFND_TYPE = 8 
RNGFND_SCALING = 1 
RNG_MIN_CM = 30
RNG_MAX_CM = 1200
RNGFND_GNDCLEAR = 15
RNGFND_ORIENT = 25
PRX_TYPE = 0 
PR_YAW_CORR = 22
EK2_ALT_SOURCE = 1
```
NOTE: THE DIFFERENCE BETWEEN THE PDF DOCUMENTATION AND THE ARDUPILOT WEBSITE IS THE RNGFND_TYPE. THE VALUE OF 8 WORKS BUT 20 DOES NOT WORK
4. Write the parameters and disconnect the pixHawk and connect again
5. In the `Flight Data` tab, navigate to `Status` and look for `sonarrange` and see the values change 
6. This method worked for both the pixHawk1

## 3.2 Connecting with pixFalcon 
1. At the point of writing this, in the Ardupilot firmware page, there was no .apj firmware file to download. I used 3.6.7 available on mission planner 
2. I connected the lidar to GPS port. It is a 6 pin port of which serial only requires 4 of the ports 
3. I then keyed in the same params into the pixHawk
4. To ensure that I did not key in anything wrongly, I loaded the parameters from the pixHawk to the pixFalcon (This means saving the params locally and then choosing compare parameters)
5. Write the parameters and disconnect the pixHawk and connect again
6. In the `Flight Data` tab, navigate to `Status` and look for `sonarrange` and see the values change 
