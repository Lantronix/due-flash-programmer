# xPico Wi-Fi Arduino Due flash programmer

## Summary
This is a utility that talks to the SAM-BA bootloader that comes in the ROM of the Atmel SAM3X series of microcontrollers to write a binary image to flash and boot to it.

The SAM3X is the microcontroller that is in the Arduino Due, so you can use this utility to program your Due if you want to do it via a web interface.

The utility makes use of the Lantronix xPico Wi-Fi Javascript library found here (a minified version is included in this project, so you don't need to get it separately at the next link):
https://github.com/Lantronix/xpwJSlib


## Usage
## How-To

Make the following connections between an xPico Wi-Fi and the SAM3X processor:

*	“ERASE/PC0” pin on Atmel processor connected to CP1 on xPico Wi-Fi
*	“NRSTB” pin on Atmel processor connected to CP2 on xPico Wi-Fi
*	UART0 on Atmel processor connected to Line 1 on xPico Wi-Fi (cross TX/RX between the two)


Put the `xpwLib.min.js` and `due.html` files on the xPico Wi-Fi's filesystem, in the http/ directory (which you need to create).

Navigate with your browser to the ip address of the xPico Wi-Fi, to the /due.html file

At this point, the utility will assert Reset and Erase on the SAM3X processor to boot it into the SAM-BA bootloader.

![Reset screenshot](/images/reset.png)

After it is done resetting, the utility will set Line 1 on the xPico Wi-Fi to Protocol None and 115200 baud, which is necessary to use the Web to Serial function to get direct Javascript access to the serial port.

The utility will then connect to the SAM-BA bootloader to retrieve flash information, including the all-important page size.

![Information screenshot](/images/info.png)

Once the flash parameters are populated, you will be able to select a binary file to program into the flash. One way that you can create a binary image to program is to use the Arduino IDE to create your program, and then the Export compiled Binary function, as shown below.

![export](/images/arduinoExport.gif)

Now choose that file, and the utility will read the file size and calculate how many pages of flash it will take. The Upload file button will now appear. Once you click on this button, the utility will start the process of programming the binary into the flash of the SAM3X microcontroller.

![File info](/images/file.png)

During this process you can see how many bytes have been uploaded, and also as each page is written it will print out on the screen. Once the utility is done programming, it will set the boot bit to boot from Flash 0 (GPNVM[1]), and then it will assert reset to restart the SAM3X processor and start executing your program.

![Done](/images/done.png)
