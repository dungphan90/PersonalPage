## Installing the NVIDIA GPU driver, CUDA and CUDNN
Right after we have a clean installation of the ArchLinux vanilla, we should go ahead with installtion of GPU driver installation. I have two graphics cards installed on my system:
1. NVIDIA GeForce GTX 1070.
2. AMD RTX570.

The installation of the drivers for these two cards is as simple as
```bash
  sudo pacman -S nvidia # For NVIDIA 
  sudo pacman -S xf86-video-amdgpu vulkan-radeon libva-mesa-driver mesa-vdpau # For AMD (https://wiki.archlinux.org/index.php/AMDGPU)
```

After the installation of `xorg` (see below), additional steps are needed to exclude `xorg` from running on the NVIDIA card. I specifically need this because I just want to run the graphic server on the AMD card and free the NVIDIA card from any graphic rendering task. The NVIDIA card will be used only for parallel computing tasks.

See [here](https://gist.github.com/wangruohui/bc7b9f424e3d5deb0c0b8bba990b1bc5) for more details. Below is my `xorg.conf` file.
```conf
Section "ServerLayout"
	Identifier 	"Layout0"
	Screen		0		"Screen0"
EndSection

Section "Screen"
	Identifier	"Screen0"
	Device		"Device0"
EndSection

Section "Device"
	Identifier	"Device0"
	Driver		"amdgpu"
	VendorName	"Advance Micro Devices, Inc."
	BusID		"PCI:04:0:0"
EndSection
```
