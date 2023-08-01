https://forum.ableton.com/viewtopic.php?t=244162  <- your wineprefix is defined as here 

WINEARCH=win64 WINEPREFIX=~/wine/ableton winetricks d3dx9 d9vk dotnet35 dotnet452 dxvk gdiplusvcrun2019 corefonts tahoma

- Warning: warning: This package (dotnet452) is broken in wine-6.0.3. Broken since 5.18. See https://bugs.winehq.org/show_bug.cgi?id=49897 for more info. Use --force to try anyway.

Installed seems fine but does not start

Ran: 
sudo apt-get remove winbind && sudo apt-get install winbind


When come home: 
cd "$WINEPREFIX"  
winetricks gdiplus msxml3 msxml4 msxml6 vcrun2005 quicktime72 fontsmooth=rgb msls31 d3dcompiler_43 corefonts atmlib


Use first source for prefix and and install dependencies as in: 
https://appdb.winehq.org/objectManager.php?sClass=version&iId=41212

