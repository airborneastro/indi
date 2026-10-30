# Fork of indilib/indi with modifications to indi_lx200gps

1) Modification implementing the :hIyymmddhhmmss command to initialize a (permanently mounted) LX200GPS with its GPS system switched off. After the :hI
initialization, TrackState is set to SCOPE_TRACKING, giving P.I.N.S. the green "tracking light".

2) Modification allowing to reboot a parked LX200GPS with the :I# command which enables to park and unpark the telescope without power toggling.

You can either replace the files below in your clone of https://github.com/indilib/indi.git or use this fork.

3) If you just want to have the modification for using the track buttons in PINS, do not use this fork. For the git clone below use
https://github.com/indilib/indi.git and replace this line in indi/drivers/telescope/lx200gps.cpp (do not change any other file):

```
SetTelescopeCapability(GetTelescopeCapability() | TELESCOPE_CAN_CONTROL_TRACK , 4); 
```
with 
```
SetTelescopeCapability(GetTelescopeCapability() | TELESCOPE_CAN_CONTROL_TRACK |
        TELESCOPE_HAS_TRACK_MODE, 4);
```
and then follow the further build instructions below. Only replace /usr/bin/indi_lx200generic with the new version.

If you would like to test your new version with an LX200GPS simulator instead of your real telescope, have a look at
https://github.com/airborneastro/LX200GPS

To build and install indi, see https://astroisk.nl/blog/2024/03/20/raspberry-pi-5-indi/ or the original README of indilib, also for installing the necessary prerequisits. 

```
mkdir -p ~/Projects
cd ~/Projects
git clone --depth 1 https://github.com/airborneastro/indi.git
mkdir -p ~/Projects/build/indi-core
cd ~/Projects/build/indi-core
cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Debug ~/Projects/indi
make -j4
```
(if you want to overwrite your indi installation or you never had one:
sudo make install
)

These files have been modified with respect to indilib/indi: 
```
drivers/telescope/lx200gps.h
drivers/telescope/lx200gps.cpp
drivers/telescope/lx200driver.h
drivers/telescope/lx200driver.cpp
drivers/telescope/lx200telescope.h
drivers/telescope/lx200telescope.cpp
```


If you do NOT make install for a full install, just replace the lx200 driver files: 
```
cd ~/Projects/build/indi-core/drivers/telescope
sudo cp /usr/bin/indi_lx200generic /usr/bin/orig_indi_lx200generic
sudo cp indi_lx200generic /usr/bin
sudo cp /lib/aarch64-linux-gnu/libindilx200.so.2.2.5 /lib/aarch64-linux-gnu/orig_libindilx200.so.2.2.5
sudo cp libindilx200.so.2.2.5 /lib/aarch64-linux-gnu/libindilx200.so.2.2.5
```
The /lib/aarch64-linux-gnu directory is for a Raspberry Pi 4/5 system only (such as PINS).

Another modification is in driver/focuser/myfocuserpro2.cpp (and .h). The driver now allows two
focusers.:
```
static std::unique_ptr<MyFocuserPro2> focuser1(new MyFocuserPro2("LX200GPS_Focus"));
static std::unique_ptr<MyFocuserPro2> focuser2(new MyFocuserPro2("ED80_Focus"));
```
If not using make install after make, do:

```
cd ~/Projects/build/indi-core/drivers/focuser
sudo cp /usr/bin/indi_myfocuserpro2_focus /usr/bin/orig_indi_myfocuserpro2_focus
sudo cp indi_myfocuserpro2_focus /usr/bin

```


