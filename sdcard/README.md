
# What to do with this directory

The APM firmware requires a Micro SD card formatted with the FAT32 filesystem.

Insert the Micro SD card into your computer, and copy the "px4io.bin" file from
this directory to the root directory of the Micro SD card filesystem. Dismount
the Micro SD card and insert it into the Pixhawk.

# Triggering PX4IO firmware upgrade

Prepare the FMU processor to upgrade the IO processor firmware:
- APM software loaded on the Pixhawk FMU processor
- Micro SD card inserted
- Arming switch plugged into Pixhawk

With the Pixhawk powered off, hold down the mrming switch. Power the Pixhawk
on (via USB or otherwise) while still holding the arming switch. The amber
LED on the IO side marked B/E should blink rapidly, and remain blinking
rapidly for longer than the B/E light on the FMU side does. After the FMU B/E
light goes off, the APM Pixhawk firmware should detect that the PX4IO is in
bootloader mode upgrade the PX4IO firmware, and set the PX4FMU's tri-color LED
to solid white.

When the firmware upgrade is complete, the IO ACT blue led will blink slowly,
and the FMU tri-color LED will start blinking in various different colors.

# Confirming firmware upgrade was successful

Upload the SMACCM standalone-flight firmware to the FMU. Connect a USB-Serial
cable to the TELEM1 port on the Pixhawk and start a gcs server with 57600 baud.

Make sure the PPM receiver is connected to the Pixhawk. Turn the RC transmitter
on and make sure the PPM receiver LED is a rapidly blinking green.

Open a new terminal and enter the command

```
watch -n 0.5 "curl -s localhost:8080/controllable_vehicle_i/px4io_state | python
-m json.tool"
```

The fields for pitch, roll, switch1, switch2, throttle, and yaw in the "rc_in"
dictionary should always be between 1100 and 1900. They should change when you
move the joysticks, mode switch, and deadman switch on the RC transmitter.


