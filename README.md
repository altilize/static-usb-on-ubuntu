# Tutorial static name usb on ubuntu by Moskes

### 1. Colok USB, kemudian jalankan perintah: 
  lsusb

### 2. Anda akan melihat output seperti ini:
  Bus 001 Device 004: ID 1234:5678 Manufacturer Name
- 1234 adalah Vendor ID
- 5678 adalah Product ID

### 3. Buat atau edit file aturan udev:
  sudo nano /etc/udev/rules.d/99-usb-static.rule0073

### 4. Tambahkan aturan berikut di dalam file:
  SUBSYSTEM=="tty", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0042", ATTRS{serial}=="123456789", SYMLINK+="ttyACM0"

### 5. Reload aturan udev dengan perintah:
  sudo udevadm control --reload-rules

### 6. Jalankan perintah untuk memicu aturan baru:
  sudo udevadm trigger
