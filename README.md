# GTuner-TitanTwo
GPC Scripts for the GTuner TitanTwo

## Steam Controller to Splatoon 3 (Nintendo Switch)

The Steam controller can normally be recognized by the TitanTwo, but some modifications to the touchpads and gyro need to be made for optimal control.

### Device Configuration

Refer to GTuner's guide for details: https://www.consoletuner.com/wiki/index.php?id=t2:usage_guides:systems:switch

Adjust input protocol. The Steam Controller's polling rate is 6ms, so the TitanTwo needs to compensate to give its best performance.

Adjust output protocol. The expected polling rate of USB Switch controllers is 8ms.

**!** Make sure that Wired USB Communication is enabled in the Switch System Settings before plugging the TitanTwo into the console.

### Import the Script

### Configure the Script

**SEGTIME** 
defines the interval, in milliseconds, to cycle between the letts on the TitanTwo's LED display. Increase the value to make each letter stay on longer.

**SENSITIVITY** 
is a multiplier scale to be placed at the end before sending out right-stick output.

**DEADZONE** 
is used as a radius in the center of the touchpad inside whose circle no output is sent. Increase the value to make the space bigger.

**MIN_(LEFT/RIGHT)** 
is the smallest value sent out to the console from intended stick input. It's used as the smallest value clamped to the output. Decrease the value but keep above 0 to allow capability of fine readings around 0.

**OUTDEAD** 
is the largest value on the touchpad input that will send maximum analog output. Touchpad extremeties may vary per Steam Controller, so tune this value to the largest value seen when circling around the outer touchpad edge, including the diagonals.

**gyroMultiply** 
is a scaling factor applied to the Yaw and Roll axes of the gyro before being sent to output. [^1][^2]

**leftLeanPoint** 
is the point in degrees that the roll accelerator of the controller reaches before modifying less known buttons.
  
[^1]: The Steam Controller's yaw and roll readings on the gyro corresponds to half of the expected value of a Switch Pro Controller's gyro. The recommended value for **gyroMultiply** is 2.0 to preserve common behavior between these controllers, but adjusting this value by the decimal before changing the game's motion sensitivity settings can alter performance based on preference; increase the value to be able to look more in game with the same amount of motion on controller.

[^2]: Splatoon's gyro ratio (output:input) is about 1.8:1 on default settings. Max possible with tinkering motion settings is 3:1, such that one full rotation on the yaw results in three full in-game turns. Lowest possible while only modifying in-game motion settings is 1:1, meaning that camera movement is identical to real-life gyro's movements. But because the Steam gyro has less polling, it's possible to tune this to 0.5:1.

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
