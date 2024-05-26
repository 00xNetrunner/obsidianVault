Certainly! Below is a cheat sheet for switching your wireless adapter between managed and monitor modes and changing the frequency band from 2.4GHz to 5GHz. I've included GitHub shields for visual appeal and emojis for a friendly touch.

---

## 📶 Wireless Adapter Mode Switching Cheat Sheet

### Switching to Monitor Mode
1. **Identify Your Wireless Adapter**
   ```bash
   sudo iw dev
   ```
   🛈 Your interface name should be something like `wlan0`.

2. **Check if Adapter Supports Monitor Mode**
   ```bash
   iw list
   ```
   🛈 Look for "monitor" under "Supported interface modes".

3. **Disable the Network Interface**
   ```bash
   sudo ip link set wlan0 down
   ```
   ⚠️ Replace `wlan0` with your actual interface name.

4. **Set the Wireless Adapter to Monitor Mode**
   ```bash
   sudo iw wlan0 set monitor none
   ```
   🔧 Again, replace `wlan0` with your actual interface name.

5. **Enable the Network Interface**
   ```bash
   sudo ip link set wlan0 up
   ```
   ✅ The adapter is now in monitor mode.

### Switching to Managed Mode
1. **Disable the Network Interface**
   ```bash
   sudo ip link set wlan0 down
   ```
   ⚠️ Replace `wlan0` with your actual interface name.

2. **Set the Wireless Adapter to Managed Mode**
   ```bash
   sudo iw wlan0 set type managed
   ```
   🔧 Again, replace `wlan0` with your actual interface name.

3. **Enable the Network Interface**
   ```bash
   sudo ip link set wlan0 up
   ```
   ✅ The adapter is now in managed mode.

### Changing Frequency Bands
#### To Switch to 2.4GHz
1. **Set the Wireless Adapter to a 2.4GHz Channel**
   ```bash
   sudo iw wlan0 set channel <channel_number>
   ```
   📡 Common 2.4GHz channels are 1-11 (e.g., `sudo iw wlan0 set channel 6`).

#### To Switch to 5GHz
1. **Set the Wireless Adapter to a 5GHz Channel**
   ```bash
   sudo iw wlan0 set channel <channel_number>
   ```
   📡 Common 5GHz channels are 36, 40, 44, 48, etc. (e.g., `sudo iw wlan0 set channel 36`).

### 🛡️ GitHub Shields
- ![GitHub stars](https://img.shields.io/github/stars/username/repo.svg)
- ![GitHub forks](https://img.shields.io/github/forks/username/repo.svg)
- ![GitHub issues](https://img.shields.io/github/issues/username/repo.svg)
- ![GitHub pull requests](https://img.shields.io/github/issues-pr/username/repo.svg)

### Notes
- 🚀 Use these commands responsibly and only on networks you own or have explicit permission to test.
- 🧰 Tools and commands may vary based on your wireless adapter and Linux distribution.

---

Feel free to save this cheat sheet for your reference! Remember, wireless analysis and monitoring should always be performed ethically and legally.