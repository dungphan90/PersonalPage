# Instructions on how to connect the Nintendo Switch Pro controller
In this document, you can find, pair, connect and read the input event signals from the controller. This would be the first step to build a game-pad controlled robot arm.

## Connecting from bluetooth
We will need `bluez` to make a bluetooth connection to the device. 
```bash
sudo pacman -S bluez bluez-utils
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
```

Run
```bash
bluetoothctl
```
Once in the `bluetoothctl` suite, run following commands. They are self-explanatory
```
power on
scan on
```
A list of discoverable devices will appear with their corresponding MAC address. Look for the name of the controller, mine is "LIC PRO CONTROLLER". Say the MAC is `40:EC:99:A5:93:20`. We continue by
```
scan off
pair 40:EC:99:A5:93:20
trust 40:EC:99:A5:93:20
connect 40:EC:99:A5:93:20
```
Make sure the connection is approved.


## Kernel module `hid-nintento` and daemon `joycond`
If you didn't install `dkms` and `linux-headers` then go ahead and install these packages first:
```bash
sudo pacman -S dkms linux-lts-headers
```
I used the `linux-lts` so I installed the `linux-lts-headers`. If you are not using the LTS version, install the non-LTS kernel headers.

Let's start with installing the kernel module. The installation instruction is right on the module's (github)[https://github.com/nicman23/dkms-hid-nintendo]. You might need to change it a bit.
```bash
git clone https://github.com/nicman23/dkms-hid-nintendo
cd dkms-hid-nintendo
sudo dkms add .
```
After the last command, the text output will let you know the version of the `hid-nintendo` being installed. At the moment of this writing, I have `nintendo-3.1` version installed. This version number goes into the next command.
```bash
sudo dkms build nintendo -v 3.1
sudo dkms install nintendo -v 3.1
```

For `joycond`, follow the instruction (here)[https://github.com/DanielOgorchock/joycond]. We need `libevdev`. I have installed `libinput` which depends on `libevdev`. If these packages are not on your system yet, install them. Otherwise, go ahead with `joycond`:
```bash
sudo pacman -S libevdev
git clone git@github.com:DanielOgorchock/joycond.git
cd joycond
sudo make install
sudo systemctl enable --now joycond
sudo systemctl start --now joycond
```

## Quick check using (Gamepad\_Tester)[https://gamepad-tester.com/]
Just go to (`https://gamepad-tester.com/`)[https://gamepad-tester.com/] while the controller is connected. Trying out the buttons on the controller and see if the website can register the controller's signals.

## Test communicatio with `evdev`
I followed the instructions in (here_)[https://core-electronics.com.au/tutorials/using-usb-and-bluetooth-controllers-with-python.html]. You should read the article carefully, it's full with useful information. I listed the commands I ran here.

Verify controller connection. The devices and the Linux system communicate by reading/writing the device files in `/dev`
