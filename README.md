# Victron VenusOs package for Waveshare RS485 CAN HAT (B)

This package adds suport for the [Waveshare RS485 CAN HAT (B)](https://www.waveshare.com/rs485-can-hat-b.htm
) to Victron VenusOs version 3.0 and newer.

## Pin assignment changes

Since two GPIOs used for VenusOS collide with the SPI-1 bus needed for the RS485 chip, the `gpio_list` file for VenusOS must be changed:

| Pin             | Default          | New              |
| --------------- | ---------------- | ---------------- |
| Relay 1         | Pin 40 / GPIO 21 | Pin 13 / GPIO 27 |
| Digital input 4 | Pin 35 / GPIO 19 | Pin 15 / GPIO 22 |

Additionally, the digital input 6 is added as required by [ShutdownMonitor](https://github.com/kwindrem/ShutdownMonitor) (Pin 36 / GPIO 16
).

## Configuration changes

The setup scripts adds the following lines to `/u-boot/config.txt`:

	#### begin CAN+RS485 overlay
    dtparam=spi=on
	dtoverlay=mcp2515-can0,oscillator=16000000,interrupt=25,,spimaxfrequency=1000000
	dtoverlay=sc16is752-spi1,int_pin=24
    #### end CAN+RS485 overlay

The two comment lines are required to find the modifications on uninstallation, so please do not remove them!

## Hardware requirements

Obviously, this package works only on Raspberry PI platforms.

Conflicts can sometimes occur when modifications to config.txt are needed.
For this reason, this script asks before installing overlays into the config.txt file.
If overlay lines are not automatically added to config.txt, it will be necessary for the installer to add the necessary lines manually.

As an aid to sorting out GPIO conflicts, I have provided a list of GPIO assignments in this package.

Setup:

VeCanSetup requires that SetupHelper is installed first.

The easiest way to install VeCanSetup is to do a "blind install" of SetupHelper and then add the VeCanSetup package via the PackageManager menus.

Refer to SetupHelper here:

https://github.com/kwindrem/SetupHelper

Once SetupHelper is installed, you can add VeCanSetup by placing the archive from its GitHub repo on a USB stick or SD card and inserting it in the GX device (CCGX, Cerbo, etc.). PackageManager will detect the archive and transfer the package to local storage. It will then appear in the Active packages list.

The VeCanSetup compressed archive can be downloaded here:

https://github.com/kwindrem/VeCanSetup/archive/refs/tags/latest.tar.gz

Another install option IF the Venus device has internet access is to run the following command:

wget -qO - https://github.com/kwindrem/VeCanSetup/archive/current.tar.gz | tar -xzf - -C /data
mv /data/VeCanSetup-current /data/VeCanSetup

If not, do the following a computer that does have internet access

click on this link in a web browser:
https://github.com/kwindrem/VeCanSetup/archive/current.tar.gz

rename the resulting .tar.gz file to venus-data.tar.gz
copy the venus-data.tar.gz to a USB stick,
put the stick in the Venus device and reboot.
When Venus boots, it will unarchive the file to /data/VeCanSetup-current

mv /data/VeCanSetup-current /data/VeCanSetup

If you are installing multiple packages, SetupHelper need only be installed once.

Once both packages are installed run setup and follow the prompts.
/data/VeCanSetup/setup

You will need root access to the Venus device. Instructions can be found here:
https://www.victronenergy.com/live/ccgx:root_access
The root password needs to be reentered following a Venus update.
Setting up an authorization key (see documentation referenced above) will save time and avoid having to reset the root password after each update.
