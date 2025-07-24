# Ensure GPU & VPU Support
```
OS: Ubuntu 22.04.5 LTS aarch64 
Host: sun55iw3 
Kernel: 5.15.147-sun55iw3 
Uptime: 5 hours, 2 mins 
Packages: 2737 (dpkg), 25 (flatpak) 
Shell: bash 5.1.16 
Resolution: 1920x1080 
DE: GNOME 42.9 (Wayland) 
Theme: Yaru-dark [GTK2/3] 
Icons: Yaru [GTK2/3] 
Terminal: gnome-terminal 
CPU: Cortex-A55 (8) @ 1.416GHz 
GPU: Mali-G57 (Panfrost) 
Memory: 2818MiB / 3840MiB 
```
## Hardware VPU Drivers Check
```
$ sudo dmesg | grep cedar
[ 9138.577451] sunxi:VE:[INFO]: 912 enable_cedar_hw_clk(): 
[ 9138.578305] sunxi:VE:[INFO]: 954 disable_cedar_hw_clk(): 
[ 9138.578478] sunxi:VE:[INFO]: 912 enable_cedar_hw_clk(): 
[ 9291.838428] sunxi:VE:[INFO]: 954 disable_cedar_hw_clk(): 
[ 9444.989250] sunxi:VE:[INFO]: 912 enable_cedar_hw_clk(): 
[ 9444.990216] sunxi:VE:[INFO]: 954 disable_cedar_hw_clk(): 
[ 9444.990394] sunxi:VE:[INFO]: 912 enable_cedar_hw_clk(): 
[13326.578046] sunxi:VE:[WARN]: 2217 cedardev_release(): release lost-lock...
```
## Github Links
- GPU VPU configuration files, must be present in your OS https://github.com/orangepi-xunlong/orangepi-build/tree/next/external/packages/bsp/t527/etc
- **iMPORTANT** Prebuilt *.deb files for GPU & VPU https://github.com/orangepi-xunlong/rk-rootfs-build/tree/t527_packages/jammy
For VPU the most important package is `gstreamer1.0-omx.deb` refer to this dicussion https://github.com/orangepi-xunlong/orangepi-build/issues/244
## VPU Codecs Support
HDR files are not supported as well as HEVC 10bit
Required packages `sudo apt-get install gstreamer1.0 gstreamer1.0-tools` You can install additional codecs `sudo apt install gstreamer1.0-plugins-{base,good,bad}` but **never** install `*-omx-generic*` packages
```
$ gst-inspect-1.0 | grep omx
libav:  avenc_h264_omx: libav OpenMAX IL H.264 video encoder encoder
libav:  avenc_mpeg4_omx: libav OpenMAX IL MPEG-4 video encoder encoder
omx:  omxavsvideodec: OpenMAX AVS Video Decoder
omx:  omxh263videodec: OpenMAX H.263 Video Decoder
omx:  omxh264dec: OpenMAX H.264 Video Decoder
omx:  omxhevcvideodec: OpenMAX H.265 Video Decoder
omx:  omxmjpegvideodec: OpenMAX MJPEG Video Decoder
omx:  omxmpeg1videodec: OpenMAX MPEG1 Video Decoder
omx:  omxmpeg2videodec: OpenMAX MPEG2 Video Decoder
omx:  omxmpeg4videodec: OpenMAX MPEG4 Video Decoder
omx:  omxvp8videodec: OpenMAX VP8 Video Decoder
omx:  omxvp9videodec: OpenMAX VP9 Video Decoder
```
## Gstreamer Video Players
Command line player with very absolutely no GUI is `gst123` More about its usage 
