
-----

**Install Accelerometer** 

Update package index

$ `sudo apt update`

Install Dependencies

$ `sudo apt install binutils-arm-none-eabi libnewlib-arm-none-eabi libstdc++-arm-none-eabi-newlib gcc-arm-none-eabi`

Install numpy, matplotlib, and LibAtlas (linear algebra software)

$ `sudo apt install python3-numpy python3-matplotlib libatlas-base-dev`

Install numpy to our Klipper python virtual env

$ `~/klippy-env/bin/pip install -v numpy`

install Klipper MCU service to Systemd to autostart

$ `sudo cp ~klipper/scripts/klipper-mcu.service /etc/systemd/system/`


Flash KlipperMCU
```
cd ~/klipper
make clean
make menuconfig # select "linux process"
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
