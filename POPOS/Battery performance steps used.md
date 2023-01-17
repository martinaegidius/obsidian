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
