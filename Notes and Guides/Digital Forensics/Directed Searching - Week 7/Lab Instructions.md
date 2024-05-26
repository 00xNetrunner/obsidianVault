---
sticker: lucide//alert-triangle
banner: https://images.foxtv.com/static.fox4news.com/www.fox4news.com/content/uploads/2021/05/1280/720/6P-190-TZ_FBI-NEW-COMPUTER-FORENSICS-LAB_00.00.00.08.png?ve=1&tl=1
---
# Expanded Lab Instructions for CMP209 - Digital Forensics

## Aim of this Lab
The aim of this lab is to:
- Examine the registry data on John Doe's disk image.

## Core Learning Outcomes
- Increased understanding and competence using the Linux operating system to perform digital forensics.
- Familiarization with the processes required to conduct a digital forensic investigation (read the supplementary docs for this).
- How to forensically examine the registry of a Windows computer.

## Directed Searching - Part 1 - Examining the Registry

1. Check the MD5 hash of your `johnDoe.dd` file. It should be unchanged from when you first created it.

2. Use the loopback mounting technique to mount your `johnDoe.dd` image file. Note that if you receive an error that the device or resource is busy, try incrementing your loopback device number. For example, instead of using `/dev/loop30`, use `/dev/loop60`, etc.

3. Create a `registry` directory in your main `jd` working folder. You can create this folder in a different directory (i.e., not your home directory), but then permissions can become problematic.

4. Once created, copy the registry files from the mounted `johnDoe.dd` image to the `registry` directory you created in step 3. Note that the following commands assume that you mounted the John Doe image under `~/suspectDrive`. It also assumes that you created your `registry` directory in `/home/cmp209/jd/`. Execute the following to copy the required files:

   ```bash
   sudo cp ~/suspectDrive/WINDOWS/system32/config/SAM /home/cmp209/jd/registry
   sudo cp ~/suspectDrive/WINDOWS/system32/config/SECURITY /home/cmp209/jd/registry
   sudo cp ~/suspectDrive/WINDOWS/system32/config/software /home/cmp209/jd/registry
   sudo cp ~/suspectDrive/WINDOWS/system32/config/system /home/cmp209/jd/registry
   sudo cp ~/suspectDrive/Documents\ and\ Settings/johndoe/NTUSER.DAT /home/cmp209/jd/registry
   ```

5. Unmount the John Doe image and remove the loopback device.

6. Ensure that you are in the `registry` directory you created in step 3 before continuing. If not, change to this directory by issuing the following command

   ```bash
   cd /home/cmp209/jd/registry
   ```

7. Change permissions to give the `cmp209` user (your current logged-in user account) the required access to the files:

   ```bash
   sudo chown cmp209 SAM SECURITY software system NTUSER.DAT
   chmod u+w SAM SECURITY software system NTUSER.DAT
   ```

8. Use the tool `chntpw` to find a list of user accounts on John Doe's computer. Execute:

   ```bash
   chntpw -l SAM
   ```

   Ensure that you are in the `/home/cmp209/jd/registry` directory when you execute the above command.

9. Run `fred` and load the registry files you were able to retrieve. You can run `fred` using the desktop shortcut or from the command line. Open these files one at a time by navigating to `File → Open hive`. Once completed, you should see a folder in the tree (on the left) for each registry file. You may also be able to get some use out of `regripper`.

10. Before embarking on any further analyses, read the article by Carvey (2005) titled "The Windows Registry as a forensic resource". This article covers techniques that match the version of Windows on John Doe's computer. For those interested in reading more widely, consider reading about newer techniques within the articles by Park (2018) and Singh et al. (2020).

11. Take note that information about USB devices is typically stored in the following registry location:

    ```
    HKEY_LOCAL_MACHINES\System\CurrentControlSet\Enum\USBSTOR
    ```

    If you are using `fred`, this information will appear under:

    ```
    ControlSet002\Enum\USBSTOR
    ```

    The word `ControlSet002` means that you are working with the current control set. More importantly, this location contains a unique entry for each device that was ever connected to John Doe's computer.

    Even more information can be obtained from:

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Device-Classes\{53f56307-b6bf-11d0-94f2-00a0c91efb8b}
    ```

12. Find out which USB storage devices have been connected to John Doe's computer.

13. To find out where these devices were mounted (together with their unique IDs), investigate:

    ```
    HKEY_LOCAL_MACHINE\System\MountedDevices
    ```

    As well as the following location from the `NTUSER.DAT` file:

    ```
    HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2
    ```

    List the discovered devices.

14. Determine which software packages were installed and available to users.

15. You can also use Autopsy to perform registry investigations. Open Autopsy and perform a similar registry investigation.

16. Think broadly about what else could be gained by investigating the registry of John Doe's computer. Do not rely solely on the lab exercises.

17. Before completing the lab exercises, check the MD5 hash of the `johnDoe.dd` image to ensure that you have not changed it while performing the above steps.

## How to Install fred

18. `fred` is already installed on the Ubuntu analysis VM, so only follow these steps if you are installing it on your own computer or VM:

    ```bash
    sudo wget -nH -P /etc/apt/sources.list.d/ https://deb.pinguin.lu/deb.pinguin.lu.list
    sudo wget -nH -P /usr/share/keyrings/ https://deb.pinguin.lu/deb-pinguin-lu.gpg
    sudo apt update
    sudo apt install fred
    sudo apt install fred-reports
    ```

![[Lab_7_Instructions.pdf]]