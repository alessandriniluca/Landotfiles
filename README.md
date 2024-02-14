# Landotfiles
## Description
This folder contains the dotfiles of Landomix' PCs
## Installation
### Important notice
I use a hidden file in the home directory to store all environment variables I need for programming. By using this, it will be created through the `create-link` script as an empty file in `$HOME/.devenvs`, and sourced in
`$HOME/.zshrc`. Feel free to remove it if you find it useless, but remember to remove also the line that sources it in the zshrc file.
### Arch' setup phase
|**Package**|**Where**|
|-----------|---------|
|[base](https://archlinux.org/packages/?name=base)|pacman|
|[linux](https://archlinux.org/packages/?name=linux)|pacman|
|[linux-firmware](https://archlinux.org/packages/?name=linux-firmware)|pacman|
|[vim](https://archlinux.org/packages/?name=vim)|pacman|
|[sudo](https://archlinux.org/packages/?name=sudo)|pacman|
|[networkmanager](https://archlinux.org/packages/?name=networkmanager)|pacman|

Then, follow installation for GRUB.
### Nvidia
References: [Arch Wiki page](https://wiki.archlinux.org/title/NVIDIA)
|**Package**|**Where**|
|-----------|---------|
|[nvidia](https://archlinux.org/packages/extra/x86_64/nvidia/)|pacman|
|[libva-nvidia-driver](https://archlinux.org/packages/extra/x86_64/libva-nvidia-driver/)|pacman|
|[linux-headers](https://archlinux.org/packages/core/x86_64/linux-headers/)|pacman|

Setup: follow the [Hyprland Guide](https://wiki.hyprland.org/Nvidia/).

TL;DR (sources: [Hyprland Wiki](https://wiki.hyprland.org/Nvidia/) and [Arch Wiki](https://wiki.archlinux.org/title/NVIDIA#DRM_kernel_mode_setting)):
Add `nvidia_drm.modeset=1` to the end of `GRUB_CMDLINE_LINUX_DEFAULT=` in `/etc/default/grub`, then run `# grub-mkconfig -o /boot/grub/grub.cfg`.

In `/etc/mkinitcpio.conf` add `nvidia nvidia_modeset nvidia_uvm nvidia_drm` to your MODULES.

Run `# mkinitcpio --config /etc/mkinitcpio.conf --generate /boot/initramfs-custom.img` (make sure you have the `linux-headers` package installed first).

Add a new line to `/etc/modprobe.d/nvidia.conf` (make it if it does not exist) and add the line `options nvidia-drm modeset=1`.

Export the following variables in the Hyprland configs (n.b.: if you clone these config, these variables are **already included**):

```
env = LIBVA_DRIVER_NAME,nvidia
env = XDG_SESSION_TYPE,wayland
env = GBM_BACKEND,nvidia-drm
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = WLR_NO_HARDWARE_CURSORS,1
```

### Utilities to run Hyprland at first login with zero effort
|**Package**|**Where**|
|-----------|---------|
|[kitty](https://archlinux.org/packages/?name=kitty)|pacman|

### Aur Helper
I chose [yay](https://aur.archlinux.org/packages/yay) as [AUR helper](https://wiki.archlinux.org/title/AUR_helpers).

Source: [yay's GitHub page](https://github.com/Jguer/yay).

According to their installation guide, the packages needed are:

|**Package**|**Where**|
|-----------|---------|
|[base-devel](https://archlinux.org/packages/core/any/base-devel/)|pacman|
|[git](https://archlinux.org/packages/?name=git)|pacman|

Since you are installing `git`, you can already store your credentials, if you want (source: [here](https://man.archlinux.org/man/git-credential-store.1.en))
```
$ git config --global credential.helper store
```
Press enter, and username and password(which needs to be a GitHub token with repo rights), will be stored

Do also the configuration of mail and name (source: [here](https://wiki.archlinux.org/title/git)):
```
$ git config --global user.name  "John Doe"
$ git config --global user.email "johndoe@example.com"
```

Then, it is enough to follow the guide.

TL;DR:
```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

### Audio
|**Package**|**Where**|
|-----------|---------|
|[pipewire](https://archlinux.org/packages/?name=pipewire)|pacman|
|[pavucontrol](https://archlinux.org/packages/extra/x86_64/pavucontrol/)|pacman|
|[wireplumber](https://archlinux.org/packages/extra/x86_64/wireplumber/)|pacman|
|[pipewire-pulse](https://archlinux.org/packages/extra/x86_64/pipewire-pulse/)|pacman|

Check, could be needed [libpipewire](https://archlinux.org/packages/extra/x86_64/libwireplumber/)


### Shell
|**Package**|**Where**|
|-----------|---------|
|[zsh](https://archlinux.org/packages/?name=zsh)|pacman|
|[nerd-fonts](https://archlinux.org/groups/x86_64/nerd-fonts/)|pacman|
|[zsh-syntax-highlighting](https://archlinux.org/packages/extra/any/zsh-syntax-highlighting/)|pacman|

For the installation procedure, see [here](https://wiki.archlinux.org/title/zsh) and [here to change the defaul shell](https://wiki.archlinux.org/title/Command-line_shell#Changing_your_default_shell).

TL;DR: install zsh through the package manager.
Type
```
$ zsh
```
and proceed with the installation. Keep default values for history, and default for
completion systems.

List available shells with
```
$ chsh -l
```
Notice that `/usr/bin/zsh` should be present.

Change the default shell to zsh through:
```
$ chsh -s /usr/bin/zsh
```

Then we can istall [oh my zsh](https://github.com/ohmyzsh/ohmyzsh) (source [here](https://wiki.archlinux.org/title/zsh)).

TL;DR:
```
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Then you can configure [powerlevel10k](https://github.com/romkatv/powerlevel10k).

TL;DR:
```
$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
and then set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`. Type from your home:
```
$ source .zshrc
```
and follow the installation.

If, as me, you print stuff when you open the terminal, you may want to set `typeset -g POWERLEVEL9K_INSTANT_PROMPT=` to `quiet` in `~/.p10k.zsh`

When you install zsh-syntax-highlighting, remember to pur `source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh` at the end of `~/.zshrc`

**Plugins**

[zsh-autosuggestion](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)

TL;DR:
```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
and add the plugin inside the list of oh my zsh plugins in `~/.zshrc`:
```
plugins=( 
    # other plugins...
    zsh-autosuggestions
)
```

[catpuccin syntax highlighting](https://github.com/catppuccin/zsh-syntax-highlighting)

TL;DR: requires [zsh-syntax-highlighting](https://archlinux.org/packages/extra/any/zsh-syntax-highlighting/), then
```
git clone https://github.com/catppuccin/zsh-syntax-highlighting.git
cd zsh-syntax-highlighting/themes/
mkdir ~/.zsh/
cp -v catppuccin_mocha-zsh-syntax-highlighting.zsh ~/.zsh/
```
If you want to do as me, clone the repo in the `~/github-repos` folder, then instead of the `cp`, just do:
```
ln -s ~/github-repos/zsh-syntax-highlighting/themes/catppuccin_mocha-zsh-syntax-highlighting.zsh ~/.zsh/catppuccin_mocha-zsh-syntax-highlighting.zsh
```
and add to `~/.zshrc`:
```
source ~/.zsh/catppuccin_mocha-zsh-syntax-highlighting.zsh

# now load zsh-syntax-highlighting plugin
```


**Login theme**

I opted for [this](https://github.com/Keyitdev/sddm-astronaut-theme.git). N.B.: You need the package `sddm` in Hyprland's stuff.

Other dependencies:
|**Package**|**Where**|
|-----------|---------|
|[qt5-graphicaleffects](https://archlinux.org/packages/extra/x86_64/qt5-graphicaleffects/)|pacman|
|[qt5-quickcontrols2](https://archlinux.org/packages/extra/x86_64/qt5-quickcontrols2/)|pacman|
|[qt5-svg](https://archlinux.org/packages/extra/x86_64/qt5-svg/)|pacman|

TL;DR:

Clone the repository into `/usr/share/sddm/themes`:
```
$ sudo git clone https://github.com/Keyitdev/sddm-astronaut-theme.git /usr/share/sddm/themes/sddm-astronaut-theme
```
Copy fonts to `/usr/share/fonts`:
```
$ sudo cp /usr/share/sddm/themes/sddm-astronaut-theme/Fonts/* /usr/share/fonts/
```
Then edit `/etc/sddm.conf`:
```
$ echo "[Theme]
Current=sddm-astronaut-theme" | sudo tee /etc/sddm.conf
```
IMPORTANT: in `sudo vim /usr/share/sddm/themes/sddm-astronaut-theme/Components/Input.qml` search for `Font.Capitalize : Font.MixedCase` and remove that line, otherwise the first letter in the username will be uppercase ([source](https://github.com/MarianArlt/sddm-sugar-dark/issues/14))

```
$ sudo systemctl enable sddm.service
```
**Spicetify**

Follow instructions [here](https://github.com/catppuccin/spicetify). TL;DR:

Assuming your user is in group `wheel`:
```
$ sudo chgrp wheel /opt/spotify
$ sudo chgrp -R wheel /opt/spotify/Apps
$ sudo chmod 775 /opt/spotify
$ sudo chmod 775 -R /opt/spotify/Apps
```
Then:
```
$ cd $HOME/github-repos/
~/github-repos$ git clone https://github.com/catppuccin/spicetify.git
~/github-repos$ ln -s $HOME/github-repos/spicetify/catppuccin $HOME/.config/spicetify/Themes/catppuccin
```
Open sptotify the first time to generate configs, then:
```
$ spicetify config current_theme catppuccin
$ spicetify config color_scheme mocha
$ spicetify backup apply
```

**Wireguard**

After having installed the package linked in optionals section, put your files in `etc/wireguard/filename.conf`. `sudo wg-quick up filename` and `sudo wg-quick down filename` will allow you to connect / disconnect to the VPN.

**Grub Setup**

[Source](https://github.com/vinceliuice/grub2-themes)
```
$ cd $HOME/github-repos && git clone https://github.com/vinceliuice/grub2-themes
github-repos$ cd grub2-themes && sudo ./install.sh -t vimix -s 2k -i color
```

### Utilities
|**Package**|**Where**|**Description**|
|-----------|---------|----------|
|[alacritty](https://archlinux.org/packages/?name=alacritty)|pacman|Terminal|
|[btop](https://archlinux.org/packages/extra/x86_64/btop/)|pacman|Usage of CPU|
|[firefox](https://archlinux.org/packages/?name=firefox)|pacman|Browser|
|[gedit](https://archlinux.org/packages/?name=gedit)|pacman|Text editor|
|[imv](https://archlinux.org/packages/?name=imv)|pacman|Image viewer|
|[telegram-desktop](https://archlinux.org/packages/?name=telegram-desktop)|pacman|Messaging|
|[nemo](https://archlinux.org/packages/?name=nemo)|pacman|File explorer|
|[nemo-fileroller](https://archlinux.org/packages/extra/x86_64/nemo-fileroller/)|pacman|Utility to compress and uncompress files with right click|
|[neofetch](https://archlinux.org/packages/extra/any/neofetch/)|pacman|Display purposes|
|[nvtop](https://archlinux.org/packages/extra/x86_64/nvtop/)|pacman|Usage and processes GPU|
|[topgrade](https://aur.archlinux.org/packages/topgrade)|AUR|Update and upgrade everything with one command|
|[visual-studio-code-bin](https://aur.archlinux.org/packages/visual-studio-code-bin)|AUR|Code IDE|
|[whatsie](https://aur.archlinux.org/packages/whatsie)|AUR|Whatsapp client|

### Dual boot Windows (sad landomix)
|**Package**|**Where**|
|-----------|---------|
|[ntfs-3g](https://archlinux.org/packages/?name=ntfs-3g)|pacman|
|[os-prober](https://archlinux.org/packages/?name=os-prober)|pacman|

This package is needed to mount windows' partition in order to add it to [GRUB](https://wiki.archlinux.org/title/GRUB)'s menu.

TL;DR:

In order to detect other file systems, edit / uncomment
```
GRUB_DISABLE_OS_PROBER=false
```
in
```
/etc/default/grub
```
N.B.: remember **sudo** when editing `/etc/default/grub`.

Then, follow the guide to generate `grub.cfg`

TL;DR:

Mount the partition from which the other system boots, and then run
```
# grub-mkconfig -o /boot/grub/grub.cfg
```

### Hyprland stuff
|**Package**|**Where**|**Purpose**|
|-----------|---------|-----------|
|[cava](https://aur.archlinux.org/packages/cava)|AUR|Fancy audio visualizer|
|[grimblast-git](https://aur.archlinux.org/packages/grimblast-git)|AUR|Screenshots|
|[polkit](https://archlinux.org/packages/extra/x86_64/polkit/)|pacman|Application development toolkit for controlling system-wide privileges|
|[polkit-kde-agent](https://archlinux.org/packages/extra/x86_64/polkit-kde-agent/)|pacman|Authentication Agent|
|[pyprland](https://aur.archlinux.org/packages/pyprland)|AUR|Easier scratchpads conf|
|[python-requests](https://archlinux.org/packages/extra/any/python-requests/)|pacman|Needed for weather in waybar|
|[sddm](https://archlinux.org/packages/extra/x86_64/sddm/)|pacman|Manager for login|
|[swaylock-effects](https://aur.archlinux.org/packages/swaylock-effects)|AUR|Lock screen|
|[swaync](https://aur.archlinux.org/packages/swaync)|AUR|Notification Center|
|[swappy](https://archlinux.org/packages/extra/x86_64/swappy/)|pacman|Needed for screenshots|
|[udiskie](https://archlinux.org/packages/extra/any/udiskie/)|pacman|Needed to automount disks connected via usb|
|[waybar](https://archlinux.org/packages/extra/x86_64/waybar/)|pacman|Top screen status bar|
|[wl-clipboard](https://archlinux.org/packages/extra/x86_64/wl-clipboard/)|pacman|Clipboard, for screenshots and others|
|[wofi](https://archlinux.org/packages/extra/x86_64/wofi/)|pacman|Applications launcher|

### Optionals
|**Package**|**Where**|**Purpose**|
|-----------|---------|-----------|
|[bitwarden](https://archlinux.org/packages/extra/x86_64/bitwarden/)|pacman|Password manager|
|[discord](https://archlinux.org/packages/?name=discord)|pacman|Discord|
|[filelight](https://archlinux.org/packages/extra/x86_64/filelight/)|pacman|Disk usage viewer|
|[inetutils](https://archlinux.org/packages/core/x86_64/inetutils/)|pacman|Provides the command `hostname`, useful when writing the comment while generating the ssh key|
|[partitionmanager](https://archlinux.org/packages/extra/x86_64/partitionmanager/)|pacman|A KDE utility that allows you to manage disks, partitions, and file systems|
|[miniconda3](https://aur.archlinux.org/packages/miniconda3)|AUR|Conda provider|
|[obs-studio](https://archlinux.org/packages/extra/x86_64/obs-studio/)|pacman|OBS|
|[onlyoffice-bin](https://aur.archlinux.org/packages/onlyoffice-bin)|AUR|Office Suite|
|[openconnect](https://archlinux.org/packages/extra/x86_64/openconnect/)|pacman|Needed to connect to university's vpn|
|[pdfarranger](https://archlinux.org/packages/extra/any/pdfarranger/)|pacman|Arrange and merge PDFs|
|[python-poetry](https://archlinux.org/packages/extra/any/python-poetry/)|pacman|Python management|
|[spotify](https://aur.archlinux.org/packages/spotify)|AUR|Spotify|
|[spicetify-cli](https://aur.archlinux.org/packages/spicetify-cli)|AUR|CLI utility to customize spotify theme|
|[teams-for-linux](https://aur.archlinux.org/packages/teams-for-linux)|AUR|Microsoft Teams|
|[upscayl-bin](https://aur.archlinux.org/packages/upscayl-bin)|AUR|Image Upscaler|
|[wireguard-tools](https://archlinux.org/packages/extra/x86_64/wireguard-tools/)|pacman|Wireguard tools to conenct to wireguard VPNs|

### Create link
Create the dynamic links to this folder files. To do so, in this cloned repo:
```wofi
Landotfiles $ chmod.x create-link.sh
Landotfiles $ ./create-link.sh
```

## What's Next?
- [ ] Complete waybar subpackages (if you want to check, run waybar from terminal, and while is open see what fails)
- [ ] wallpaper
- [ ] Startup things little bit delayed (e.g., nextcloud)
- [ ] bindings in hypr.conf (source different files)
- [x] screenshot
- [ ] screen sharing
- [ ] fix waybar & cava
- [ ] Authentication agent W.I.P. : still need to solve vscode's keyring issue with the wallet. According to the documentation of VScode it doesn't detect the Desktop Environment, need to investigate
- [x] swaylock
- [x] swaync
- [ ] Screen recording
- [ ] double keyboard layout
- [x] Whatsie
- [x] pdfarranger
- [x] obs
- [x] onlyoffice (has been fixed for hyprland)
- [x] discord
- [x] spicetify & spotify
- [x] Wireguard
- [x] Bitwarden
- [ ] Nextcloud
- [x] Teams
- [ ] Fix icons weather
- [ ] Fix themes of applications
- [x] Better GRUB
- [ ] Dashboards, plus control volumes from Gl00ria's dotfiles
- [x] Remap GMMK Pro rotatory encoder (see if with maiusc + encoder you can change brightness instead of volume).
- [ ] check if you can change brightness and microphone volume with rotatory encoder of the keyboardgrub 
- [ ] volume on screen popup
- [x] pyprland
- [ ] Clean pyprland.json and complete configs
- [ ] change pyprland dropdown term with alacritty
- [x] fix time
- [x] add login interface
- [ ] Timeshift (need to fix partition on the other SSD).
-----------------------
- [ ] make installation script
- [ ] do the same for the laptop

## Credits
- https://github.com/Gl00ria/dotfiles
- https://github.com/juriSacchetta/.dotfiles
- https://github.com/Fili-ai/Dotfiles
- https://github.com/Keyitdev/sddm-astronaut-theme
- https://github.com/Axenide/Dotfiles