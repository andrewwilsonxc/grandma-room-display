# 🖥️ Grandma Room Display

A lightweight Raspberry Pi setup for displaying a web dashboard in fullscreen (kiosk) mode using **Chromium** under **Wayland** and **LabWC**.

---

## ⚙️ Setup Instructions

### 1. Add to LabWC autostart

Edit or create the file:

`~/.config/labwc/autostart`

Add the following:

```bash
#!/bin/sh

# Give the network & compositor a moment to come up
sleep 5

# Optional: periodic refresh using wtype (Wayland-friendly)
#(
#   sleep 10
#   while true; do
#       wtype -M ctrl -P r -p r
#       sleep 300   # refresh every 5 minutes
#   done
#) &

# Launch Chromium in kiosk mode under Wayland (foreground loop)
while true; do
  chromium-browser \
    --kiosk "https://andrewwilsonxc.github.io/grandma-room-display/src/" \
    --noerrdialogs \
    --disable-infobars \
    --incognito \
    --check-for-update-interval=31536000 \
    --start-fullscreen \
    --enable-features=OverlayScrollbar,UseOzonePlatform \
    --ozone-platform=wayland

  echo "[KIOSK] Chromium exited — restarting in 2s..."
  sleep 2
done
```

---

### 2. Add a nightly reboot (recommended)

For extra stability, set up a **cron job** to reboot the Raspberry Pi nightly:

```bash
sudo crontab -e
```

Add a line like this:

```bash
0 3 * * * /sbin/reboot
```

This reboots the system every night at 3 AM.

---

### 3. Connect & Enjoy

- Ensure your Pi is connected to Wi-Fi or Ethernet.  
- LabWC should automatically launch Chromium in kiosk mode on startup.  
- Your display will always stay full-screen and automatically relaunch if closed.

---

💡 **Tip:**  
You can customize the displayed URL in the script if you’re hosting your own page or dashboard.
