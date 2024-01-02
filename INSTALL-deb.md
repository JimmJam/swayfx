## A guide to installing swayfx on Debian
( You may have to adapt this guide to install on other distros )

### First install all required dependencies and download the source code

```bash
sudo apt install -y build-essential cmake meson libwayland-dev wayland-protocols libegl1-mesa-dev libgles2-mesa-dev libdrm-dev libgbm-dev libinput-dev libxkbcommon-dev libudev-dev libpixman-1-dev libsystemd-dev libcap-dev libxcb1-dev libxcb-composite0-dev libxcb-xfixes0-dev libxcb-xinput-dev libxcb-image0-dev libxcb-render-util0-dev libx11-xcb-dev libxcb-icccm4-dev freerdp2-dev libwinpr2-dev libpng-dev libavutil-dev libavcodec-dev libavformat-dev universal-ctags git wget ninja-build cmake-extras gettext gettext-base fontconfig libfontconfig-dev libffi-dev libxml2-dev libxkbcommon-x11-dev libxkbregistry-dev libseat-dev seatd libxcb-dri3-dev libegl-dev glslang-tools libinput-bin libxcb-ewmh2 libxcb-ewmh-dev libxcb-present-dev libxcb-res0-dev xdg-desktop-portal-wlr libtomlplusplus3 hwdata xwayland
```

### Now setup SwayFX/Wlroots' build enviroment
```bash
mkdir ~/build
cd ~/build # Or whatever you have named it
# Download display server libraries
wget https://gitlab.freedesktop.org/wayland/wayland/-/releases/1.22.0/downloads/wayland-1.22.0.tar.xz
tar -xvJf wayland-1.22.0.tar.xz
rm wayland-1.22.0.tar.xz

# Download additional protocols
wget https://gitlab.freedesktop.org/wayland/wayland-protocols/-/releases/1.31/downloads/wayland-protocols-1.31.tar.xz
tar -xvJf wayland-protocols-1.31.tar.xz
rm wayland-protocols-1.31.tar.xz

# Download display info library
wget https://gitlab.freedesktop.org/emersion/libdisplay-info/-/releases/0.1.1/downloads/libdisplay-info-0.1.1.tar.xz
tar -xvJf libdisplay-info-0.1.1.tar.xz
rm libdisplay-info-0.1.1.tar.xz

# Downloading Wlroots
wget https://gitlab.freedesktop.org/wlroots/wlroots/-/archive/0.16.2/wlroots-0.16.2.tar.gz
tar -xf wlroots-0.16.2.tar.gz
rm wlroots-0.16.2.tar.gz

# Downloaing Swayfx
wget https://github.com/WillPower3309/swayfx/archive/refs/tags/0.3.2.tar.gz
tar -xf 0.3.2.tar.gz
rm 0.3.2.tar.gz
```
swayfx and wlroots should now be located in the `build` directory.
```
.
├── swayfx-0.3.2
├── wlroots-0.16.2
```
___
### Compiling
You MUST compile Wayland-related libraries before Wlroots.
```bash
# Compile the display server libraries
cd wayland-1.22.0
mkdir build && cd build
meson setup .. --prefix=/usr --buildtype=release -Ddocumentation=false
ninja
sudo ninja install
cd ../..
# Compile additional Wayland protocols
cd wayland-protocols-1.31
mkdir build && cd build
meson setup --prefix=/usr --buildtype=release
ninja
sudo ninja install
cd ../..
# Compile display info libraries
cd libdisplay-info-0.1.1
mkdir build && cd build
meson setup --prefix=/usr --buildtype=release
```

Then, compile Wlroots before SwayFX.
```bash
cd wlroots-0.16.2
meson setup build/
ninja -C build/
```

Finally, compile swayfx.
```bash
cd swayfx-0.3.2
meson build/
ninja -C build/
sudo ninja -C build/ install
```
Reboot and then add the desired effects in your ~/.config/sway/config file <br/>
e.g. `blur enable|disable`

+ Guide created with ♥️ by babymusk
