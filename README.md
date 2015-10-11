#About
* Requires LightDM session manager and working HMAC-SHA1 PAM auth
* Implemented via UDEV events
* Should be working with any YubiKey (detects the keyboard device, which should be present on all devices)
* Automatic user detection based on token serial number and active sessions
* Logs events to syslog

#Usage

###Clone this repo
```git clone https://github.com/blufor/yk-lock.git && cd yk-lock```
###Copy the script
```sudo cp yk-lock /usr/local/bin/```
###Configure UDEV
####Lock/unlock mode
```
#/etc/udev/rules.d/90-yk-lock-gss.rules
SUBSYSTEM=="input", ENV{PROP}=="0", ENV{ID_VENDOR}=="Yubico", RUN+="/usr/local/bin/yk-lock"
```
####Lock only mode
Use this mode, if you want to authenticate in a safer way (either PAM or YubiKey's static password)
```
#/etc/udev/rules.d/90-yk-lock-gss.rules
ACTION=="remove", SUBSYSTEM=="input", ENV{PROP}=="0", ENV{ID_VENDOR}=="Yubico", RUN+="/usr/local/bin/yk-lock"
```
###Restart UDEV
```restart udev```
