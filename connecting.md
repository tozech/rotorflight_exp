Problem: Access serial port in Linux (Ubuntu)
---------------------------------------------
Did: Open rotorflight, connect flight controller (fc) via USB, press connect.
Result: Got error message "Failed to open serial port"
Solution:

Find serial port of flight controller (fc)

'''
$ dmesg | grep tty
[    0.000000] console [tty0] enabled
[  192.093923] cdc_acm 2-1.2:1.0: ttyACM0: USB ACM device
'''

View permissions

```
$ ls -l /dev/ttyACM0
crw-rw---- 1 root dialout 166, 0 Aug 15 21:43 /dev/ttyACM0
```

Become part of group dialout

```
$ sudo usermod -aG dialout tzech
```

Problem: Wrong version of configurator
--------------------------------------
Did: Connected to fc
Result: Warning "This firmware (currently on the fc) is not supported. Please use a firmware version that is supported by this version of the configurator. Use the CLI for backup before flashing. CLI backup/restore procedure is in the documentation. Alternatively download and use an old version of the configurator if you are not ready to upgrade.
Solution:
Follow instructions in https://github.com/rotorflight/rotorflight/wiki/Back-up-and-restore

Click on "Clear output history"

Type "dump all" into CLI and hit enter.

Click on "Save to File". Follow file dialog.

Problem: Which version of rotorflight is currently on the fc
------------------------------------------------------------
Did: Open dumped file "backups/RTFL_cli_20230817_083449_dump_all.txt" in text editor.
Result: First lines are

```
# version
# Rotorflight / STM32F405 (S405) 4.3.0-20230724 Jul 23 2023 / 22:05:47 (967e91d) MSP API: 11.2
```

Solution: The 4.3.0-20230724 is the version of the rotorflight firmware. In this case it is currently the latest snapshot of RF2
and belongs to the rotorflight configurator of snapshot 2.0.0-20230724.

Problem: Do I need USB drivers in linux?
----------------------------------------
Did: While preparing flashing, I read that "Note: When flashing boards that have directly connected USB sockets ensure you have read the USB flashing section of the manual  and have the correct  software  and drivers installed."
Solution: Answer from discord: Linux has the drivers built-in

Problem: Hanging on flashing, 'Initiating reboot to bootloader'
---------------------------------------------------------------
Did: Flashed version 1.02, see https://github.com/rotorflight/rotorflight/wiki/Installing-Rotorflight-Firmware
Results: After clicking Flash Firmware it shows 'Initiating reboot to bootloader' and then hangs.
Solution: Add user not only to group 'dialout', but in ubuntu also to 'plugdev' 

```
$ sudo usermod -a -G plugdev <username>
```

Source https://betaflight.com/docs/development/USB-Flashing
