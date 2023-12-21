# waveshare-lcd

See [Waveshare documentation](<https://www.waveshare.com/wiki/3.5inch_RPi_LCD_(C)#Install_the_touch_driver>) for further details and more devices.

---

|                                    |                                     |
| ---------------------------------- | ----------------------------------- |
| raspberry pi 4                     | rasberry pi os - bookworm - 64 bits |
|                                    | rasberry pi os - bullseye - 64 bits |
| waveshare 3.5 inch lcd display (C) |                                     |

---

---

# setup

Before getting started:

```sh
sudo apt-get update && sudo apt-get upgrade
```

## install driver

```sh
mkdir setup && cd setup

git clone https://github.com/waveshare/LCD-show.git

cd LCD-show/

# replace `LCD35C` with whatever models
chmod +x LCD35C-show
# add `lite` flag for server install (no desktop)
# e.g ./LCD35C-show lite
./LCD35C-show
```

Device will reboot after last command has run, then.

### 64-bits systems

For **64-bits** systems, following must be replaced as `LCD-show` aims **32-bits** only:

```sh
sudo nano LCD35C-show
```

Then edit the file:

```sh
# find
sudo dpkg -i -B ./xinput-calibrator_0.7.5-1_armhf.deb

# to replace with
sudo apt-get install xinput-calibrator
```

---

## render to LCD screen

After last step, device should have rebooted but nothing is show on the new LCD screen.  
device graphic output must be rendered and drawn into the correct device / screen.

```sh
 sudo nano /usr/share/X11/xorg.conf.d/99-fbdev.conf

### add the following

Section "Device"
  Identifier "myfb"
  Driver "fbdev"
  # "/dev/fb0" will draw on HDMI screen instead
  Option "fbdev" "/dev/fb1"
EndSection
```

Now reboot and the LCD screen should display content.

---

---

# misc

### set calibration

If using Desktop, **Raspberry Pi OS** has aGUI available to retrieve **calibration values**.

```sh
sudo nano /etc/X11/xorg.conf.d/99-calibration.conf

### add the following

Section "InputClass"
  Identifier "calibration"
  MatchProduct "ADS7846 Touchscreen"
  # this may be changed and adjusted some settings later on
  Option "Calibration" "3900 290 290 3900"
  Option "SwitchAxes" "1"
EndSection
```

---

## virtual keyboard

By default, none is set, so a **physical keyboard** is still required at this point.

[`onboard`](https://manpages.ubuntu.com/manpages/focal/man1/onboard.1.html) and [`matchbox-keyboard`](https://github.com/mwilliams03/matchbox-keyboard) are most suggested options.

Following example will use `onboard`.

To install:

```sh
sudo apt-get install onboard
```

If using Desktop, a GUI interface will allow to add customization and further settings.

---

## login screen

By default, if **auto-login** is disabled, user will get prompt to enter an username and a password.  
This login screen does not have however the **virtual keyboard** enable, a **physical keyboard** is then still needed.

[`lightdm-gtk-greeter`](https://github.com/Xubuntu/lightdm-gtk-greeter) will replace the default `pi-greeter` layout in the following example.  
It is compatible with `lightdm` default manager.

```sh
# remove pi-greeter
sudo remove pi-greeter

# install lightdm-gtk-greeter
sudo apt-get install lightdm-gtk-greeter
```

Then set config for `lightdm-gtk-greeter`:

```sh
sudo nano /etc/lightdm/lightdm-gtk-greeter.conf

### add the following

# allow "accessibility" and "power" button in navbar (to allow on-scree keyboard)
indicators=~a11y;~power
# or matchbox-keyboard, or else
keyboard=onboard
# lcd 3.5 inch
keyboard-position=0 60%;100% 40%
# or lower / thiner
# keyboard-position=0 75%;100% 25%
```

Now reboot and all should be done!

---

---

# documentation

```

```
