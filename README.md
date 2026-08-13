# FPC 10a5:a900 Linux Driver

Research and reverse-engineering project for bringing Linux support to the **FPC Sensor Controller L:0002** fingerprint sensor.

## Device

```text
Vendor:       FPC
VID:          10a5
PID:          a900
Product:      FPC Sensor Controller L:0002
Firmware:     22.26.2.29
USB Speed:    High Speed (480 Mbps)
USB Version:  1.10
```

## USB Interface

```text
Interface Class:       Vendor Specific (0xff)
Interface SubClass:   Vendor Specific (0xff)
Interface Protocol:   Vendor Specific (0xff)

Endpoint:
    0x82
    Direction: IN
    Transfer: Bulk
    Max Packet Size: 64 bytes
```

The device is detected by Linux, but there is currently no known working Linux driver for this particular device.

## Goal

The goal of this project is to investigate the communication protocol used by the Windows driver and develop Linux support.

The preferred implementation target is:

1. `libfprint`, if the device is compatible with its architecture.
2. A userspace `libusb` implementation, if appropriate.
3. A kernel USB driver only if kernel-space functionality is actually required.

## Current Status

* [x] Device identified on USB
* [x] VID/PID identified
* [x] USB descriptors collected
* [x] Windows driver available
* [ ] Analyze Windows driver
* [ ] Capture Windows USB traffic
* [ ] Identify initialization protocol
* [ ] Identify fingerprint capture commands
* [ ] Identify enrollment/authentication protocol
* [ ] Implement Linux communication layer
* [ ] Test with `libfprint`
* [ ] Develop Linux driver
* [ ] Test fingerprint enrollment
* [ ] Test fingerprint verification

## Device Identification

```text
Bus 003 Device 002: ID 10a5:a900 FPC FPC Sensor Controller L:0002 FW:22.26.2.29
```

Full descriptor information is available in:

```text
usb/lsusb.txt
usb/descriptors.txt
```

## Reverse Engineering

The Windows driver is available to the project maintainer for analysis.

The Windows driver itself is **not included in this repository** unless redistribution is legally permitted.

The investigation will focus on determining:

* USB control transfers
* Bulk transfers
* Initialization commands
* Sensor configuration
* Image acquisition
* Fingerprint enrollment
* Fingerprint matching
* Error handling
* Device reset/reinitialization
* Firmware communication

## USB Traffic

Windows USB traffic will be captured using tools such as:

* USBPcap
* Wireshark

Linux traffic may be captured using:

```bash
sudo modprobe usbmon
```

and Wireshark or:

```bash
sudo cat /sys/kernel/debug/usb/usbmon/3u
```

Capture files should be placed under:

```text
captures/
```

Do not commit personal data or actual fingerprint images/templates.

## Linux Investigation

Useful commands:

```bash
lsusb -d 10a5:a900 -v
```

```bash
lsusb -t
```

```bash
udevadm info --query=all --name=/dev/bus/usb/003/002
```

Check kernel messages:

```bash
dmesg | grep -i -E 'usb|fpc|finger'
```

## Related Projects

* libfprint
* fprintd
* Linux USB subsystem

## Contributing

If you have experience with:

* FPC fingerprint sensors
* Linux USB drivers
* libfprint
* libusb
* USB protocol reverse engineering
* Windows driver analysis
* USBPcap/Wireshark

please feel free to contribute.

## Important

This project is intended for interoperability, research, and Linux hardware support.

Please do not redistribute proprietary Windows driver binaries unless their license permits redistribution.
