# Fedora 37 Post Install Guide
Things to do after installing Fedora 37

## Faster Updates
* `sudo nano /etc/dnf/dnf.conf` 
* Copy and replace the text with the following:
```
[main] 
gpgcheck=1 
installonly_limit=3 
clean_requirements_on_remove=True 
best=False 
skip_if_unavailable=True 
fastestmirror=1
max_parallel_downloads=10 
deltarpm=true
``` 
* Note: The `fastestmirror=1` plugin can be counterproductive at times, use it at your own discretion. Set it to `fastestmirror=0` if you are facing bad download speeds. Many users have reported better download speeds with the plugin enables so it is there by default.

## Fix CKB-NEXT
You need to install from source again. Never use RPM package

* `sudo dnf install gcc gcc-c++ make cmake glibc zlib-devel qt5-qtbase-devel quazip-qt5-devel systemd-devel pulseaudio-libs-devel qt5-linguist qt5-qtx11extras-devel xcb-util-wm-devel xcb-util-devel libxcb-devel git dbusmenu-qt5-devel`
* `cd /home/alan/ckb-next/`
* `git pull`
* `./quickinstall`

##Install fonts
* `sudo dnf install -y 'google-roboto*' 'mozilla-fira*' fira-code-fonts`


## REPOSLIST CONFIGURATION

Actual list of repos:
```
repo id                                                                   repo name
anydesk                                                                   AnyDesk Fedora - stable
balena-etcher                                                             balena-etcher
balena-etcher-noarch                                                      balena-etcher-noarch
balena-etcher-source                                                      balena-etcher-source
brave-browser-rpm-release.s3.brave.com_x86_64_                            created by dnf config-manager from https://brave-browser-rpm-release.s3.brave.com/x86_64/
copr:copr.fedorainfracloud.org:atim:gping                                 Copr repo for gping owned by atim
copr:copr.fedorainfracloud.org:atim:lazydocker                            Copr repo for lazydocker owned by atim
copr:copr.fedorainfracloud.org:dwmw2:openconnect                          Copr repo for openconnect owned by dwmw2
copr:copr.fedorainfracloud.org:oprizal:timeshift-upstream                 Copr repo for timeshift-upstream owned by oprizal
copr:copr.fedorainfracloud.org:t0xic0der:nvidia-auto-installer-for-fedora Copr repo for nvidia-auto-installer-for-fedora owned by t0xic0der
copr:copr.fedorainfracloud.org:zirix:gdm-wallpaper                        Copr repo for gdm-wallpaper owned by zirix
fedora                                                                    Fedora 37 - x86_64
fedora-cisco-openh264                                                     Fedora 37 openh264 (From Cisco) - x86_64
fedora-modular                                                            Fedora Modular 37 - x86_64
hashicorp                                                                 Hashicorp Stable - x86_64
phracek-PyCharm                                                           Copr repo for PyCharm owned by phracek
rpmfusion-free                                                            RPM Fusion for Fedora 37 - Free
rpmfusion-free-tainted                                                    RPM Fusion for Fedora 37 - Free tainted
rpmfusion-free-updates                                                    RPM Fusion for Fedora 37 - Free - Updates
rpmfusion-nonfree                                                         RPM Fusion for Fedora 37 - Nonfree
rpmfusion-nonfree-nvidia-driver                                           RPM Fusion for Fedora 37 - Nonfree - NVIDIA Driver
rpmfusion-nonfree-steam                                                   RPM Fusion for Fedora 37 - Nonfree - Steam
rpmfusion-nonfree-updates                                                 RPM Fusion for Fedora 37 - Nonfree - Updates
rpmsphere                                                                 RPM Sphere - Basearch
rpmsphere-noarch                                                          RPM Sphere - Noarch
sandrospadaro                                                             RPM by Sandro Spadaro
teamviewer                                                                TeamViewer - x86_64
updates                                                                   Fedora 37 - x86_64 - Updates
updates-modular                                                           Fedora Modular 37 - x86_64 - Updates
virtualbox                                                                Fedora  -  - VirtualBox
vscode                                                                    Visual Studio Code
```
## Update system configuration files

* `sudo dnf install rpmconf`
* `sudo rpmconf -a - Yes for all`  

## Clean-up retired packages

* `sudo dnf install remove-retired-packages`
* `remove-retired-packages`
* `sudo dnf install remove-retired-packages`
* `remove-retired-packages 35`
* `sudo dnf repoquery --unsatisfied`
* `sudo dnf repoquery --duplicates`


## Remove old kernels
run script ~/./removeoldkernels

Exemple:
```
#!/usr/bin/env bash

old_kernels=($(dnf repoquery --installonly --latest-limit=-1 -q))
if [ "${#old_kernels[@]}" -eq 0 ]; then
    echo "No old kernels found"
    exit 0
fi

if ! dnf remove "${old_kernels[@]}"; then
    echo "Failed to remove old kernels"
    exit 1
fi

echo "Removed old kernels"
exit 0
```
![image](https://user-images.githubusercontent.com/20565821/208081868-3aee6f37-f453-4715-88ae-43318bcf01f1.png)



## Firmware
* If your system supports firmware update delivery through lvfs, update your device firmware by:
```
sudo fwupdmgr get-devices 
sudo fwupdmgr refresh --force 
sudo fwupdmgr get-updates 
sudo fwupdmgr update
```

## NVIDIA Drivers - Nouveu driver cannot load UI
You must remove old driver and reinstall it. If screen remains only prompting press F6, F2 or something to load terminal.
```
sudo dnf remove \*nvidia\* --exclude=nvidia-gpu-firmware 
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
```

### OpenH264 for Firefox
* Enable the OpenH264 Plugin in Firefox's settings.

## Update Flatpak

* `flatpak update`


# Terminal font and config

![image](https://user-images.githubusercontent.com/20565821/208084409-8466e335-ec91-4377-bf20-b06771e0f422.png)


## Gnome Extensions

![image](https://user-images.githubusercontent.com/20565821/208085611-f4e2afb7-fba9-4da5-83ad-7724bb12f5d6.png)
![image](https://user-images.githubusercontent.com/20565821/208085734-369fb552-b4d1-4f65-b98d-eca35420eea2.png)


## Search duplicating pressed key in Gnome
Go to Gnome extensions and disable and enable again DashtoDock extension
https://extensions.gnome.org/extension/307/dash-to-dock/

![image](https://user-images.githubusercontent.com/20565821/208087269-836e82c6-073c-401a-9c03-ec0f4dea0052.png)


## Theming [Optional]

### GTK Themes
* Don't install these if you are using a different spin of Fedora.
* https://github.com/lassekongo83/adw-gtk3
* https://github.com/vinceliuice/Colloid-gtk-theme
* https://github.com/EliverLara/Nordic
* https://github.com/vinceliuice/Orchis-theme
* https://github.com/vinceliuice/Graphite-gtk-theme

### Use themes in Flatpaks
* `sudo flatpak override --filesystem=$HOME/.themes`
* `sudo flatpak override --env=GTK_THEME=my-theme` 

### Icon Packs
* https://github.com/vinceliuice/Tela-icon-theme
* https://github.com/vinceliuice/Colloid-gtk-theme/tree/main/icon-theme

### Wallpaper
* https://github.com/manishprivet/dynamic-gnome-wallpapers

### Firefox Theme
* Install Firefox Gnome theme by: `curl -s -o- https://raw.githubusercontent.com/rafaelmardojai/firefox-gnome-theme/master/scripts/install-by-curl.sh | bash`

### Grub Theme
* https://github.com/vinceliuice/grub2-themes
