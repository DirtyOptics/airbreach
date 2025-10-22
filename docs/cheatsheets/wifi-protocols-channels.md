# 📡 WiFi Protocols & Channels Cheatsheet

A comprehensive reference for wireless standards, frequencies, and regulatory requirements in Australia.

---

## 📋 Summary - WiFi in Australia

| Band | Frequency Range | Channels (AU) | Max Width | DFS Required<span title="Dynamic Frequency Selection - Required for channels that may interfere with radar systems"> ℹ️</span> | Indoor/Outdoor |
|------|----------------|---------------|-----------|--------------|----------------|
| **2.4GHz** | 2.412-2.472 GHz | 1-13 | 40 MHz | ❌ No | Both |
| **5GHz** | 5.150-5.850 GHz | 36-165 | 160 MHz | ✅ Yes (UNII-2) | Both |
| **6GHz** | 5.925-7.125 GHz | 1-233 | 320 MHz | ❌ No | Both |

---

## 📶 2.4GHz Band (AU)

=== "Standards & Channels"
    
    **802.11b:**
    ```text
    Frequency: 2.4GHz
    Max Speed: 11 Mbps
    Range: ~100m (indoor), ~300m (outdoor)
    Security: WEP (deprecated)
    ```

    **802.11g:**
    ```text
    Frequency: 2.4GHz
    Max Speed: 54 Mbps
    Range: ~100m (indoor), ~300m (outdoor)
    Security: WPA/WPA2
    ```

    **802.11n:**
    ```text
    Frequency: 2.4GHz + 5GHz
    Max Speed: 600 Mbps (4x4 MIMO)
    Range: ~150m (indoor), ~400m (outdoor)
    Security: WPA2
    ```

    **802.11ax (WiFi 6) - 2.4GHz:**
    ```text
    Frequency: 2.4GHz
    Max Speed: 1.2 Gbps
    Range: ~150m (indoor), ~400m (outdoor)
    Security: WPA3
    ```

=== "Channel Allocation"
    
    **Available Channels:**
    ```text
    Channels 1-13: 2.412 - 2.472 GHz
    Channel Width: 20 MHz (standard), 40 MHz (bonded)
    Non-overlapping: 1, 6, 11 (20 MHz)
    ```

    **Channel Frequencies:**
    ```text
    Channel 1:  2.412 GHz
    Channel 2:  2.417 GHz
    Channel 3:  2.422 GHz
    Channel 4:  2.427 GHz
    Channel 5:  2.432 GHz
    Channel 6:  2.437 GHz
    Channel 7:  2.442 GHz
    Channel 8:  2.447 GHz
    Channel 9:  2.452 GHz
    Channel 10: 2.457 GHz
    Channel 11: 2.462 GHz
    Channel 12: 2.467 GHz
    Channel 13: 2.472 GHz
    ```

    **Channel Bonding (40 MHz):**
    ```text
    20 MHz + 20 MHz = 40 MHz
    Primary: 6, Secondary: 2 (6+2)
    Primary: 6, Secondary: 10 (6+10)
    ```

=== "Bandwidth & Performance"
    
    **Theoretical Speeds:**
    ```text
    802.11b: 11 Mbps
    802.11g: 54 Mbps
    802.11n: 150 Mbps (1x1), 300 Mbps (2x2)
    802.11ax: 287 Mbps (1x1), 1.2 Gbps (4x4)
    ```

    **Real-world Speeds:**
    ```text
    802.11b: ~5 Mbps
    802.11g: ~25 Mbps
    802.11n: ~75 Mbps (1x1), ~150 Mbps (2x2)
    802.11ax: ~150 Mbps (1x1), ~600 Mbps (4x4)
    ```

    **Range Characteristics:**
    ```text
    Indoor: 30-100m (depending on walls)
    Outdoor: 100-300m (line of sight)
    Penetration: Good through walls
    Interference: High (crowded band)
    ```

---

## 📡 5GHz Band (AU)

