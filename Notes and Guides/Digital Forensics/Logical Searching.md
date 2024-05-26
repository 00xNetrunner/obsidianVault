## Logical Searching.md

> [!INFO]
> `fas:InfoCircle` This document focuses on hashing and mounting the `johnDoe.dd` image for logical searching.

### Part 1: Hashing (and Mounting) the `johnDoe.dd` Image

First, ensure the integrity of the disk image by verifying the MD5 hash, which should be `d63dd1b8917ca28bac7c955fc3b6cd25`.

![[Pasted image 20240324072448.png]]

Create a directory named `suspectDrive` in the home directory:

```bash
mkdir suspectDrive
```

Before mounting the file image, use the `mmls` command to identify the partition table:

```bash
mmls -t dos johnDoe.dd
```

###### Command Breakdown:
- `mmls`: A command used in digital forensics to list partitions in a disk image.
- `-t dos`: Specifies the type of partition table to interpret (DOS/MBR).
- `johnDoe.dd`: The disk image file to be analyzed.

![[Pasted image 20240324074136.png]]

> [!INFO]
> `fas:InfoCircle` The output shows an NTFS file system within the image file, along with the start, end, and length of the partition.

Calculate the offset by multiplying the start sector (`63`) by `512`:

```
63 * 512 = 32256
```

![[Pasted image 20240324081659.png]]

> [!TIP]
> `fas:Lightbulb` Calculating the offset is crucial for the next step: mounting the partition.

Create a loopback device and mount the drive:

```bash
sudo losetup -o 32256 /dev/loop30 johnDoe.dd
sudo mount -o -ro -t ntfs /dev/loop30 ~/suspectDrive
```

![[Pasted image 20240324083016.png]]

> [!WARNING]
> `fas:ExclamationTriangle` If no error message appears, the file system should be mounted successfully.

> [!TIP]
> `rir:StickyNote` When mounting multiple file systems, use different loop devices to avoid conflicts. Use the `losetup -l` command to see which loop devices are in use.

![[Pasted image 20240324083428.png]]

Navigate to the `~/suspectDrive` directory, where a Windows file system structure is visible. A file named `birdwatching.doc` is immediately noticeable.

![[Pasted image 20240324085442.png]]

From the mounted directory, use `md5deep` to create a list of all files along with their MD5 hashes and locations:

```bash
md5deep -r * > /home/cmp209/jd/johnDoe.nameAndmd5list.txt
```

> [!TIP]
> `fas:Clock` This process may take some time to complete.

Filter the list to include only JPG and BMP files:

```bash
grep -E "\.jpg$|\.bmp$" johnDoe.nameAndmd5List.txt > filteredJohnDoeList.txt
```

To create a whitelist, repeat the process with the `winXPPostIntall.dd` file. First, verify the MD5 hash of the file.

![[Pasted image 20240409184333.png]]

Use `mmls` to identify the partition table and calculate the offset:

```bash
mmls -t dos winXPPostInstall.dd
```

![[Pasted image 20240409185310.png]]

> [!INFO]
> `fas:InfoCircle` The start sector is `63`, resulting in an offset of `32256`.

Mount the image file using the calculated offset:

```bash
sudo losetup -o 32256 /dev/loop31 winXPPostIntall.dd
sudo mount -o -ro -t ntfs /dev/loop31 /home/sansforensics/Desktop/mount_points/windows_mount/
```

![[Pasted image 20240409190745.png]]

Run `md5deep` on the mounted drive and save the output to the appropriate directory:

![[Pasted image 20240409191052.png]]

Use the `cut` command to remove the file path from the text file, leaving only the MD5 hashes:

```bash
cut -f 1 -d ' ' winXP.nameAndmd5List.txt > winXP.md5List.txt
```

![[Pasted image 20240409212610.png]]

Move the `johnDoe.txt` and `winXP.txt` files to the same folder and use `grep` with the whitelist to identify the locations of the image files:

```bash
grep -v -f winXP.md5List.txt filteredJohnDoe.txt > filteredImages.txt
```

![[filterdImages.txt]]


**How to remove loopback and unmount**
```bash
sudo umount ~/suspectDrive
sudo losetup -d /dev/loop30
```

