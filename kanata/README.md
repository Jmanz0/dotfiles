````md
# Kanata Setup on macOS (Simple Auto-Start)

This setup runs Kanata in the background on macOS at boot using `launchd`.

---

## 1. Install Kanata

```bash
brew install kanata
````

---

## 2. Create Kanata config

Create your keymap file:

```bash
~/.config/kanata/kanata.kbd
```

Example (you define your own layout).

Tip: you can add a reload key inside Kanata config (`lrld`) to reload without restarting the service.

---

## 3. Create LaunchDaemon

Create the daemon file:

```bash
sudo nano /Library/LaunchDaemons/org.custom.kanata.plist
```

Paste:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
 "http://www.apple.com/Dtds/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>org.custom.kanata</string>

  <key>ProgramArguments</key>
  <array>
    <string>/opt/homebrew/bin/kanata</string>
    <string>--cfg</string>
    <string>/Users/YOUR_USER/.config/kanata/kanata.kbd</string>
  </array>

  <key>RunAtLoad</key>
  <true/>

  <key>KeepAlive</key>
  <true/>
</dict>
</plist>
```

Replace `YOUR_USER` with your macOS username:

```bash
whoami
```

---

## 4. Fix permissions

```bash
sudo chown root:wheel /Library/LaunchDaemons/org.custom.kanata.plist
sudo chmod 644 /Library/LaunchDaemons/org.custom.kanata.plist
```

---

## 5. Enable Kanata at startup

```bash
sudo launchctl load /Library/LaunchDaemons/org.custom.kanata.plist
```

Kanata will now:

* start on boot
* run in the background
* restart automatically if it crashes

---

## 6. Stop Kanata

```bash
sudo launchctl unload /Library/LaunchDaemons/org.custom.kanata.plist
```

---

## 7. Debugging

Check logs:

```bash
cat /tmp/kanata.err
cat /tmp/kanata.log
```

Check process:

```bash
ps aux | grep kanata
```
