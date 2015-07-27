
# Uploading the APM firmware

The included `px_uploader.py` python program should be used to upload firmware
to the Pixhawk FMU processor. The Pixhawk FMU processor will run a USB
bootloader for 5 seconds after booting.

On the mac, the USB bootloader typically enumerates as /dev/cu.usbmodem1.
On a linux computer, the USB bootloader typically enumerates as /dev/ttyACM0.
The trailing integer device numbers may change depending on what else is plugged
into your computer.

Run the `px_uploader.py` to upload the `px4fmu-v2_APM.px4` image provided in
this directory:

```
python px_uploader.py --port /dev/cu.usbmodem1 px4fmu-v2_APM.px4
```

changing the argument to `--port` to the correct serial port for your system.

You can start the `px_uploader.py` program before the Pixhawk has been plugged
via USB, and it will connect to the USB bootloader as soon as the device
enumerates. As an alternative to physically plugging the Pixhawk in to activate
bootloader mode, you can also use a pen to push the reset button for the FMU,
located on the side of the plastic case to the left of the USB port.

