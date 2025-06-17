# 🖱️ libinput-gestures Setup for Ubuntu 24.04

Enable smooth 3-finger touchpad gestures to control your desktop — like workspace switching and shortcut emulation — using `libinput-gestures` and `xdotool`.

---

## 📦 Installation

```bash
sudo apt install libinput-tools xdotool wmctrl
git clone https://github.com/bulletmark/libinput-gestures.git
cd libinput-gestures
sudo make install
```

---

## 🚀 Enable Gestures on Startup

```bash
libinput-gestures-setup autostart
libinput-gestures-setup start
```

If the above doesn't start it on login, use this workaround.

---

## 🛠️ The `.xprofile` Method (Startup Fix)

1. Open `.xprofile` (create if it doesn’t exist):

   ```bash
   nano ~/.xprofile
   ```

2. Add this line to run gestures on login:

   ```bash
   libinput-gestures &
   ```

3. Make it executable (important!):

   ```bash
   chmod +x ~/.xprofile
   ```

This ensures the gesture daemon runs even when `autostart` fails (like on some GNOME/Wayland setups).

---

## 🔍 Detecting Your Touchpad

Make sure your touchpad is detected:

```bash
libinput list-devices
```

---

## 🧠 X11 vs Wayland

* Run `echo $XDG_SESSION_TYPE`

  * `x11` = you're on X11 (xdotool works)
  * `wayland` = use `_internal` or DBus workarounds

---

## ✏️ Config File Paths

* **System-wide**: `/etc/libinput-gestures.conf`
* **User-specific (preferred)**: `~/.config/libinput-gestures.conf`

If both exist, the user one takes precedence.

---

## ✏️ Recommended Custom Gestures

Edit `/etc/libinput-gestures.conf` or `~/.config/libinput-gestures.conf`:

```ini
gesture swipe right   xdotool key ctrl+alt+Right
gesture swipe left    xdotool key ctrl+alt+Left
gesture swipe down    xdotool key super+d
gesture swipe up      _internal ws_up
```

Restart:

```bash
libinput-gestures-setup restart
```

---

## 🧠 What’s `_internal ws_up`?

* `_internal` = built-in command using `wmctrl`
* `ws_up` = move to upper workspace
* Works on both Xorg and Wayland (via XWayland)

---

## 🧪 Debug Gestures

```bash
libinput-gestures -v
```

Shows real-time gesture detection and action mapping.

---

## 🧯 Troubleshooting

* Not starting on boot? Use `.xprofile` method.
* Wrong gestures firing? Check if `~/.config/libinput-gestures.conf` overrides `/etc/`.
* Wayland + `xdotool` doesn’t work: use `_internal` or native Wayland actions via DBus.

---

## 💡 Tips

* Use `xdotool` for keyboard emulation (Xorg only)
* Use `_internal` for workspace movement (portable)
* Add `-cols 4` for grid-like workspace layouts:

  ```ini
  gesture swipe up   _internal --cols 4 ws_up
  ```
