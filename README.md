# Static USB Device Names on Ubuntu

Give a USB serial device (Arduino, ESP32, USB-UART adapter) a fixed `/dev` name with a udev rule, so it does not change between reboots or replugs.

## 1. Find the device IDs

Plug in the device and run:

```bash
lsusb
```

Example output:

```
Bus 001 Device 004: ID 1234:5678 Manufacturer Name
```

- `1234` is the Vendor ID (`idVendor`)
- `5678` is the Product ID (`idProduct`)

To also get the serial number (needed when several identical devices are connected):

```bash
udevadm info -a -n /dev/ttyACM0 | grep -E 'idVendor|idProduct|serial'
```

## 2. Create the udev rules file

```bash
sudo nano /etc/udev/rules.d/99-usb-static.rules
```

The file name must end in `.rules`, otherwise udev ignores it.

## 3. Add the rule

```
SUBSYSTEM=="tty", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0042", ATTRS{serial}=="123456789", SYMLINK+="ttyACM0"
```

Replace the vendor ID, product ID and serial with your device's values. `SYMLINK+=` sets the name the device will appear under in `/dev`. A descriptive name such as `SYMLINK+="arduino_main"` avoids clashing with kernel-assigned names like `ttyACM0`.

## 4. Reload the udev rules

```bash
sudo udevadm control --reload-rules
```

## 5. Apply the new rule

```bash
sudo udevadm trigger
```

Replug the device and check that the symlink exists:

```bash
ls -l /dev/ttyACM0
```
