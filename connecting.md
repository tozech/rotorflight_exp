Problem: Access serial port in Linux (Ubuntu)
---------------------------------------------
Did: Open rotorflight, connect flight controller (fc) via USB, press connect.
Result: Got error message "Failed to open serial port"
Solution:

Find serial port of flight controller (fc)

$ dmesg | grep tty
[    0.000000] console [tty0] enabled
[  192.093923] cdc_acm 2-1.2:1.0: ttyACM0: USB ACM device

View permissions

$ ls -l /dev/ttyACM0
crw-rw---- 1 root dialout 166, 0 Aug 15 21:43 /dev/ttyACM0

Become part of group dialout

$ sudo usermod -aG dialout tzech

Problem: Wrong version of configurator
--------------------------------------
Did: Connected to fc
Result: Warning "This firmware (currently on the fc) is not supported. Please use a firmware version that is supported by this version of the configurator. Use the CLI for backup before flashing. CLI backup/restore procedure is in the documentation. Alternatively download and use an old version of the configurator if you are not ready to upgrade.