
These udev rules do the following things;

 * Generate /dev/hdmi2usb symlinks.
 * Grant anyone on the system permission to access the HDMI2USB boards.
 * Make modem-manager ignore the serial ports.

They work the following way;

 * `98-hdmi2usb-xxx.rules` - These files tag the ENV with the HDMI2USB
   information.
  - `98-hdmi2usb-opsis.rules` - Rules for the Numato Opsis board.
  - `98-hdmi2usb-atlys.rules` - Rules for the Digilent Atlys board.

 * `99-hdmi2usb-xxx.rules` - These files use the tags to do things.
  - `99-hdmi2usb-aliases.rules` - Creates the extra symlinks
  - `99-hdmi2usb-permissions.rules` - Sets the permissions
  - `99-hdmi2usb-mm-blacklist.rules` - Makes modem manager ignore the device

 * `99-hdmi2usbaux-xxx.rules` - These files are used for HDMI2USB related
   devices which don't really follow the proper rules.
  - `99-hdmi2usbaux-cypress.rules` - Rules for an unconfigured Cypress FX2
    (such as the Numato Opsis/Digilent Atlys in fail-safe mode).
  - `99-hdmi2usbaux-ixo-usb-jtag.rules` - Rules for the boards when loaded with
    ixo-usb-jtag.

 * `hdmi2usb-human-path-helper.sh` is just a simple shell script which converts
   the kernel naming into something human readable.

# Examining udev data

```
udevadm info --attribute-walk --name=/dev/ttyACM0
udevadm test /sys/class/tty/ttyACM0 2>&1 | less
udevadm test /sys/class/video4linux/video0 2>&1 | less
udevadm test $(udevadm info -q path -n /dev/bus/usb/003/109) 2>&1 | less
```

Useful docs:
 * [udev Linux Man Page](http://linux.die.net/man/8/udev)
 * [Linux kernel udev docs](https://www.kernel.org/pub/linux/utils/kernel/hotplug/udev/udev.html)
 * [Debian Wiki](https://wiki.debian.org/udev)
 * [Oracle Docs on Device Management](https://docs.oracle.com/cd/E37670_01/E41138/html/ol_devices.html)
