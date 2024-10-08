# GTuner-TitanTwo
GPC Scripts for the GTuner TitanTwo

## Steam Controller to Splatoon 3 (Nintendo Switch)

The Steam controller can normally be recognized by the TitanTwo, but some modifications to the touchpads and gyro need to be made for optimal control.

![Steam Controller](https://www.consoletuner.com/wiki/lib/exe/fetch.php?media=t2:usage_guides:controllers:steamcontroller.png) ![Switch Pro](https://www.consoletuner.com/wiki/lib/exe/fetch.php?media=t2:usage_guides:controllers:switchprojoycons.png)

### Device Configuration

Refer to GTuner's guide for details: [Nintendo Switch Guide on ConsoleTuner](https://www.consoletuner.com/wiki/index.php?id=t2:usage_guides:systems:switch)

![Device Configuration](https://github.com/DamianS-eng/GTuner-TitanTwo/blob/main/TitanTwoDeviceConfiguration.png) ![Steam Controller Input](SteamControllerInput.png) ![Switch Output](SwitchConsoleOutput.png)

Edit the input polling rate. The Steam Controller's polling rate is 6ms, so the TitanTwo needs to compensate to give its best performance.

Adjust output protocol and polling rate. The expected polling rate of USB Switch controllers is 8ms.

*! Make sure that Wired USB Communication is enabled in the Switch System Settings before plugging the TitanTwo into the console. *

![Enable Wired Communication Settings](https://www.lifewire.com/thmb/VSjLSCxSAzcyu2bG_UopT6tDofo=/750x0/filters:no_upscale():max_bytes(150000):strip_icc():format(webp)/002-how-to-connect-ps4-xbox-controllers-to-switch-4707374-aee6b470c9ca457e8b67356e0fcf1bc1.jpg)

### Import the Script

1. Connect the TitanTwo from the PROG slot with a Micro USB cable to a computer with [GTunerIV IDE](https://www.consoletuner.com/titan-two-downloads/) installed and running.

2. Download the supplied [steam.gph](https://github.com/DamianS-eng/GTuner-TitanTwo/blob/main/Steam.gph) header file, and save it in the include folder of the Gtuner installation.
Download the supplied steamtoSwitch.gpc file. [steamToSwitch.gpc](https://github.com/DamianS-eng/GTuner-TitanTwo/blob/main/SteamToSwitch.gpc)

3. Open and compile the script in GTunerIV. This is the chance to find anything wrong with the script, or adjust the configuration details.

4. Save the script to a memory slot on the TitanTwo. The cable can now be removed from the PC.

Turn on the Steam Controller and plug the USB Wireless Receiver into the left USB-A slot. The controller can also be plugged directly into this slot with a Micro USB data cable. Bluetooth is possible but not configured in the script at this time, and requires a separate bluetooth module for the TitanTwo.

Supply power to the TitanTwo and connect it from the OUTPUT slot to the Nintendo Switch. Use the arrow keys on the TitanTwo to select the number of the slot the script is stored.

### Configure the Script

**SEGTIME** 
defines the interval, in milliseconds, to cycle between the letts on the TitanTwo's LED display. Increase the value to make each letter stay on longer.

**SEGMAX**
is an unsigned integer constant for the number of letters in the word segment to be displayed on the LED. For now, the length of letters in the word to be displayed must match this number exactly.

**DELTATIME**

**TOUCHTHRESH**
is an unsigned floating number describing the analog value required to be sent by the touch input to be considered "active", between 0.0 and 100.0.

**DECAY**
is an unsigned floating number with higher precision that determines how soon the output is decremented back to 0.0. Increase this value to strengthen the decay.

**SWIPESCALE**
is an unsigned scalar multiplier applied to the swipe function which has calculated the distance between the previous and current trackpad points recorded. Use less than 1 to decrease the output, and use greater than 1 to strengthen the effect.

**POLLINGNO**

**SWIPEDECAY**,
 if TRUE, allows the output of the stick after calculating the difference between positions to decay over time to 0.0 as long as the touchpad is not touched.

 **SWIPETRACK**,
 if TRUE, allows the trackpad to be used as a mouse-like swipe required to generate output instead of the default absolute deflection given by simple touch.

 **REQUIRETOUCHGYRO**,
 if TRUE, completely disables Gyro output as long as the touchpad is not active, determined by **TOUCHTHRESH**.

 **ADAPTIVEJOYSTICK**,
 if TRUE, the output joystick is determined by swiping motions whose magnitude is determined between the absolute point where the touchpad was active and the ongoing point of continuous touch on the pad. This allows simulation of the motion of the stick at any point on the pad instead of using only specific parts of the pad as the stick's final output.
 If FALSE, the direction and magnitude of deflection of the output joystick is determined immediately by the active point on the touchpad.

**SENSITIVITY** 
is a multiplier scale to be placed at the end before sending out right-stick output.

**DEADZONE** 
is the linear radius from the center of the touchpad inside whose axis no output is sent. Increase the value to make the space of no input bigger.

**MIN_(LEFT/RIGHT)** 
is the smallest value sent out to the console from intended stick input. It's used as the smallest value clamped to the output. Decrease the value but keep above 0 to allow capability of fine readings around 0.

**OUTDEAD** 
is the largest value on the touchpad input that will send maximum analog output. Touchpad extremeties may vary per Steam Controller, so tune this value to the largest input value seen when circling around the outer touchpad edge, including the diagonals.

**gyroMultiply** 
is a scaling factor applied to the Yaw and Roll axes of the gyro before being sent to output. [^1][^2]

**leftLeanPoint** 
is the point in degrees that the roll accelerator of the controller reaches before modifying less known buttons. Useful for titles that require all the buttons on an official Nintendo Switch Pro Controller, including both stick clicks.
  
[^1]: The Steam Controller's yaw and roll readings on the gyro corresponds to half of the expected value of a Switch Pro Controller's gyro. The recommended value for **gyroMultiply** is 2.0 to preserve common behavior between these controllers, but adjusting this value by the decimal before changing the game's motion sensitivity settings can alter performance based on preference; increase the value to be able to look more in game with the same amount of motion on controller.

[^2]: Splatoon's gyro ratio (output:input) is about 1.8:1 on default settings. Max possible with tinkering in-game motion settings is 3:1, such that one full rotation on the yaw results in three full in-game turns. Lowest possible while only modifying in-game motion settings is 1:1, meaning that camera movement is identical to real-life gyro's movements. But because the Steam gyro has less polling, it's possible to tune this to 0.5:1 while adjusting the Steam Controller's gyro sensitivity.

### Functions Explained

**segDis**
changes the LED segment display on the TitanTwo. For now, it will change based on USB protocol plugged into TitanTwo. Reinforce that this script is meant for Steam Controller.

**segDisRepeat**
is based off of **segDis**, but cycles through letters in defined arrays of characters after **SEGTIME** passes.

**isPressed**
sense when the touchpad is touched, not just clicked. For left touchpad, the last analog input values persist even after releasing from touch, so analog output needs to be reset to 0 when not sensed to simulate stick return-to-center.

**outDeadZone**
modifies the inner and outer deadzone on the touchpad. The full range cannot be currently achieved due to irregular mapping and clamped maximum values as the focus of the ouput.

**TrackpadTrace**
is currently experimental. It's meant to properly recognize horizontal swipes and output to something useful for Splatoon, such as spinning the camera. Adjusting the right stick output is similar to Steam's Mouse-Like-Joystick, which may not be fully desirable. Another idea is to scale the swipe span as an augmentation to the controller's yaw and roll values.

**buffGyro**
Alter the gyro output by a scale factor **gyroMultiply**.

**extraButtons**
uses **leanLeft** to switch stick click's function based on the tilt of the controller. Other button remappings, including the paddles, dual-stage triggers and touchpad clicks are managed here.

#### Compute the new stick output values based on a changed radial deadzone.

If both the X and Y values of a trackpad are greater than zero, the official output is the diagonal force vector, the hypotenuse of the triangle made up by the x and y input values. 

Calculate the hypotenuse by multiplying the value by the minimum deadzone and then divide by the outer deadzone, then transplated to its relative axis using arcsin or arccos.
