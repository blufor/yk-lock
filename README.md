#About
* Requires properly working DBUS session with gnome-screensaver
* Implemented via UDEV events
* Should be working with any YubiKey (detects the keyboard device, which should be present on all devices)
* Automatic user detection based on token serial number
* Logs event to syslog

#Usage

###Clone this repo
```git clone https://github.com/blufor/yk-lock-gss.git && cd yk-lock-gss```
###Copy the script
```sudo cp yk-lock-gss /usr/local/bin/```
###Save your YubiKey SN
```ykinfo -a | awk '/serial:/ {printf("%010d",$2)}' > ~/.ykserial && chmod 600 ~/.ykserial```
###Configure UDEV
####Lock/unlock mode
```
#/etc/udev/rules.d/90-yk-lock-gss.rules
SUBSYSTEM=="input", ENV{PROP}=="0", ENV{ID_VENDOR}=="Yubico", RUN+="/usr/local/bin/yk-lock-gss"
```
####Lock only mode
Use this mode, if you want to authenticate in a safer way (either PAM or YubiKey's static password)
```
#/etc/udev/rules.d/90-yk-lock-gss.rules
ACTION=="remove", SUBSYSTEM=="input", ENV{PROP}=="0", ENV{ID_VENDOR}=="Yubico", RUN+="/usr/local/bin/yk-lock-gss"
```
###Restart UDEV
```restart udev```
