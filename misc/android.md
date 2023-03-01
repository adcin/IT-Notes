# adb

[Official user guide for the android command-line tools](https://developer.android.com/studio/command-line)

## Connect device

First you need to enable **USB debugging** in the **[Developer Options](https://developer.android.com/studio/debug/dev-options)** on your device.

**Attention!** You probably will need to accept the following connection attempts physically on the device.

1. Connect the device via USB.
2. Set the device to listen to port 5555 via tcp/ip.
```shell
adb tcpip 5555
```
3. Connect via WiFi.
```shell
adb connect 192.168.0.123:5555
```
4. Check the connection.
```shell
adb devices
```

## Check connected devices

```shell
adb devices
```

The first part of the output in each line is the serial number, which specifies the device in adb.  

Use the -l option for more information:

```shell
adb devices -l
``` 

## Multiple Devices

If more then one device is connected you can use the -s option with the adb commands to specify the serial number. 

Example:  

```shell
adb -s 192.168.0.137:5555 shell input keyevent 4
```

## Trigger events

```shell
adb shell input keyevent <event code number>
```

Example - back `←` event

```shell
adb shell input keyevent 4
```

| code | event |
|:----:|:------|
| 0 | KEYCODE_UNKNOWN |
| 1 | KEYCODE_MENU |
| 2 | KEYCODE_SOFT_RIGHT |
| 3 | KEYCODE_HOME |
| 4 | KEYCODE_BACK |
| 5 | KEYCODE_CALL |
| 6 | KEYCODE_ENDCALL |
| 7 | KEYCODE_0 |
| 8 | KEYCODE_1 |
| 9 | KEYCODE_2 |
| 10 | KEYCODE_3 |
| 11 | KEYCODE_4 |
| 12 | KEYCODE_5 |
| 13 | KEYCODE_6 |
| 14 | KEYCODE_7 |
| 15 | KEYCODE_8 |
| 16 | KEYCODE_9 |
| 17 | KEYCODE_STAR |
| 18 | KEYCODE_POUND |
| 19 | KEYCODE_DPAD_UP |
| 20 | KEYCODE_DPAD_DOWN |
| 21 | KEYCODE_DPAD_LEFT |
| 22 | KEYCODE_DPAD_RIGHT |
| 23 | KEYCODE_DPAD_CENTER |
| 24 | KEYCODE_VOLUME_UP |
| 25 | KEYCODE_VOLUME_DOWN |
| 26 | KEYCODE_POWER |
| 27 | KEYCODE_CAMERA |
| 28 | KEYCODE_CLEAR |
| 29 | KEYCODE_A |
| 30 | KEYCODE_B |
| 31 | KEYCODE_C |
| 32 | KEYCODE_D |
| 33 | KEYCODE_E |
| 34 | KEYCODE_F |
| 35 | KEYCODE_G |
| 36 | KEYCODE_H |
| 37 | KEYCODE_I |
| 38 | KEYCODE_J |
| 39 | KEYCODE_K |
| 40 | KEYCODE_L |
| 41 | KEYCODE_M |
| 42 | KEYCODE_N |
| 43 | KEYCODE_O |
| 44 | KEYCODE_P |
| 45 | KEYCODE_Q |
| 46 | KEYCODE_R |
| 47 | KEYCODE_S |
| 48 | KEYCODE_T |
| 49 | KEYCODE_U |
| 50 | KEYCODE_V |
| 51 | KEYCODE_W |
| 52 | KEYCODE_X |
| 53 | KEYCODE_Y |
| 54 | KEYCODE_Z |
| 55 | KEYCODE_COMMA |
| 56 | KEYCODE_PERIOD |
| 57 | KEYCODE_ALT_LEFT |
| 58 | KEYCODE_ALT_RIGHT |
| 59 | KEYCODE_SHIFT_LEFT |
| 60 | KEYCODE_SHIFT_RIGHT |
| 61 | KEYCODE_TAB |
| 62 | KEYCODE_SPACE |
| 63 | KEYCODE_SYM |
| 64 | KEYCODE_EXPLORER |
| 65 | KEYCODE_ENVELOPE |
| 66 | KEYCODE_ENTER |
| 67 | KEYCODE_DEL |
| 68 | KEYCODE_GRAVE |
| 69 | KEYCODE_MINUS |
| 70 | KEYCODE_EQUALS |
| 71 | KEYCODE_LEFT_BRACKET |
| 72 | KEYCODE_RIGHT_BRACKET |
| 73 | KEYCODE_BACKSLASH |
| 74 | KEYCODE_SEMICOLON |
| 75 | KEYCODE_APOSTROPHE |
| 76 | KEYCODE_SLASH |
| 77 | KEYCODE_AT |
| 78 | KEYCODE_NUM |
| 79 | KEYCODE_HEADSETHOOK |
| 80 | KEYCODE_FOCUS |
| 81 | KEYCODE_PLUS |
| 82 | KEYCODE_MENU |
| 83 | KEYCODE_NOTIFICATION |
| 84 | KEYCODE_SEARCH |
| 85 | TAG_LAST_KEYCODE|

## Simulate a tap on the touchscreen

```shell
adb shell input tap <x-Koordinate> <y-Koordinate>
```

## Start an app

```shell
adb shell monkey -p your.app.package.name 1
```

## Force an app to quit

```shell
adb shell am force-stop your.app.package.name
```

## Take screenshot

```shell
adb -s <DEVICE_ADB_SERIAL> exec-out screencap -p > /path/to/the/screenshot.png
```

Example:

```shell
adb -s 192.168.0.137:5555 exec-out screencap -p > /home/myusername/Pictures/screenshot.png
```
