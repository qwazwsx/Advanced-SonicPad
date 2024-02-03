# Advanced-SonicPad


**Why install SonicPad-Debian**
The Sonic Pad runs Tina Linux, a forked version of OpenWRT made by Allwinner, the SOC manufacturer for the Sonic Pad. The Sonic Pad runs modified versions of popular open source software like Klipper, Moonraker, and Klipper Screen. This is great because it makes it "just work" for their one particular use case, but offers little in terms of customization for advanced users.

The SonicPad-Debian project is a fork of Debian Linux designed to run on the sonic pad. It comes preinstalled with mainline, unmodified 3d printing software like Klipper, Mainsail, and Klipper Screen.  

In order to install this we are going to need to download the new firmware, flash it to the sonic pad, and then do some configuration afterwards.


Download Page links (direct download links are provided below):
[Creality OG Firmware Download](https://www.creality.com/pages/download-creality-sonic-pad) (in case you ever want to go back)
[SonicPad Debian GitHub Releases Page](https://github.com/Jpe230/SonicPad-Debian/releases)
[Creality SonicPad Firmware GitHub Page - PhoenixSuit Firmware Install Tool](https://github.com/CrealityOfficial/Creality_Sonic_Pad_Firmware)


Download three files from the SonicPad Debian GitHub Releases Page:
- [`t800-sonic-debian.zip`](https://github.com/Jpe230/SonicPad-Debian/releases/download/v0.0-bullseye1/t800-sonic-debian.zip)
- [`t800-sonic-debian.z01`](https://github.com/Jpe230/SonicPad-Debian/releases/download/v0.0-bullseye1/t800-sonic-debian.z01)
- [`t800-sonic-debian.z02`](https://github.com/Jpe230/SonicPad-Debian/releases/download/v0.0-bullseye1/t800-sonic-debian.z02)
If you haven't seen it before, this is a multi part zip file. That is, the contents of the zip are spread out throughout the three files and you need all three to extract the full contents. 

Unzip the firmware, you will find a file called `t800-sonic-debian.img`

This subsection of the tutorial has an Official Video Tutorial from Creality, as well as official written guides for Windows, MacOS, and Ubuntu. Please See:
- [`YouTube: Creality Sonic Pad System Restore Tutorial`](https://www.youtube.com/watch?v=i0iNu-1b6NI)
- [`Sonic Pad Firmware Burning Tutorial _Windows_V1.1.pdf`](https://raw.githubusercontent.com/qwazwsx/Advanced-SonicPad/main/Official%20Firmware%20Update%20Tutorials/Sonic%20Pad%20Firmware%20Burning%20Tutorial%20_Windows_V1.1.pdf)
- [`Sonic Pad Firmware Burning Tutorial_MacOS_V1.3.pdf`](https://raw.githubusercontent.com/qwazwsx/Advanced-SonicPad/main/Official%20Firmware%20Update%20Tutorials/Sonic%20Pad%20Firmware%20Burning%20Tutorial_MacOS_V1.3.pdf)
- [`Sonic Pad Firmware Burning Tutorial_Ubuntu_V1.1.pdf`](https://raw.githubusercontent.com/qwazwsx/Advanced-SonicPad/main/Official%20Firmware%20Update%20Tutorials/Sonic%20Pad%20Firmware%20Burning%20Tutorial_Ubuntu_V1.1.pdf)

Download [`PhoenixSuit_Windows_V1.10.zip`](https://github.com/CrealityOfficial/Creality_Sonic_Pad_Firmware/raw/main/tools/PhoenixSuit_Windows_V1.10.zip) from the "tools" directory in the Creality Sonic Pad Firmware GitHub page.  Unzip the file and enter the `PhoenixSuit_V1.10` directory. Open the `PhoenixSuit.exe` file. After a few moments, PhoenixSuit will open.


![image](ImageResources/PhoenixSuit.png)

Navigate to the "Firmware" tab. Click "Image" and select the newly extracted `t800-sonic-debian.img` file. 

You can leave the software be for now.


![image](ImageResources/USBPort.png)

Connect one end of a [**USB A Male to USB A Male**](https://www.amazon.com/s?k=usb+a+male+to+usb+a+male) cable into the bottom port labeled "CAM" on the back of the Sonic Pad. Plug the other end into your computer.


![image](ImageResources/BurningModePin.png)

Put the Sonic Pad into "Burning Mode" by pressing and holding down the outermost recessed button with a small tool while turning the Sonic Pad on. The screen will remain black, but light on the side will still illuminate.

Next, we need to install the PhoenixSuit drivers in order to communicate with the SonicPad

![image](ImageResources/DeviceManager.png)

Open **Device Manager**

Navigate to "Other Devices" and look for the item listed as "Unknown Device". If you want to confirm that this is indeed the Sonic Pad, disconnect the USB cable and check to see if the device listing disappears. 

**Right click the Unknown Device** and select **Update Driver**

![image](ImageResources/UpdateDriver.png)

Select **"Browse my computer for driver software"**

![image](ImageResources/UpdateDriver2.png)

Select **"Browse"** and select the subdirectory "Drivers" from the Phoenix Suit zip file you downloaded earlier.

Click **Next**, the drivers will be installed, and the PhoenixSuit software should pop up. If it doesn't, try restarting the Sonic Pad and re-entering Burning Mode.  

![image](ImageResources/PhoenixSuit2.png)

Once PhoenixSuit has popped up, click "Yes" to the prompt.

![image](ImageResources/PhoenixSuit3.png)

The firmware should now begin to flash, **DO NOT REMOVE POWER OR USB CONNECTION**

![image](ImageResources/PhoenixSuit4.png)

The new firmware has now been installed! We just need to set some things up first.

Once it's done, restart the Sonic Pad, you should see the Debian logo (red swirl) while the device is booting.

**Connect to WiFi**
Using the touchscreen, select **Menu > Network** and connect to your WiFi network (or use Ethernet). Take note of the internal IP address your Sonic Pad is assigned after connecting to your WiFi. 

If you don't already have an SSH client installed, get PuTTY
[PuTTY x86 Binary Download .exe](https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe)
[PuTTY download page](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

**Connect to your SonicPad using SSH**
$ `ssh sonic@<your-sonic-pad-ip>`
The default password is `pad`. For security, no characters will show up when typing the password. You may have to type "yes" to confirm connecting.

**Resize filesystem**
Since the image we flashed to the Sonic Pad was only 3 GB, we need to expand this 3 GB filesystem to the full 8 GB of the drive. 

$ `sudo resize2fs /dev/mmcblk0p5`

Command Breakdown
`sudo` run the following command as root
`resize2fs` the filesystem resizer tool
`/dev/mmcblk0p5` the root partition of our drive

You can examine the filesystem size before and after the operation using the command `df -h`

**Update System**
$ `sudo apt-get update && sudo apt-get upgrade`

Command Breakdown:
As root, update all package index files, and then upgrade all packages

Install 3D Printing Software
SonicPad-Debian comes preinstalled with KIAUH the Klipper Installation And Update Helper. This will allow you to easily install or remove software like Klipper, Moonraker, Mainsail, Fluid, Crowsnest, and others. You can open KIAUH using the following command

$ `~/kiauh/kiauh.sh`

Command breakdown:
Runs the executable located at `/home/sonic/kiauh/kiauh.sh`

![image](ImageResources/KIAUH.png)

Follow the onscreen instructions to install the software you want.

*Klipper* - talks directly with the printer
*Mainsail* - provides an API for Klipper, so other software can talk to it
*Moonraker* - web frontend for printer control (install one or both)
*Fluidd *- web frontend for printer control (install one or both)
KlipperScreen - provides GUI for Sonic Pad screen

**Configure Software**
You should be able to configure the software you have installed by editing config files located in the  `/home/sonic/printer_data/config`

If you are experiencing and problems you can view the log files located at `/home/sonic/printer_data/config`


------

Optional (but helpful) addon steps:
- **Adding KlipperScreen to Moonraker's Update Manager**
- **Fix System Clock**
- **Install Accelerometer** 
- **Turn off display after inactivity**

**Adding KlipperScreen to Moonraker's Update Manager**
Moonraker has an update manager which makes keeping your system up to date really easy. To add KlipperScreen to this list, add the following lines to the end of `moonraker.conf`. You should then be able to update KlipperScreen via the web interface.

```
[update_manager KlipperScreen]
type: git_repo
path: ~/KlipperScreen
origin: https://github.com/jordanruthe/KlipperScreen.git
env: ~/.KlipperScreen-env/bin/python
requirements: scripts/KlipperScreen-requirements.txt
install_script: scripts/KlipperScreen-install.sh
managed_services: KlipperScreen
```

-----

**Fix System Clock**
To see the list of all available time zones:

`timedatectl list-timezones | more`

To set the timezone:
$ `timedatectl set-timezone 'America/Chicago'`

To verify you the currently set time zone
$ `timedatectl`

Reboot for the changes to take effect

-----

**Install Accelerometer** 

$ `sudo apt update`
Update package index

$ `sudo apt install python3-numpy python3-matplotlib libatlas-base-dev`
Install numpy, matplotlib, and LibAtlas (linear algebra software)

$ `~/klippy-env/bin/pip install -v numpy`
Install numpy to our Klipper python virtual env

$ `sudo cp ~klipper/scripts/klipper-mcu.service /etc/systemd/system/`
install Klipper MCU service to Systemd to autostart

```
cd ~/klipper
make clean
make menuconfig
make flash
```

Add the following to the `printer.cfg`
```
[mcu rpi]
serial: /tmp/klipper_host_mcu

[adxl345]
cs_pin: rpi:None
spi_speed: 2000000
spi_bus: spidev2.0

[resonance_tester]
accel_chip: adxl345
accel_per_hz: 70
probe_points:
      117.5,117.5,10
```

-----

**Turn off display after inactivity**
By default, the screen will blank after inactivity, but the backlight will remain on. This script, by [Calculous](https://github.com/calculous/SonicPad-Debian) on GitHub, remedies this.

Create a new file called `display-sleep.sh` in your home directory with the following contents:

```
#!/bin/bash
 
#config
delay=2 # repeat every 2 seconds
 
# init
 
read dummy dummy  former_state < <(xset -display :0 -q | grep "Monitor is ")
echo $former_state
sleep $delay
 
#loop
while true; do
  read dummy dummy state < <(xset -display :0 -q | grep "Monitor is ")
  echo $state
  [ "$state" = "On" ] || state="Off" # squeeze away suspend/standby, monitor is off
  if [ "$state" != "$former_state" ]; then
    if [ "$state" = "On" ]; then
        brightness -s 1
    else
        brightness -s 0
    fi
    former_state="$state"
  fi
sleep $delay
done
```

You can use the command $ `nano ~/display-sleep.sh` to open the file with the nano text editor. Use CTRL+X to close and save.  

$ `chmod +x ~/display-sleep.sh`
Mark the file as executable

Next, we're going to create a Systemd service to keep this script running. Create a file at `/etc/systemd/system/display-sleep.service` with the following contents:
```
[Unit]
 
Description=Display Sleep
 
After=default.target
 
[Service]
 
ExecStart=/home/sonic/display-sleep.sh
 
[Install]
 
WantedBy=default.target
```
 $ `sudo nano /etc/systemd/system/display-sleep.service`

Enable the service
$ `sudo systemctl start display-sleep.service`
$ `sudo systemctl enable display-sleep.service`

-----

