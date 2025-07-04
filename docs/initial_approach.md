# Initial Approach: Tailscale + Filebrowser

This document captures the first working setup of remote access and file browsing using Tailscale and Filebrowser on a Raspberry Pi.

---

## 📡 Remote Access via Tailscale

### ✅ On Raspberry Pi

1. **Install Tailscale:**
   ```bash
   curl -fsSL https://tailscale.com/install.sh | sh
   ```

2. **Authenticate and Connect:**
   ```bash
   sudo tailscale up
   ```
   - This will open a login URL. Visit it and log in using your account (Google/Microsoft/GitHub).
   - Once connected, run:
     ```bash
     tailscale status
     ```

3. **Verify Tailscale IP:**
   - Look for your Pi’s unique IP in the `100.x.x.x` range.

---

### ✅ On Laptop/Mac

1. Download and install Tailscale from [tailscale.com](https://tailscale.com/download).
2. Log in with the same account used on the Raspberry Pi.
3. Once connected, the Pi should appear in your Tailscale device list.
4. Use the Pi’s Tailscale IP to access services running on it.

---

## 🗂️ Filebrowser for Web Access

### 📥 Install Filebrowser on Raspberry Pi

```bash
curl -fsSL https://raw.githubusercontent.com/filebrowser/get/master/get.sh | bash
```

### 🚀 Run Filebrowser (initial test)

```bash
filebrowser -r /home/pi/temp_storage
```

- First run will display:
  - Randomly generated password for `admin`
  - Listening address: `127.0.0.1:8080` (localhost only)

### 🌐 Expose Filebrowser to Tailscale Network

To make Filebrowser accessible from other Tailscale-connected devices:

```bash
filebrowser --address 0.0.0.0
```

Now open the Filebrowser UI from your browser using:

```text
http://<tailscale-ip-of-pi>:8080
```

---

### 📱 Access from Mobile

Once the Tailscale app is installed and logged in on your phone (using the **same account** as your Raspberry Pi), you can access Filebrowser directly by visiting the Pi’s Tailscale IP in your mobile browser:

```
http://<tailscale-ip>:8080
```

✅ This works as long as the Tailscale app is **active** on your phone and the Pi is connected to the internet.  
No extra configuration is needed — secure access is handled through Tailscale's mesh VPN.


---

## ✅ Summary

- ✅ Raspberry Pi is accessible from any Tailscale-connected device using its Tailscale IP.
- ✅ Filebrowser is running and accessible over the browser.
- ✅ Ready for upload/download from mobile and laptop.

> 🛠️ Next Steps: Add reverse proxy, custom login, and UI wrappers.