=== "Standards & Channels"
    
    **802.11a:**
    ```text
    Frequency: 5GHz
    Max Speed: 54 Mbps
    Range: ~50m (indoor), ~150m (outdoor)
    Security: WEP (deprecated)
    ```

    **802.11n:**
    ```text
    Frequency: 5GHz
    Max Speed: 600 Mbps (4x4 MIMO)
    Range: ~100m (indoor), ~300m (outdoor)
    Security: WPA2
    ```

    **802.11ac:**
    ```text
    Frequency: 5GHz
    Max Speed: 6.77 Gbps (8x8 MIMO)
    Range: ~100m (indoor), ~300m (outdoor)
    Security: WPA2
    ```

    **802.11ax (WiFi 6) - 5GHz:**
    ```text
    Frequency: 5GHz
    Max Speed: 9.6 Gbps
    Range: ~100m (indoor), ~300m (outdoor)
    Security: WPA3
    ```

=== "Channel Allocation"
    
    **UNII-1 Band (5.150-5.250 GHz):**
    ```text
    Channels 36-48: 5.180-5.240 GHz
    Channel Width: 20 MHz
    Indoor use only
    ```

    **UNII-2A Band (5.250-5.350 GHz):**
    ```text
    Channels 52-64: 5.260-5.320 GHz
    Channel Width: 20 MHz
    DFS required (Dynamic Frequency Selection)
    ```

    **UNII-2C Band (5.470-5.725 GHz):**
    ```text
    Channels 100-140: 5.500-5.700 GHz
    Channel Width: 20 MHz
    DFS required
    ```

    **UNII-3 Band (5.725-5.850 GHz):**
    ```text
    Channels 149-165: 5.735-5.825 GHz
    Channel Width: 20 MHz
    Outdoor use only
    ```

=== "Bandwidth & Performance"
    
    **Theoretical Speeds:**
    ```text
    802.11a: 54 Mbps
    802.11n: 150 Mbps (1x1), 300 Mbps (2x2)
    802.11ac: 433 Mbps (1x1), 1.3 Gbps (3x3)
    802.11ax: 600 Mbps (1x1), 9.6 Gbps (8x8)
    ```

    **Channel Bonding:**
    ```text
    20 MHz: Standard channel width
    40 MHz: 2x20 MHz channels bonded
    80 MHz: 4x20 MHz channels bonded
    160 MHz: 8x20 MHz channels bonded
    ```

    **Range Characteristics:**
    ```text
    Indoor: 50-100m (shorter than 2.4GHz)
    Outdoor: 150-300m (line of sight)
    Penetration: Poor through walls
    Interference: Low (less crowded)
    ```

---

## 🚀 WiFi 6/6E (AU)

=== "WiFi 6 802.11ax Standards"
    
    **Key Features:**
    ```text
    OFDMA: Orthogonal Frequency Division Multiple Access
    MU-MIMO: Multi-User Multiple Input Multiple Output
    BSS Coloring: Basic Service Set coloring
    Target Wake Time: Power saving for IoT devices
    ```

    **Performance Improvements:**
    ```text
    4x capacity increase over 802.11ac
    Better performance in dense environments
    Improved power efficiency
    Enhanced security with WPA3
    ```

    **Frequency Bands:**
    ```text
    2.4GHz: 1-13 (Australia)
    5GHz: 36-165 (Australia)
    Channel Width: 20/40/80/160 MHz
    ```

=== "WiFi 6E 802.11ax Extended (6GHz Band)"
    
    **6GHz Band (Australia):**
    ```text
    Frequency: 5.925-7.125 GHz
    Channels: 1-233 (6.0-7.1 GHz)
    Channel Width: 20/40/80/160/320 MHz
    Indoor/Outdoor: Both allowed
    ```

    **Key Benefits:**
    ```text
    Massive bandwidth: 1.2 GHz available
    No DFS requirements
    Clean spectrum (no legacy devices)
    Ultra-wide channels (320 MHz)
    ```

    **Channel Allocation:**
    ```text
    Low Band: 5.925-6.425 GHz (Channels 1-93)
    Mid Band: 6.425-6.875 GHz (Channels 97-149)
    High Band: 6.875-7.125 GHz (Channels 153-233)
    ```

=== "Performance & Applications"
    
    **Theoretical Speeds:**
    ```text
    160 MHz: 2.4 Gbps (1x1)
    320 MHz: 4.8 Gbps (1x1)
    8x8 MIMO: 9.6 Gbps
    ```

    **Real-world Applications:**
    ```text
    VR/AR: Ultra-low latency
    8K Video: High bandwidth streaming
    Gaming: Sub-millisecond latency
    IoT: Massive device connectivity
    ```

    **Range Characteristics:**
    ```text
    Indoor: 30-80m (shorter range)
    Outdoor: 100-200m (line of sight)
    Penetration: Very poor through walls
    Interference: Minimal (new band)
    ```

