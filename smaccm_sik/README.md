
# Upload smaccm-sik firmware

Connect your 3DR Radio (or similar HM-TRP based radio with a USB-Serial
connection) to your computer and note the serial device name. On the mac, this
will typically be something like `/dev/cu.usbserial-NNNNNNNN`, and on a linux
machine, it will typically be something like `/dev/ttyUSB0`.

The included `sik_uploader.py` program will activate the radio bootloader and
upload a new firmware image. Invoke `sik_uploader.py` as below:

```
python sik_uploader.py --port /dev/cu.usbserial-NNNNNNNN smaccm-sik-1.6-3drradio.ihx
```

changing the argument to `--port` to the device name noted above.

If you have previously entered AT command mode on your radio (as described
below) make sure to power cycle the radio before attempting to upload new
firmware.

# Modify smaccm-sik settings

After a firmware upload, all radio settings are restored to factory defaults.
The only factory default we want to change is the radio's channel setting.

Use a serial terminal to connect to the smaccm-sik radio at 57600 baud.
We use `miniterm.py`, which is installed with the `pyserial` python package.

When connected to a smaccm-sik radio, inputs will not be echoed to the serial
terminal.

## Enter AT command mode

To enter AT command mode, send the three characters "+++" on the serial
terminal, and then send nothing for 1 second.

When successfully entered into AT command mode, the radio will respond

```
AT
OK
```


Send the "ATI0" command to read out the firmware version string. It should
report

```
ATI0
SMACCM-SiK 1.6 on HM-TRP
```

## Read the channel

Send the command "ATI5" on the serial terminal. (All commands are completed by a
carraige return). The radio will return an incomplete list of the settings. (The
list is incomplete due to a software bug that will not be fixed, sorry. Luckily
the settings we care about are in the beginning of the list.)

The setting marked "S3: NETID=" indicates the channel setting.

## Set the channel


Send command "ATS3=NNN" replacing NNN with the channel code desired. Setting the
channel to 500 will be "ATS3=500".

Send "ATI5" to confirm that the S3 setting has changed to the channel you set.

Then, commit the change by sending the command "AT&W".

## Reboot the radio and verify

Exit your serial terminal, power cycle the radio by unplugging and re-plugging.
In a new serial terminal, enter AT command mode, and confirm the setting has
written to memory with the "ATI5" command.

Power cycle the radio a final time to exit AT command mode.

