# 🔐 Aircrack-ng Suite Cheatsheet

A comprehensive reference for wireless security testing with the Aircrack-ng suite.


---

## 🔧 Monitor Mode Setup

=== "Basic Setup"

    ### **Method 1: Standard Monitor Mode**
    ```bash
    # Check available interfaces
    iwconfig

    # Enable monitor mode
    sudo airmon-ng start wlan0

    # Verify monitor mode
    iwconfig
    ```

    !!! tip "Basic Setup"
        - **Pros**: Simple, straightforward
        - **Cons**: May conflict with NetworkManager
        - **Best for**: Dedicated testing machines

=== "Check Kill Method"

    ### **Method 2: Check Kill Setup**
    ```bash
    # Kill interfering processes
    sudo airmon-ng check kill

    # Start monitor mode
    sudo airmon-ng start wlan0

    # Verify interface
    iwconfig
    ```

    !!! warning "Check Kill Method"
        - **Pros**: Resolves most conflicts
        - **Cons**: Disrupts network services
        - **Best for**: Systems with network conflicts

=== "Manual Setup"

    ### **Method 3: Manual Monitor Mode**
    ```bash
    # Bring interface down
    sudo ifconfig wlan0 down

    # Set monitor mode
    sudo iwconfig wlan0 mode monitor

    # Bring interface up
    sudo ifconfig wlan0 up

    # Verify mode
    iwconfig
    ```

    !!! danger "Manual Setup"
        - **Pros**: Full control, bypasses airmon-ng
        - **Cons**: More complex, requires manual management
        - **Best for**: Advanced users, troubleshooting

---

## 🔍 AP Discovery

=== "Basic Scanning"

    ### **Method 1: Basic AP Scan**
    ```bash
    # Scan all channels
    sudo airodump-ng wlan0mon

    # Scan specific channel
    sudo airodump-ng -c 6 wlan0mon

    # Scan multiple channels
    sudo airodump-ng -c 1,6,11 wlan0mon
    ```

    !!! tip "Basic Scanning"
        - **Pros**: Simple, shows all networks
        - **Cons**: May be overwhelming with many APs
        - **Best for**: Initial reconnaissance

=== "Target Specific"

    ### **Method 2: Target Specific Networks**
    ```bash
    # Target by BSSID
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF wlan0mon

    # Target by ESSID (network name)
    sudo airodump-ng -c 6 --essid "TargetNetwork" wlan0mon

    # Target with file output
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w capture wlan0mon
    ```

    !!! info "Target Specific"
        - **Pros**: Focused, less noise
        - **Cons**: Requires prior knowledge of target
        - **Best for**: Known targets, specific assessments

=== "Advanced Filtering"

    ### **Method 3: Advanced Filtering**
    ```bash
    # Filter by encryption
    sudo airodump-ng --encrypt WPA wlan0mon

    # Filter by manufacturer
    sudo airodump-ng --manufacturer "Cisco" wlan0mon

    # Filter by signal strength
    sudo airodump-ng --manufacturer "Netgear" wlan0mon
    ```

    !!! warning "Advanced Filtering"
        - **Pros**: Highly targeted, efficient
        - **Cons**: May miss important targets
        - **Best for**: Specific vulnerability assessments

---

## ⚡ Capture Handshake

=== "Passive Capture"

    ### **Method 1: Passive Capture**
    ```bash
    # Start capture
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w handshake wlan0mon

    # Wait for client to connect naturally
    # Monitor for WPA handshake in top-right corner
    ```

    !!! tip "Passive Capture"
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

    !!! warning "Active Deauth"
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

    !!! danger "Fake AP Attack"
        - **Pros**: Can capture multiple handshakes
        - **Cons**: Highly detectable, requires careful setup
        - **Best for**: Advanced testing, social engineering scenarios

---

## 🔓 Crack Handshake

=== "Dictionary Attack"

    ### **Method 1: Dictionary Attack**
    ```bash
    # Basic dictionary attack
    sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt handshake-01.cap

    # Custom wordlist
    sudo aircrack-ng -w /path/to/custom.txt handshake-01.cap

    # Multiple wordlists
    sudo aircrack-ng -w wordlist1.txt,wordlist2.txt handshake-01.cap
    ```

    !!! tip "Dictionary Attack"
        - **Pros**: Fast with good wordlists, common passwords
        - **Cons**: Limited to wordlist contents
        - **Best for**: Common passwords, social engineering

=== "Brute Force Attack"

    ### **Method 2: Brute Force Attack**
    ```bash
    # 4-way handshake brute force
    sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b AA:BB:CC:DD:EE:FF handshake-01.cap

    # PMKID attack (if supported)
    sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -r PMKID handshake-01.cap
    ```

    !!! warning "Brute Force Attack"
        - **Pros**: Comprehensive, finds complex passwords
        - **Cons**: Very slow, resource intensive
        - **Best for**: Short passwords, targeted attacks

=== "Hashcat Integration"

    ### **Method 3: Hashcat Integration**
    ```bash
    # Convert cap to hccapx
    cap2hccapx handshake-01.cap handshake.hccapx

    # Hashcat attack
    hashcat -m 2500 handshake.hccapx /usr/share/wordlists/rockyou.txt

    # GPU acceleration
    hashcat -m 2500 -d 1 handshake.hccapx /usr/share/wordlists/rockyou.txt
    ```

    !!! danger "Hashcat Integration"
        - **Pros**: GPU acceleration, advanced techniques
        - **Cons**: Requires powerful hardware, complex setup
        - **Best for**: Professional assessments, high-performance cracking

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

## 📋 Quick Reference

### **Essential Commands**
| Task | Command |
|------|---------|
| **Start Monitor** | `sudo airmon-ng start wlan0` |
| **Check Kill** | `sudo airmon-ng check kill` |
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
