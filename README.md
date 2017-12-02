# How to configure LG 25UM58 on Linux

The [LG 25UM58-P](http://www.lg.com/us/monitors/lg-25UM58-P-ultrawide-monitor) monitor
can be tricky to configure.

The open-source Nouveau driver for NVIDIA graphics cards refuses to accept
2560x1080 resolution, as does the macOS NVIDIA driver.

The proprietary driver for NVIDIA automatically supports every resolution.

## Extracting EDID information

After installing and booting the system with the proprietary NVIDIA driver, run:

```
$ nvidia-xconfig --extract-edids-from-file=/var/log/Xorg.0.log --extract-edids-output-file=./LG-25UM58-P.bin

Found 1 EDID in "/var/log/Xorg.0.log".
  Wrote EDID for "LG Electronics LG ULTRAWIDE (DFP-1)" to "./LG-25UM58-P.bin" (256 bytes).

```

Note mine was at **DFP-1**.

## Configure xorg to read custom EDID file

Copy `LG-25UM58-P.bin` to `/etc/X11/LG-25UM58-P.bin` and
change the `Section "Device"` on your `/etc/X11/xorg.conf`:

```
    Option         "ConnectedMonitor" "DFP-0"
    Option         "CustomEDID" "DFP-0:/etc/X11/edid.bin"
    Option         "IgnoreEDID" "false"
    Option         "UseEDID" "true"
```

