```
gsettings set com.system76.hidpi enable false
```
Disabled hidpi daemon 
configured tlp CPU-mode to performance when on AC and powersave when on battery in /etc/tlp.conf

installed tlp and tlpui (visual conf into tlp.conf)

Added in auto-cpufreq: https://github.com/AdnanHodzic/auto-cpufreq

tlp and auto-cpufreq may conflict with system.powertools. Monitor for this. 
Steps for diverging to Pop_OS: 
[!] Backup the /etc/os-release file:
sudo mv /etc/os-release /etc/os-release-backup
[!] Create hardlink to /etc/os-release:
sudo ln /etc/pop-os/os-release /etc/os-release

Running live; check if alleviates before installing daemon as refed in github 
sudo auto-cpufreq --live


Disabled pop-os system76 Power (Gnome extensions)


added auto-cpufreq service to startup, is installed as snap; checck status by issuing: 
systemctl status snap.auto-cpufreq.service.service



<h1>Log 25th of June </h1>
- Calibrated powertop three times hoping to fix errors 
- Made and enabled systemd powertop.service in /etc/systemd/system/ which changes mode to --auto-tune
- Forced snap.auto-cpufreq.service.service to boot after powertop.service
- Added charging threshold 80% by following https://askubuntu.com/questions/1006778/set-battery-thresholds-on-ubuntu-asus
	- echo 80 | sudo tee /sys/class/power_supply/BAT0/charge_control_end_threshold
	- /etc/crontab: @reboot root echo 60 > /sys/class/power_supply/BAT0/charge_control_end_threshold
- Check if works by seeing if charges over 80% (light should turn off/on/change color)
- Also made a service which is easier to change 
	- battery-charge-threshold.service as described in arch-wiki. Change this if you want to change value 
- Added libinput-gestures
	- https://github.com/bulletmark/libinput-gestures
	- Seems not to fuck with wayland bindings, now switch workplace with three fingers instead of 4

Empty when turn-on issues: 
	https://discussion.fedoraproject.org/t/incorrect-battery-charge-display-in-gnome/71058/3
	



Log oct 8th 2023
- extended charge threshold to 98%
- Enabled powertop.service, which hopefully helps out battery issues



