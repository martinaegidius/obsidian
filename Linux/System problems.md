After updating Manjaro, ipv6 seems to be default to NetworkManager. 
Gives errors. 
sudo journalctl -fu NetworkManager

Should be set using sysconf, but I have no luck. Tried adding to grub-boot-line ipv6.disable_ipv6=1, still to no awail even after sudo update-grub. 

Goal: disable ipv6 alltogether for network card. 

Update: seems to be working

