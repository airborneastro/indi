# Fork of indilib/indi with modifications to indi_lx200gps

Modification implementing the :hIyymmddhhmmss command to initialize a (permanently mounted) LX200GPS with its GPS system switched off. 

Further modification allowing to reboot a parked LX200GPS with the :I# command which enables to park and unpark the telescope without power toggling.

You can either replace the files below in your clone of https://github.com/indilib/indi.git or use this fork.

To build indi, see https://astroisk.nl/blog/2024/03/20/raspberry-pi-5-indi/ or the original README of indilib.

```
mkdir -p ~/Projects
cd ~/Projects
git clone --depth 1 https://github.com/airborneastro/indi.git
mkdir -p ~/Projects/indi/tmp
cd ~/Projects/indi/tmp
cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Debug ~/Projects/indi
make -j4
sudo make install

```
These files have been modified with respect to indilib/indi: 
```
drivers/telescope/lx200gps.h
drivers/telescope/lx200gps.cpp
drivers/telescope/lx200driver.hs
drivers/telescope/lx200driver.cpp
drivers/telescope/lx200telescope.h
drivers/telescope/lx200telescope.cpp
```


If you do NOT make install for a full install, just replace the lx200 driver files: 
```
cd ~/Projects/indi/tmp/drivers/telescope
sudo cp /usr/bin/indi_lx200generic orig_indi_lx200generic
sudo cp indi_lx200generic /usr/bin
sudo cp /lib/aarch64-linux-gnu/libindilx200.so.2.2.3 /lib/aarch64-linux-gnu/orig_libindilx200.so.2.2.3
sudo cp libindilx200.so.2.2.3 /lib/aarch64-linux-gnu/libindilx200.so.2.2.3
```
The /lib/aarch64-linux-gnu directory is for a Raspberry Pi 4/5 system only.

Another modification is in driver/focuser/myfocuserpro2.cpp (and .h). The driver now allows two
focusers.:
```
static std::unique_ptr<MyFocuserPro2> focuser1(new MyFocuserPro2("LX200GPS_Focus"));
static std::unique_ptr<MyFocuserPro2> focuser2(new MyFocuserPro2("ED80_Focus"));
```
If not using make install after make, do:

```
cd ~/Projects/indi/tmp/drivers/focuser
sudo cp /usr/bin/indi_myfocuserpro2_focus orig_indi_myfocuserpro2_focus
sudo cp indi_myfocuserpro2_focus /usr/bin

```


