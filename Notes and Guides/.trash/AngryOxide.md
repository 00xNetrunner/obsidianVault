---
banner: https://repository-images.githubusercontent.com/708570580/c06b2db3-0060-4297-8468-50559f44fbe4
sticker: emoji//1f92c
---
### Updated User Guide for AngryOxide (AO) Tool  `fas:Wrench`

This guide will walk you through installing the drivers for the ALFA AWUS1900 (which uses the RTL8814AU chipset) and using AngryOxide on Kali Linux, especially within a virtual machine (VM) environment.

#### Current Version: v0.8.3 `fas:CodeBranch`

### Part 1: Installing Drivers for ALFA AWUS1900 `fas:NetworkWired`

> [!NOTE] `fas:InfoCircle`
> Ensure you are connected to the internet before proceeding with the installation steps.

#### Step 1: Install Required Packages `fas:BoxOpen`

First, ensure you have all the necessary packages installed:

```bash
sudo apt-get update
sudo apt-get install dkms build-essential libelf-dev linux-headers-$(uname -r) git
```

#### Step 2: Download and Install the Driver `fas:Download`

Clone the Aircrack-ng repository and install the driver:

```bash
git clone -b v5.6.4.2 https://github.com/aircrack-ng/rtl8814au.git
cd rtl8814au
sudo make dkms_install
```

#### Step 3: Verify the Driver Installation `fas:Check`

Load the module and check if the driver is working:

```bash
sudo modprobe 8814au
```

### Part 2: Setting Up the Wireless Adapter for Monitor Mode `fas:Wifi`

> [!important] `fas:ExclamationTriangle`
> Using the monitor mode may interfere with your normal Wi-Fi connectivity. Proceed with caution.

#### Step 1: Bring the Interface Down `fas:ArrowDown`

Identify your wireless interface (e.g., `wlan0`, `wlx00c0cab0cafe`) and bring it down:

```bash
sudo ip link set wlx00c0cab0cafe down
```

#### Step 2: Set the Interface to Monitor Mode `fas:Eye`

Set the interface to monitor mode:

```bash
sudo iw dev wlx00c0cab0cafe set type monitor
```

#### Step 3: Bring the Interface Up `fas:ArrowUp`

Bring the interface back up:

```bash
sudo ip link set wlx00c0cab0cafe up
```

#### Step 4: Verify Monitor Mode `fas:Search`

Verify that the interface is now in monitor mode:

```bash
sudo iw dev wlx00c0cab0cafe info
```

### Part 3: Using AngryOxide `fas:Tools`

#### Basic Usage `fas:Play`

To run AngryOxide, specify the network interface:

```bash
sudo angryoxide -i wlx00c0cab0cafe
```

This command runs AngryOxide with default settings, targeting all APs on channels 1, 6, and 11.

> [!Tip] `fas:Lightbulb`
> For better performance, run AngryOxide with elevated privileges.

#### Getting Help `fas:QuestionCircle`

For a list of all available options, use:

```bash
angryoxide --help
```

#### Example Commands `fas:Terminal`

1. **Capture Handshakes from All APs:**

   ```bash
   sudo angryoxide -i wlx00c0cab0cafe
   ```

2. **Target Specific APs:**

   ```bash
   sudo angryoxide -i wlx00c0cab0cafe -t aabbccddeeff
   ```

3. **Capture Handshakes from 5GHz Networks:**

   ```bash
   sudo angryoxide -i wlx00c0cab0cafe -b 5
   ```

4. **Run with Debugging Information:**

   ```bash
   RUST_BACKTRACE=1 sudo angryoxide -i wlx00c0cab0cafe
   ```

### Part 4: Advanced Options and Features `fas:Cogs`

#### Setting Specific Channels `fas:SlidersH`

Specify channels explicitly to avoid auto-detection issues:

```bash
sudo angryoxide -i wlx00c0cab0cafe -c 1,6,11
```

#### Auto Hunt Mode `fas:Binoculars`

Enable auto-hunting to automatically lock onto target channels:

```bash
sudo angryoxide -i wlx00c0cab0cafe --autohunt
```

#### Combining Hashline Files `fas:File`

Combine all `.hc22000` files into one for easier processing:

```bash
sudo angryoxide -i wlx00c0cab0cafe --combine
```

#### Running in Headless Mode `fas:Server`

Run AO in headless mode for scripts or remote execution:

```bash
sudo angryoxide -i wlx00c0cab0cafe --headless --autoexit
```

### Troubleshooting `fas:Bug`

> [!warning] `fas:ExclamationTriangle`
> If AngryOxide exits unexpectedly, running it with `RUST_BACKTRACE=1` can help diagnose the issue by providing more detailed error output.

```bash
RUST_BACKTRACE=1 sudo angryoxide -i wlx00c0cab0cafe
```

### Summary `fas:Clipboard`

Ensure your wireless adapter is in monitor mode using the `ip` and `iw` commands. Install the latest drivers for your RTL8814AU chipset to ensure compatibility and functionality with tools like AngryOxide. Use the provided commands to capture handshakes and manage the operation of AngryOxide effectively.

For more detailed instructions and troubleshooting, refer to the [Aircrack-ng GitHub repository](https://github.com/aircrack-ng/rtl8814au) and the [astsam GitHub repository](https://github.com/astsam/rtl8812au).