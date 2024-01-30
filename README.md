# Landotfiles
## Description
This folder contains the dotfiles of Landomix' PCs
## Installation
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

### Utilities
|**Package**|**Where**|**Description**|
|-----------|---------|----------|
|[alacritty](https://archlinux.org/packages/?name=alacritty)|pacman|Terminal|
|[firefox](https://archlinux.org/packages/?name=firefox)|pacman|Browser|
|[gedit](https://archlinux.org/packages/?name=gedit)|pacman|Text editor|
|[telegram-desktop](https://archlinux.org/packages/?name=telegram-desktop)|pacman|Messaging|
|[nemo](https://archlinux.org/packages/?name=nemo)|pacman|File explorer|
|[visual-studio-code-bin](https://aur.archlinux.org/packages/visual-studio-code-bin)|AUR|Code IDE|
