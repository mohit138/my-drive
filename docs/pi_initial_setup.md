# Raspberry Pi Self-Hosted Drive Setup

## Setup Instructions

### 1. Prepare the SD Card

- Use Raspberry Pi Imager to flash **Raspberry Pi OS (64-bit)** (Bookworm) onto SD card
- Before flashing, press `Command+Shift+X` to open advanced options:
  - Enable SSH
  - Set hostname: `raspberrypi`
  - Set username/password
  - Configure Wi-Fi and locale

### 2. First Boot & SSH Access

- Insert SD card and power on Pi
- Find IP address from router or use:
  ```bash
  ping raspberrypi.local
  ssh pi@raspberrypi.local
  ```

### 3. Fix `.local` Not Working (mDNS)

- Ensure both Mac and Pi are on **same Wi-Fi band** (2.4GHz)
- Rename 5GHz SSID if needed to force connection to 2.4GHz

### 4. Update the System

```bash
sudo apt update && sudo apt full-upgrade -y
```

### 5. (Optional) Add Fan Control (Official Pi 5 Cooler)

- Edit config:

```bash
sudo nano /boot/firmware/config.txt
```

- Add at the bottom:

```ini
dtoverlay=rpi-cooling-fan,gpiopin=14,temp=65000
```

- Reboot:

```bash
sudo reboot
```

### 6. (Optional) Check CPU Temperature

```bash
vcgencmd measure_temp
watch -n 2 vcgencmd measure_temp
```

### 7. (Optional) Run Stress Test to Trigger Fan

```bash
sudo apt install stress -y
stress --cpu 4 --timeout 60 &
watch -n 2 vcgencmd measure_temp
```

### 8. Troubleshooting Log

| Problem                             | Fix                                                     |
| ----------------------------------- | ------------------------------------------------------- |
| `.local` not resolving              | Ensure both devices are on same band (2.4GHz)           |
| Loud fan at idle                    | Fan overlay missing → added `dtoverlay=rpi-cooling-fan` |
| Missing fan control in raspi-config | OS updated, added fan overlay manually                  |
| Imager verification stuck           | Reflashed SD card after restart                         |
