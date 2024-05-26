---
banner: Linx Commands/images/USB_Flash.webp
---
![[DALL·E 2024-05-23 22.36.51 - A simple 2D pixel art illustration featuring a USB stick and an ISO file icon. The USB stick should be rectangular with a visible USB connector and si.webp]]

#### Formatting USB Drives

> [!info] **Step 1: Identify the USB Drive** &#xf2b2;
>
> First, identify the device name of your USB drive. Run:
> ```bash
> lsblk
> ```
> Look for your USB drive in the output, e.g., `/dev/sdX` (where `X` is the device letter).

> [!info] **Step 2: Unmount the USB Drive** &#xf256;
>
> Before formatting, unmount the USB drive:
> ```bash
> sudo umount /dev/sdX1
> ```

> [!info] **Step 3: Format the USB Drive** &#xf0a0;
>
> - **Format to FAT32:**
>   ```bash
>   sudo mkfs.vfat -F 32 /dev/sdX
>   ```
> - **Format to ext4:**
>   ```bash
>   sudo mkfs.ext4 /dev/sdX
>   ```
> - **Format to NTFS:**
>   ```bash
>   sudo mkfs.ntfs /dev/sdX
>   ```

> [!info] **Step 4: Create a Partition Table (Optional)** &#xf249;
>
> To create a new partition table:
> ```bash
> sudo parted /dev/sdX -- mklabel msdos
> ```

> [!info] **Step 5: Create a New Partition (Optional)** &#xf065;
>
> To create a new primary partition:
> ```bash
> sudo parted -a optimal /dev/sdX mkpart primary 0% 100%
> ```

#### Flashing ISO Images to USB Drives

> [!info] **Step 1: Identify the USB Drive** &#xf2b2;
>
> Use `lsblk` to identify your USB drive as shown above.

> [!info] **Step 2: Flash the ISO Image** &#xf0a0;
>
> Use the `dd` command to flash the ISO:
> ```bash
> sudo dd if=/path/to/your.iso of=/dev/sdX bs=4M status=progress
> ```
> - `if` is the input file (the ISO image).
> - `of` is the output file (the USB drive).
> - `bs=4M` sets the block size to 4 Megabytes.
> - `status=progress` shows the progress of the operation.

> [!info] **Step 3: Sync the Filesystem** &#xf017;
>
> Ensure all data is written to the USB drive:
> ```bash
> sudo sync
> ```

#### Examples

> [!example] **Format to FAT32 Example** &#xf2b2;
> ```bash
> sudo mkfs.vfat -F 32 /dev/sdb
> ```

> [!example] **Flash ISO to USB Example** &#xf0a0;
> ```bash
> sudo dd if=/path/to/ubuntu.iso of=/dev/sdb bs=4M status=progress
> sudo sync
> ```

### Notes

> [!warning] **Important Reminders** &#xf071;
>
> - **Double-check device names** to avoid data loss.
> - **Backup important data** before formatting or flashing.
> - Use `sudo` for commands that require administrative privileges.
