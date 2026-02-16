# 📺 Network-Based Android TV Management & Orchestration

> Remote discovery, control, and automation of multiple Android TV / IoT devices over a shared network using **ADB + mDNS (Avahi)**.

![Linux](https://img.shields.io/badge/Platform-Linux-black)
![ADB](https://img.shields.io/badge/Tool-ADB-green)
![Focus](https://img.shields.io/badge/Focus-Network%20Exploration-blue)
![Status](https://img.shields.io/badge/Status-Active-success)

---

## 📚 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Repository Structure](#repository-structure)
- [Key Commands](#key-commands)
- [Focus Monitoring Deep Dive](#focus-monitoring-deep-dive)
- [Real Lab Issues Encountered](#real-lab-issues-encountered)
- [Demo](#demo)
- [Learning Outcomes](#learning-outcomes)
- [Future Improvements](#future-improvements)
- [Disclaimer](#disclaimer)

---

## 🧠 Overview

In a classroom/lab network, multiple Android TVs and smart boards exposed **ADB over TCP**.  
The goal was to orchestrate them **without physical access**.

### Objectives
- Discover all devices on the network  
- Connect remotely via ADB  
- Deploy apps in batch  
- Monitor device state in real time  
- Verify execution remotely  

This repository documents the commands, workflow, and automation used.

---

## 🚀 Features

### 🔍 Service Discovery
Map active ADB services across the network using **Avahi (mDNS)**.

- Identify devices exposing ADB  
- Extract IP addresses  
- Build connection lists  

---

### 📦 Remote Deployment
Install and launch APKs across multiple devices.

- Batch `adb connect`
- Install APK to many IPs
- Launch activities remotely
- Handle multiple devices simultaneously

---

### 👁️ State Monitoring
Track what is happening on remote screens.

- Detect active app  
- Monitor focus changes  
- Debug package name issues  
- Verify launch success  

---

### 🎮 Remote Control
Send shell commands remotely.

- Simulate input  
- Adjust volume  
- Trigger recording  
- Pull files  

---

### 📹 Remote Verification
Confirm deployment using screen recording.

- Record screen remotely  
- Pull recording to host  
- Validate success  

---

## 🛠️ Key Commands

### 1️⃣ Discover Devices

```bash
avahi-browse -a -r -t
```

Save output:

```bash
avahi-browse -a -r -t > lab_services.txt
```

Extract IPs → connect with ADB.

---

### 2️⃣ Connect to Device

```bash
adb connect <device_ip>
```

Verify:

```bash
adb devices
```

---

### 3️⃣ Install APK

```bash
adb -s <device_ip> install app.apk
```

---

### 4️⃣ Check Current App (Most Important)

```bash
adb -s <device_ip> shell dumpsys window | grep mCurrentFocus
```

```ini
mCurrentFocus=Window{fc7ba18 u0 com.fingersoft.hillclimb/com.fingersoft.game.MainActivity}
```

#### Meaning

| Field    | Description                |
| -------- | -------------------------- |
| Package  | `com.fingersoft.hillclimb` |
| Activity | `MainActivity`             |
| User     | `u0`                       |

### 5️⃣ Real-Time Watch Mode
```bash
watch -n 1 "adb -s <device_ip> shell dumpsys window | grep mCurrentFocus"
```
Updates every second.

### 6️⃣ Remote Screen Recording
```bash
adb shell screenrecord /sdcard/demo.mp4
adb pull /sdcard/demo.mp4
```
Used to verify remote execution.

### ⚠️ Real Lab Issues Encountered

* Multiple devices connected simultaneously
* Devices switching to offline
* Wrong package names
* Different Android builds
* 4K codec issues
* Launch activity mismatches

**Example mismatch:**

```
com.fingersoft.hillclimb
vs
com.fingersoft.hillclimbracing
```

### 📊 Example Focus Results
| Focus Output               | Meaning        |
| -------------------------- | -------------- |
| `com.xbh.launcher`         | Home screen    |
| `com.xbh.whiteboard`       | Whiteboard app |
| `com.taishan.tvui`         | System UI      |
| `com.fingersoft.hillclimb` | Game running   |
## 🎥 Demo

Remote screen recording showing app running without physical access.

```bash
/recordings/demo.mp4
```

https://github.com/user-attachments/assets/443ccfc4-3b52-4669-ab4f-f51b6ea9144c


## 🧪 Learning Outcomes

* Network service discovery
* ADB automation
* Multi-device orchestration
* Remote observability
* Debugging distributed hardware
* Automation scripting

## 🔮 Future Improvements

* Python automation controller
* Auto-parse Avahi output
* Parallel deployment
* Web dashboard
* Device state tracker
* SSH/ADB hybrid tooling

## ⚠️ Disclaimer

This project was performed in a controlled lab environment on devices within an authorized network.

Do not attempt to access devices without permission.