---

## 📚 WiFi Standards Evolution

=== "Standards Timeline"
  
    | Year | Standard | Frequency | Max Speed | Key Features |
    |------|----------|-----------|-----------|--------------|
    | **1997** | 802.11 | 2.4GHz | 2 Mbps | First WiFi standard |
    | **1999** | 802.11a | 5GHz | 54 Mbps | 5GHz introduction |
    | **1999** | 802.11b | 2.4GHz | 11 Mbps | WEP security |
    | **2003** | 802.11g | 2.4GHz | 54 Mbps | WPA security |
    | **2009** | 802.11n | 2.4GHz + 5GHz | 600 Mbps | MIMO technology |
    | **2013** | 802.11ac | 5GHz | 6.77 Gbps | MU-MIMO, 160 MHz |
    | **2019** | 802.11ax (WiFi 6) | 2.4GHz + 5GHz | 9.6 Gbps | OFDMA, WPA3 |
    | **2021** | WiFi 6E | 6GHz | 9.6 Gbps | 6GHz band, 320 MHz |

=== "Security Evolution"
    
    | Security | Year | Encryption | Status |
    |----------|------|------------|--------|
    | **WEP** | 1997 | RC4 | ❌ Deprecated |
    | **WPA** | 2003 | TKIP | ⚠️ Vulnerable |
    | **WPA2** | 2004 | AES-CCMP | ✅ Current |
    | **WPA3** | 2018 | SAE | 🚀 Future |

---

## 📊 Channel Allocation Overview

### **📶 2.4GHz Channels (AU)**

| Channel | Frequency (GHz) | Non-Overlapping | Notes |
|---------|----------------|-----------------|-------|
| 1 | 2.412 | ✅ | Primary |
| 2 | 2.417 | ❌ | Overlaps with 1,3 |
| 3 | 2.422 | ❌ | Overlaps with 2,4 |
| 4 | 2.427 | ❌ | Overlaps with 3,5 |
| 5 | 2.432 | ❌ | Overlaps with 4,6 |
| 6 | 2.437 | ✅ | Primary |
| 7 | 2.442 | ❌ | Overlaps with 6,8 |
| 8 | 2.447 | ❌ | Overlaps with 7,9 |
| 9 | 2.452 | ❌ | Overlaps with 8,10 |
| 10 | 2.457 | ❌ | Overlaps with 9,11 |
| 11 | 2.462 | ✅ | Primary |
| 12 | 2.467 | ❌ | Overlaps with 11,13 |
| 13 | 2.472 | ❌ | Overlaps with 12 |

### **📡 5GHz Channels (AU)**

| UNII Band | Frequency Range | Channels | DFS Required | Indoor/Outdoor |
|----------|----------------|----------|--------------|----------------|
| **UNII-1** | 5.150-5.250 GHz | 36, 40, 44, 48 | ❌ No | Indoor only |
| **UNII-2A** | 5.250-5.350 GHz | 52, 56, 60, 64 | ✅ Yes | Both |
| **UNII-2C** | 5.470-5.725 GHz | 100, 104, 108, 112, 116, 120, 124, 128, 132, 136, 140 | ✅ Yes | Both |
| **UNII-3** | 5.725-5.850 GHz | 149, 153, 157, 161, 165 | ❌ No | Outdoor only |

### **🚀 6GHz Channels (AU)**

| Sub-Band | Frequency Range | Channel Range | Max Width | Indoor/Outdoor |
|----------|----------------|---------------|-----------|----------------|
| **Low Band** | 5.925-6.425 GHz | 1-93 | 160 MHz | Both |
| **Mid Band** | 6.425-6.875 GHz | 97-149 | 160 MHz | Both |
| **High Band** | 6.875-7.125 GHz | 153-233 | 160 MHz | Both |

---

## ⚠️ Legal Notice

**Important:** This information is for educational purposes only. Always comply with local regulations and obtain proper authorization before conducting wireless security testing.

---

## 🔗 Additional Resources

- [ACMA Radiofrequency Spectrum Plan](https://www.acma.gov.au/radiofrequency-spectrum-plan)
- [WiFi Alliance Certification](https://www.wi-fi.org/)
- [IEEE 802.11 Standards](https://www.ieee802.org/11/)
- [Australian Communications and Media Authority](https://www.acma.gov.au/)
