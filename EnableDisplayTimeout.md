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
