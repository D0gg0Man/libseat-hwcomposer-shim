**libseat-hwcomposer-shim**

LD_PRELOAD shim for running phoc/phosh on FuriOS with a Mali GPU via
the wlroots hwcomposer backend (Android HWC2).

Tested on Furiphone FLX1 (Dimensity 900, Mali-G68 MC4) running FuriOS.

*What it does*

* libseat -- fake API opening devices with O_NONBLOCK (critical: without
  it the GLib main loop blocks in evdev_read and frames never fire)
* DRM caps -- patches drmGetDevice2 to advertise a render node; fixes
  drmGetCap for PRIME/vblank/timestamp; stubs drmModeCreateLease
* EGL visual-id -- returns HAL_PIXEL_FORMAT_RGBA_8888 (1) for RGBA8888
  configs where Android EGL driver returns 0
* HWC2 vsync -- always returns HWC2_ERROR_NONE from set_vsync_enabled
  so schedule_frame() is called and phosh receives frame callbacks
* Buffer shims -- forces 2 HWC buffers; discards redundant acquire fences

*Building*

```
make
sudo make install LIBDIR=/usr/lib/aarch64-linux-gnu
```

*See also*

* https://github.com/D0gg0Man/drm-mali
* https://github.com/D0gg0Man/phosh-hwcomposer-session
* https://github.com/FuriLabs/libdrm-hybris
