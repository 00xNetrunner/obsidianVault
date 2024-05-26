## Network Evidence - Findings.md

> [!INFO]
> `fas:InfoCircle` This document focuses on analyzing network evidence from the `johnDoe.dd` image.

First, check the MD5 hash to ensure the file is in its original state:

![[Pasted image 20240423221521.png]]

> [!SUCCESS]
> `fas:CheckCircle` The file remains unchanged, so the analysis can proceed.

Mount the `johnDoe.dd` image using the loopback device:

```bash
sudo losetup -o 32256 /dev/loop30 johnDoe.dd
sudo mount -o -ro -t ntfs /dev/loop30 ~/suspectDrive
```

Navigate to the `suspectDrive` directory and locate the following path:

```
Documents and Settings/<username>/Local Settings/Temporary Internet Files/Content.IE5
```

Copy the `index.dat` file from each user's directory to a folder named `browser` in the `jd` directory. Additionally, create a Pasco file for each `index.dat` file, which can be viewed in a spreadsheet. Save the Pasco files to the `jd` directory.

After processing the Temporary Internet Files, move to the History folder located at:

```
Documents and Settings/<username>/Local Settings/History/History.IE5
```

The following script can be used to copy all `index.dat` files to the case folder:

```bash
#!/bin/bash

# Array of source file paths
source_files=(
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/bob/Application Data/Microsoft/Office/Recent/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/bob/Cookies/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/bob/Local Settings/History/History.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/bob/Local Settings/History/History.IE5/MSHist012005020320050204/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/bob/Local Settings/Temporary Internet Files/Content.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/Default User/Cookies/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/Default User/Local Settings/History/History.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/Default User/Local Settings/Temporary Internet Files/Content.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/jane/Cookies/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/jane/Local Settings/History/History.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/jane/Local Settings/Temporary Internet Files/Content.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/johndoe/Application Data/Microsoft/Office/Recent/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/johndoe/Cookies/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/johndoe/Local Settings/History/History.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/johndoe/Local Settings/History/History.IE5/MSHist012005012420050131/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/johndoe/Local Settings/History/History.IE5/MSHist012005013120050207/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/johndoe/Local Settings/History/History.IE5/MSHist012005020920050210/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/johndoe/Local Settings/Temporary Internet Files/Content.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/johndoe/UserData/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/LocalService/Cookies/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/LocalService/Local Settings/History/History.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/Documents and Settings/LocalService/Local Settings/Temporary Internet Files/Content.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/WINDOWS/pchealth/helpctr/OfflineCache/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/WINDOWS/system32/config/systemprofile/Cookies/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/WINDOWS/system32/config/systemprofile/Local Settings/History/History.IE5/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/WINDOWS/system32/config/systemprofile/Local Settings/History/History.IE5/MSHist012005012420050125/index.dat"
    "/home/sansforensics/Desktop/mount_points/suspectDrive/WINDOWS/system32/config/systemprofile/Local Settings/Temporary Internet Files/Content.IE5/index.dat"
)

# Destination directory
dest_dir="/home/sansforensics/Desktop/cases/CMP209/jd/index_files"

# Create the destination directory if it doesn't exist
sudo mkdir -p "$dest_dir"

# Initialize a counter for copied files
copied_files=0

# Iterate over the source files and copy them to the destination directory
for file in "${source_files[@]}"; do
    if [ -f "$file" ]; then
        # Extract the directory path from the source file
        dir_path=$(dirname "$file")
        
        # Create the corresponding subdirectory in the destination
        sudo mkdir -p "$dest_dir/$dir_path"
        
        # Copy the file to the destination subdirectory
        sudo cp "$file" "$dest_dir/$dir_path"
        
        ((copied_files++))
    else
        echo "File not found: $file"
    fi
done

echo "Copied $copied_files index.dat files to $dest_dir"
```

After all the files have been copied, unmount the suspect drive and remove the loopback device using the following commands:

```bash
sudo umount ~/suspectDrive
sudo losetup -d /dev/loop30
```

Check the MD5 hash once again to ensure the image file remains in its original state:

![[Pasted image 20240424203748.png]]