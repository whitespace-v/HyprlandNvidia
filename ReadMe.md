# Honestly: The cleanest way to setup Archlinux Hyprland (nvidia)
## Setup
- GPU: NVIDIA Corporation GA104 GeForce RTX 3060 Ti Lite Hash Rate
- CPU: 12th Gen Intel(R) Core(TM) i7-12700K (20) @ 5.00 GHz
- HOST B660 DS3H DDR4
## Installing Arch: 
### lsblk
nvme:
- 1: efi: 500m [ext4]
- 2: @: 150g [fat32]
- 3: @/home: 105g [fat32]
### software during setting Arch:
pacstrap /mnt base base-devel linux-zen linux-zen-headers linux-firmware intel-ucode gdm nvidia-dkms nvidia-settings 
### boot:
common grub
### software after rebooting and logging:
1. configure yay git
2. install:
hyprland
hyprpaper
hyprlock
### get ready for hyprland:

install:
- lib32-nvidia-utils
- libvdpau
- opencl-nvidia
- lib32-opencl-nvidia
- libxnvctrl
- mesa
- lib32-mesa
- libva-mesa-driver
- mesa-vdpau
- opencl-clover-mesa
- egl-wayland
    
NOTE: DO NOT install the vulkan-intel or lib32-vulkan-intel driver as this will be used before the nvidia driver and games will not be rendered by the nvidia gpu.

set kernel parameters:

```
i915.modeset=1 nvidia_drm.modeset=1 nvidia_drm.fbdev=1 nvidia.NVreg_PreserveVideoMemoryAllocations=1
```
edit /etc/mkinitcpio.conf:

```MODULES=(i915 nvidia nvidia_modeset nvidia_uvm nvidia_drm)```

edit /etc/modprobe.d/nvidia.conf set:

```
options nvidia NVreg_RegistryDwords="PowerMizerEnable=0x1; PerfLevelSrc=0x2222; PowerMizerLevel=0x3; PowerMizerDefault=0x3; PowerMizerDefaultAC=0x3"
options nvidia_drm modeset=1 fbdev=1
```

edit hyprland.conf:

```
env = DRI_PRIME,pci-0000_01_00_0
env = __VK_LAYER_NV_optimus,NVIDIA_only 
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = LIBVA_DRIVER_NAME,nvidia
env = XDG_SESSION_TYPE,wayland
env = GBM_BACKEND,nvidia-drm

cursor {
    no_hardware_cursors = true
}
```

`sudo mkinitcpio -P`
`sudo reboot`
done +

