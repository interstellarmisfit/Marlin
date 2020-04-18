# Marlin 3D Printer Firmware

[![Build Status](https://travis-ci.org/MarlinFirmware/Marlin.svg?branch=RCBugFix)](https://travis-ci.org/MarlinFirmware/Marlin)
[![Coverity Scan Build Status](https://scan.coverity.com/projects/2224/badge.svg)](https://scan.coverity.com/projects/2224)

<img align="top" width=175 src="buildroot/share/pixmaps/logo/marlin-250.png" />

This is my fork of the Marlin firmware. Please see the official [Marlin GitHub](https://github.com/MarlinFirmware/Marlin) for additional information.

Additional documentation can be found at the [Marlin Home Page](http://marlinfw.org/).


## Ender 3 Bugfix Branch

__Not for production use. Use with caution!__

This branch is based on the official Marlin bugfix-1.1.x. The configration has been custiomized to work on an Ender 3 with a Creality board and a BLTouch v3.

I am using this firmware build on my personal Ender 3. It is working extremly well with great print quality.

It is configured with full menus, BLTouch and BiLinear leveling.

S-Curve acceleration and Junction Deviation are disabled in the config. I recommend trying S-Curve. It almost completely eliminates ringing.

I have included an option to optimize the configuration for printing from Octoprint.

HEX files are included.

## Installation notes
The firmware is preconfigured and ready to use on an Ender 3 running the stock board. 

If you have Creality Slient board, you'll need to enable the CREALITY_SILENT option in configuration.h.

The BLTouch configuration is for an AntLabs BLTouch v3.1 using the Creality mount. The extruder offsets are set based on the Creality mount.

In it's current configuration it compiles to 130000 bytes, leaving 48 free. If you have issues compiling you may need to disable a small feature. 

If you are upgrading from a different version you will have to initialize your EEPROM from the control menu. Record any custom settings such as extruder steps calibration.

Homing rates are slowed down to prevent moving past the limit switch and slamming into stops. 

## progisp users
WARNING!! The fuse values that Creality says to use are wrong. The settings do not match the 16mhz crystal oscillator that is used on the Creality boards. I have verified this against the datasheet for the Atmel chip. 

The correct fuse values, that also get burned if you use an Arduino, are: Low=FF, High=DE, Extended=FD.

## HEX Files


Marlin119-CrealityBlTouch.hex: Basic config for Creality BLTouch. 3x3 probe grid. No S-Curve acceleration

Marlin119-CrealityBLTouch-wBootLoader.hex: Basic config for Creality BLTouch including the bootloader

Marlin119-CrealityBlTouch-5x5.hex: Basic config but with a 5x5 probe grid instead of 3x3

Marlin119-CrealityBlTouch-Scurve.hex: 3x3 probe grid with S-Curve acceleration

Marlin119-CrealityBlTouch-Scurve-5x5.hex: 5x5 probe grid with S-Curve acceleration

Marlin119-CrealityBlTouch-Scurve-5x5-italian: Italian slim menus, 5x5 probe grid with S-Curve acceleration

## Non-Standard configuration options.
I have added a couple of non-standard configuration options to configuration.h.

OCTOPRINT_OPTIMIZE: If you print through the serial port using Octoprint I highly recommend enabling this one. It disables the SD Card and increases the baud rate and buffers. Make a huge difference in print quality.

CREALITY_SILENT: Sets the stepper driver types to TMC2208_STANDALONE for the slient board.

BEEPER_DISABLE: Sets the BEEPER_PIN to -1 to disable the beeper with BLTouch. Prevents the beeper from interfering with BLTouch and saves 600 bytes. THis is automatically added when BLTOUCH is defined.

## Enabled Features
I tried to fit as many useful features into the small amount of memory that I could.

    BLTOUCH
    BLTOUCH_SET_5V_MODE: Needed for v3/3.1
    MULTIPLE_PROBING 2: More accurate BLTouch probes.
    AUTO_BED_LEVELING_BILINEAR
    EXTRAPOLATE_BEYOND_GRID
    Z_SAFE_HOMING: Needed for BLTouch to home Z in middle of bed.
    RESTORE_LEVELING_AFTER_G28
    BEEPER_DISABLE: Saves 600 bytes
    PREVENT_LENGTHY_EXTRUDE: 460mm
    NO_MOTION_BEFORE_HOMING: Since there is no Z Limit switch, helps prevent crashing of head into bed if you haven't homed yet.
    MIN_SOFTWARE_ENDSTOPS: Also helps prevent head crashes.
    SOFT_ENDSTOPS_MENU_ITEM: Allows you to disable soft end stops when calibrating BLTouch Z. 
    ENCODER_PULSES_PER_STEP 4: Encoder settings are in the official Creality firmware. Seem to work well. Disable if you have encoder issues.
    ENCODER_STEPS_PER_MENU_ITEM 1
    LCD_SET_PROGRESS_MANUALLY: Add M73 to set progress. Disable if you don't print through serial port.
    BABYSTEPPING: Multiplicator 10
    EMERGENCY_PARSER:



## Disabled Features
Had to make a few sacrifices to make it all fit.

    SHOW_BOOTSCREEN: Quicker booting
    PID_AUTOTUNE_MENU: Can still use M303
    ENDSTOPPULLUPS: The Creality board has pull-up resistors. 
    QUICK_HOME: Saves 524 bytes!
    SPEAKER: Not needed on a Creality board.
    SCROLL_LONG_FILENAMES
    STATUS_MESSAGE_SCROLLING
    LCD_INFO_MENU
    AUTOTEMP: Saves 1k.
    ARC_SUPPORT
    NO_WORKSPACE_OFFSETS: Enabled to disable workspace offsets. Save 1700 bytes. 
	S_CURVE_ACCELERATION
    JUNCTION_DEVIATION: With calculated mm from accel and jerk
	
## Options and their sizes.
OPTION|PROG BYTES
:--|--:
BLTOUCH|16962
SDSUPPORT|15814
AUTO_BED_LEVELING_BILINEAR|11016
ARC_SUPPORT|3638
S_CURVE_ACCELERATION|3176
LIN_ADVANCE|3174
AUTOTEMP|1372
EEPROM_CHITCHAT|716
BABYSTEPPING|682
SHOW_BOOTSCREEN|476
E0_AUTO_FAN_PIN|404
PREVENT_LENGTHY_EXTRUDE|400
PID_AUTOTUNE_MENU|352
NO_MOTION_BEFORE_HOMING|236
USE_CONTROLLER_FAN|176
SCROLL_LONG_FILENAMES|164
EMERGENCY_PARSER|162
SOFT_ENDSTOPS_MENU_ITEM|150
JUNCTION_DEVIATION|36
DISABLE_INACTIVE_EXTRUDER false|22
RESTORE_LEVELING_AFTER_G28|14
BEEPER_PIN -1|-634
NO_VOLUMETRICS|-1554
NO_WORKSPACE_OFFSETS|-1758
SLIM_LCD_MENUS|-9660
NO_LCD_MENUS|-39038
## Recent Changes

## Credits

Please see the official [Marlin GitHub](https://github.com/MarlinFirmware/Marlin) for a full list of credits.


## License

Marlin is published under the [GPL license](/LICENSE) because we believe in open development. The GPL comes with both rights and obligations. Whether you use Marlin firmware as the driver for your open or closed-source product, you must keep Marlin open, and you must provide your compatible Marlin source code to end users upon request. The most straightforward way to comply with the Marlin license is to make a fork of Marlin on Github, perform your modifications, and direct users to your modified fork.

While we can't prevent the use of this code in products (3D printers, CNC, etc.) that are closed source or crippled by a patent, we would prefer that you choose another firmware or, better yet, make your own.
