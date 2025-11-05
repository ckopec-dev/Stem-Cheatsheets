
# Arduino Cheatsheet

## IDE Installation

### Install on Unbuntu

- Download [AppImage](https://downloads.arduino.cc/arduino-ide/arduino-ide_2.1.0_Linux_64bit.AppImage).  
- Right-click on downloaded file. Click Properties, then Permissions tab. Check "Allow executing file as program". Close Properties windows.
- Double-click downloaded file to launch.

### Basic test to verify everything is working correctly

- Connect Arduino device to PC with USB cable.
- Launch IDE.
- From Tools menu, select Board and then choose the relevant device that's plugged in (e.g. Arduino UNO)
- From Tools menu, select Port and then choose the relevant port (e.g. /dev/ttyUSB0)
- From File menu, select Examples/01.Basics/Blink.
- From Sketch menu, select Upload.
- If everything has gone well, the green onboard LED should now be blinking.

## Coding

### Barebones example

~~~cpp
void setup()
{
    // Runs once when device is powered on, or has been programmed.  
}

void loop()
{
    // Runs after setup finishes, and then repeatedly.
}
~~~

### Multiline comment

`/* This comment can extend across multiple lines */`

### Initialize a pin as either an input or output

`pinMode(13, OUTPUT);`

### Turn pin power on (+5V)

`digitalWrite(13, HIGH);`

### Turn pin power off

`digitalWrite(13, LOW);`

### Sleep for a second

`delay(1000);`
