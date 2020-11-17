## Installing the NVIDIA GPU driver, CUDA and CUDNN
Right after we have a clean installation of the ArchLinux vanilla, we should go ahead with installtion of GPU driver installation. I have two graphics cards installed on my system:
1. NVIDIA GeForce GTX 1070.
2. AMD RTX570.

The installation of the drivers for these two cards is as simple as
```bash
# For NVIDIA
sudo pacman -S nvidia
# For AMD (https://wiki.archlinux.org/index.php/AMDGPU)
sudo pacman -S xf86-video-amdgpu vulkan-radeon libva-mesa-driver mesa-vdpau
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
The `BusID` can be found be the `lspci | VGA` command. 

### CUDA and CUDNN
CUDA and CUDNN can be installed by the `yay` AUR helper.
```bash
yay -S cuda cudnn
```

If `yay` is not installed yet, you can install it by
```bash
cd /opt
sudo git clone https://aur.archlinux.org/yay-git.git
sudo chown -R $USER:$USER ./yay-git
cd yay-git
makepkg -si
```
To update the all the installed packages, run
```bash
yay -Syu
```

## SpaceVIM
`SpaceVIM` is my favorite VIM distribution. The installation and usage instructions of it can be found [here](https://spacevim.org/quick-start-guide/).
```bash
sudo pacman -S neovim
curl -sLf https://spacevim.org/install.sh | bash
cd $HOME/.SpaceVim/bundle/vimproc.vim
make
```

To setup soft wrap, we will add a couple of lines to the `$HOME/.vim/vimrc`.
```bash
echo "set wrap linebreak nolist" >> $HOME/.vim/vimrc
echo "set showbreak=+++" >> $HOME/.vim/vimrc
```

## zsh, Oh-My-Zsh, and Powerline10k
This is my complete set of SHELL environment and terminal decoration. This works perfectly with the pre-config `st-distrotube-git` package. You don't have to manually switch the default SHELL to `zsh`, during the installation of `Oh-My-Zsh` you will be asked if you want `zsh` as the default SHELL, just select `Yes` there.
```bash
# zsh
sudo pacman -S zsh
# Oh-My-Zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
To be able to setup the `Powerline10k`, open the `$HOME/.zshrc` and change the `ZSH_THEME` to `ZSH_THEME="powerlevel10k/powerlevel10k"`. When you restart the terminal or run `exec $SHELL`, the `p10k configure` will be run automatically to help you choose your flavor of `Powerline10k`.

## Sound system
First make sure your account is in the `audio` group.
```bash
usermod -aG audio $USER
```

Next we need the driver for the audio hardware. I choose to use `alsa` here.
```bash
sudo pacman -S alsa-utils alsa-plugins
```
You don't need to run the `systemctl enable` for this program. It is enabled with `systemd` by default.

Next, we will install a sound server. `PulseAudio` is my choice here. Besides that, we also want a sound control app.
```bash
sudo pacman -S pulseaudio-alsa
yay -S pamix-git
```

## Kerberos and SSH connections
```bash
sudo pacman -S krb5
```
The Kerberos config file and the connection functions can be found in the my GitHub [repo](https://github.com/dungphan90/LinuxOS-Config). Instructions to use these config files can be found below. 

## Versioning the `$HOME` directory with `git`
I will not say much about the issue as this [article](https://www.electricmonk.nl/log/2015/06/22/keep-your-home-dir-in-git-with-a-detached-working-directory/) has provided an excellent instruction on how to achieve it. Just a small notice is that the fourth coding listing in the article
```bash
~/.dotfiles$ echo "alias dgit='git --git-dir ~/.dotfiles/.git --work-tree=\$HOME'" >> ~/.bashrc
```
contains a small typo. You just need to remove the `\` sign.
```bash
~/.dotfiles$ echo "alias dgit='git --git-dir ~/.dotfiles/.git --work-tree=$HOME'" >> ~/.bashrc
```
Another small issue is for those who use GitHub. They change the default branch from `master` to `main`, so pay attention when following the instructions in the article.


