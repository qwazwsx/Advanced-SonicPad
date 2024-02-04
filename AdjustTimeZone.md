-----

**Fix System Clock**
To see the list of all available time zones:

`timedatectl list-timezones | more`

To set the timezone:
$ `timedatectl set-timezone 'America/Chicago'`

To verify you the currently set time zone
$ `timedatectl`

Reboot for the changes to take effect

***Turning the power off without gracefully shutting down the Sonic Pad first will corrupt the filesystem. Do not use the power button on the side of the Sonic Pad.***

-----
