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

After the installation of `xorg` (see below), additional steps are needed to exclude `xorg` from running on the NVIDIA card. I specifically need this because I just want to run the graphic server on the AMD card and free the NVIDIA card from any graphic rendering task. The NVIDIA card will be used only for parallel computing tasks. See [here](https://gist.github.com/wangruohui/bc7b9f424e3d5deb0c0b8bba990b1bc5) for more details. Below is my `xorg.conf` file.
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
The `BusID` can be found be the `lspci | grep VGA` command. 

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

Lastly, for those of you who want to look at my config files, they are [here](https://github.com/dungphan90/LinuxOS-Config). I also keep a config repository for MacOS [here](https://github.com/dungphan90/MacOS-SysConf).

## Setup `pyenv`
A simple python guideline is that you never touch the `system` python. Install your own base versions with `pyenv`. For each base version, build your set of package dependencies with `venv`. In ArchLinux, you can install `pyenv` with `pacman`.
```bash
sudo pacman -S pyenv
```

To use `pyenv`, you need to have a few setup line in the `.bashrc` or `.zshrc` profiles. I keep mine in my GitHub repo. For those of you that don't want my whole `.zenv.sh`, the script is below.
```bash
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```

More details can be found [here](https://realpython.com/intro-to-pyenv/).

### Setup base and virtual python environment
Setup the base environment with `pyenv`. Start a new SHELL (`exec $SHELL` or restart the terminal) and run the following lines.
```bash
# Check which versions are available
# If you run this for the first time, the deault python will be the system one.
pyenv versions 

# Install a base version then setting it as defautl
pyenv install 3.8.3
pyenv global 3.8.3
```
You can also setup a temporary python environment for a work session with `pyenv local`.

Setup the virtual environment with your choice of a set of packages. We do this with `venv` package.
```bash
python -m venv <venv_name>
source /path/to/venv_name/bin/activate
```
