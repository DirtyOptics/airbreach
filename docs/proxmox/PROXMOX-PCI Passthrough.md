# 🛠️ How To: Proxmox PCI/GPU Passthrough

👤 <strong>Author:</strong> Dwayne Black  
📅 <strong>Date:</strong> 16 Oct 25  
📌 <strong>Version:</strong> 01.2025

---

# 📖 Overview

PCI passthrough allows you to use a physical PCI device (graphics card, network card) inside a VM (KVM virtualization only).

If you "PCI passthrough" a device, the device is not available to the host anymore. Note that VMs with passed-through devices cannot be migrated.

Good for Gfx cards passthrough direct to Plex/Jellyfin VM for transcoding etc.

---

# 📑 Table of Contents

- [📖 Overview](#-overview)
- [📋 Prerequisites](#-prerequisites)
- [🧩 Step-by-Step Guide](#-step-by-step-guide)
  - [🔧 Step 1: {{Prepare BIOS}}](#-step-1-prepare-bios)
  - [🔧 Step 2: {{Step Title}}](#-step-2-step-title)
  - [🔧 Step 3: {{Step Title}}](#-step-3-step-title)
  -  [🔧 Step 4: {{Step Title}}](#-step-3-step-title)
- [❗ Troubleshooting](#-troubleshooting)
- [📚 Resources](#-resources)
- [🔄 Revision History](#-revision-history)
- [📝 Additional Notes](#-additional-notes)

---

# 📋 Prerequisites

| 📦 Item         | 📖 Description                                                                                                              | 🔢 Version |
| --------------- | --------------------------------------------------------------------------------------------------------------------------- | ---------- |
| Proxmox VE      | proxmox virtual environment                                                                                                 | 9.0        |
| Motherboard     | VT-d/IOMMU capable Motherboard                                                                                              |            |
| PCI device      | SAS HBA, Gfx Card                                                                                                           |            |
| Quick Reference | https://pve.proxmox.com/wiki/PCI_Passthrough                                                                                |            |
| Original Guide  | [HSVE Guide](https://github.com/HSVEOCT/docs/tree/main/guides/GPU%20Passthrough%20Guide/Passthrough%20Commands) Thank you!! |            |

---

# 🧩 Step-by-Step Guide

## Step 1: Prepare BIOS
This prep is required to ensure that your system is compatible with 'passthrough' and you have the best chance of success. if possible, **start by ensuring your BIOS firmware is up to date!**

#### ▶️ BIOS Enable/Disable:

✅ **Enable SVM/Virtualization Mode**

✅ **Enable VT-d**  
_Required for passthrough to work at all

✅ **Enable SR-IOV**  
_If present._

🛑  **Disable CSM/Legacy Boot**

✅ **Enable ACS**  
_If present, set to Enabled (Auto doesn’t work)._

✅ **Set PEG {NUMBER} ASPM to L0 or L0sL1**  
_Set whichever amount you've got to mentioned, if L0 is present, pick that._

✅ **Enable 4G Decoding**

🛑  **Disable Resizable BAR/Smart Access Memory**  
_You might experience ‘Code 43 error’ if enabled._

✅ **Enable IOMMU**  
_If present, mostly for AMD boards;

✅ **Set Primary Display to CPU/iGPU**  
_If your CPU has an iGPU, if not skip this._

✅ **Set Pre-Allocated Memory to 64M**  
_If your CPU has an iGPU, if not skip this too._

> [!ATTENTION]
This step is not necessary for the passthrough, but helps keeping things clean.
For ignoring some annoying “warnings” in your `dmesg` output, do the following:

**⚠️ Attention:** This step is not necessary for the passthrough, but helps keeping things clean.  
For ignoring some annoying “warnings” in your `dmesg` output, do the following:

<div class="admonition attention">
<strong>⚠️ Attention:</strong> This step is not necessary for the passthrough, but helps keeping things clean. For ignoring some annoying "warnings" in your dmesg output, do the following:
</div>

!!! attention "Attention" This step is not necessary!


```ad-tip
title: This is a tip

This is the content of the admonition tip.
```



Open:
```
nano /etc/modprobe.d/kvm.conf
```

Add the following line:
```
options kvm ignore_msrs=Y report_ignored_msrs=0
```

---

## 🔧 Step 2: Setting the Boot Arguments

#### For **Intel** CPUs

For **GRUB**;  
Replace the current similar line with this one in your grub file:
```
nano /etc/default/grub
```

Make these changes to that file:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt initcall_blacklist=sysfb_init"
```

> [!ATTENTION]
This config is from a youtube video which seemed to work and display "IOMMU Enabled" when checking as well.
```GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt intel_pstate=disable```

Hit **Ctrl + X → Y → Enter** to save changes.

---
#### For **AMD** CPUs

For **GRUB**;  
Replace the current similar line with this one in your grub file:
```
nano /etc/default/grub
```

Make these changes to that file:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet iommu=pt initcall_blacklist=sysfb_init"
```

Hit **Ctrl + X → Y → Enter** to save changes.

---

> [!ATTENTION]
Your system might not be relying on Grub, but **systemd** instead. This is often the case when you’re using **ZFS**.  So for **systemd**:

#### Intel CPUs (Systemd/ZFS)

```
nano /etc/kernel/cmdline
```

```
root=ZFS=rpool/ROOT/pve-1 boot=zfs intel_iommu=on iommu=pt initcall_blacklist
=sysfb_init
```

#### AMD CPUs (Systemd/ZFS)

```
nano /etc/kernel/cmdline
```

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet iommu=pt initcall_blacklist=sysfb_init"
```

---

For **Grub** only:
```
update-grub
```

For **Systemd** only:
```
pve-efiboot-tool refresh
```

Reboot to commit changes with:
```
reboot
```

---

## ✍️ Step 3: Configuring IOMMU

Once the host is up and running again we need to check if IOMMU is enabled and working. We can do this by running the following commands.

For **Intel**:
```
dmesg | grep -e DMAR -e IOMMU
```

For **AMD**:
```
dmesg | grep -e DMAR -e IOMMU -e AMD-Vi
```

If there's no output you've done something wrong, please retrace your steps:
You should be seeing something like this: "DMAR: IOMMU Enabled"

---

The next step is enabling the modules required for GPU passthrough:
Enable the necessary modules by running the following command:

```
nano /etc/modules
```

Add the following lines to this file:
```
vfio
vfio_iommu_type1
vfio_pci
```

Hit **Ctrl** + **X** > **Y** **Enter** to Save changes.

---

After changing the following modules you need to refresh your initramfs with the following command:
```
update-initramfs -u -k all
```

Lets check if remapping has been enabled with the following command:
```
dmesg | grep remapping
```

You should see something like "Interrupt remapping enabled / IRQ Remapping Enabled"

If your system doesn't support Remapping you can run this command to enable remapping. **However please be aware that this option could make your system unstable.**
```
nano /etc/modprobe.d/iommu_unsafe_interrupts.conf
```

Add the following line:
```
options vfio_iommu_type1 allow_unsafe_interrupts=1
```

Hit **Ctrl** + **X** > **Y** > **Enter** to save your changes.

---

## 🚀 Step 4: Blacklisting Driver modules

Blacklisting driver modules to give VM full access to Hardware.
```
nano /etc/modprobe.d/pve-blacklist.conf 
```

Add:
```
blacklist nouveau
blacklist nvidia
blacklist nvidiafb
blacklist snd_hda_codec_hdmi
blacklist snd_hda_intel
blacklist snd_hda_codec
blacklist snd_hda_core
blacklist radeon
blacklist amdgpu
```

Hit **Ctrl** + **X** > **Y** > **Enter**

---

#### Adding Device ID's to Blacklist

To find the corresponding IDs for your PCI devices run the following command:
**Replace {device} = vga,usb,audio,wireless,sas etc.**
```
lspci -nn | grep -i {device}
```

You will see an output similar to the below screenshot:

![[device id.png]]

As you can see the IDs of the GPU above are **10de:1287** and **1002:67d**.

You may decide you want to find the IDs of your USB controllers too just in case you need to whitelist these down the line due to issues.

To blacklist the devices from use by Proxmox and full access use to your VM run the following commands.

```
nano /etc/modprobe.d/vfio-pci.conf
```

Add the device IDs in the blank file like this:
```
options vfio-pci ids=1234:5678,1234:5678 disable_vga=1
```

Replacing the **1234:5678** with your actual IDs, if you only have 1 GPU remove the other IDs from the command.

_**Example:**_
```
options vfio-pci ids=10de:1287 disable_vga=1
```

You can also add **disable_idle_d3=1** to the end of the line to disable the D3 device power state which will prevent certain hardware from entering low power mode, which can cause issues when passed through to VMs.

Hit **Ctrl** + **X** > **Y** > **Enter** to save changes.

Once all of the above is sorted reboot your machine once again with the command below:
```
reboot
```

---

## ⏳ Step 4: Adding devices to you VM


(Coming Soon!)




---

## ❗ Troubleshooting

| 🚨 Issue | 📋 Description | ✅ Possible Solution |
| -------- | -------------- | ------------------- |
|          |                |                     |

---

## 📚 Resources

- [📖 Original Guide that worked!](https://github.com/HSVEOCT/docs/tree/main/guides/GPU%20Passthrough%20Guide/Passthrough%20Commands)
- [🔗 Proxmox Wiki on PCI passthrough](https://pve.proxmox.com/wiki/PCI_Passthrough)
- [🔖 Proxmox VW Administration Guide](https://pve.proxmox.com/pve-docs/pve-admin-guide.html)
- 🎥 [Tutorial Video - 10 Oct 2025](https://www.youtube.com/watch?v=Pjjyuk4YU78)

---

## 🔄 Revision History

| 📅 Date     | 🔖 Version | 👤 Author    | 📝 Changes / Notes |
| ----------- | ---------- | ------------ | ------------------ |
| 16 Oct 2025 | 2025.01    | Dwayne Black | Initial draft      |

---

## 📝 Additional Notes

Nothing of importance to note just yet.