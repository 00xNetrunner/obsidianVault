## Physical Searching

> [!INFO]
> `fas:InfoCircle` This document focuses on file-carving and metadata analysis in Linux.

### Part 1: File-Carving and Metadata in Linux

To begin the analysis, navigate to the directory containing the `johnDoe.dd` file using the `cd` command:

```bash
cd ~/jd
```

Change the file permissions of `johnDoe.dd` to read-only using the `chmod` command:

```bash
chmod 400 johnDoe.dd
```

![[Pasted image 20240316004145.png]]

> [!WARNING]
> `fas:ExclamationTriangle` Setting the file to read-only ensures that no changes can be made to the original evidence.

Use **Foremost** to carve out files from the `johnDoe.dd` image:

```bash
foremost -T -t all -i johnDoe.dd
```

> [!TIP]
> `fas:Clock` This process may take a considerable amount of time to complete.

Once Foremost finishes processing, a folder is created containing subfolders categorized by file types (e.g., gif, jpg, png).

![[Pasted image 20240316033344.png]]

If necessary, change the folder permissions to access the subfolders:

```bash
sudo chmod +x folder
```

Use **Metacam** to search for images taken with a Canon PowerShot camera within the jpg folder:

```bash
metacam *.png | grep "Canon PowerShot"
```

> [!SUCCESS]
> `fas:CheckCircle` This command displays all images found with Canon PowerShot in the EXIF data.

A script has been created to streamline this process and sort the matching images into a dedicated folder:

```bash
#!/bin/bash

# Check if a keyword was provided
if [ $# -eq 0 ]; then
  echo "Usage: $0 'keyword'"
  exit 1
fi

KEYWORD="$1"

# Define the target directory for images matching the keyword
TARGET_DIR="./${KEYWORD// /_}_Images"

# Create the target directory if it doesn't already exist
mkdir -p "$TARGET_DIR"

# Search and move images containing the keyword in their metadata within the current directory
find . -maxdepth 1 -iname "*.jpg" -exec sh -c '
  keyword="$1"; shift
  target_dir="$1"; shift
  for file; do
    if metacam -a "$file" | grep -q "$keyword"; then
      echo "Moving: $file to '"$target_dir"'"
      mv "$file" "$target_dir"
    fi
  done
' sh "$KEYWORD" "$TARGET_DIR" {} +

echo "Operation completed."
```

To use the script:

1. Grant execute permissions to the script:
   ```bash
   sudo chmod +x metaScript.sh
   ```

2. Run the script with the desired keyword:
   ```bash
   sudo ./metaScript.sh keyword
   ```
   Replace `keyword` with the search term (e.g., "Canon PowerShot").

![[Pasted image 20240323195320.png | Script running]]

![[Pasted image 20240323195421.png|Images found with EXIF data placed in their own folder]]

Files found with "Canon PowerShot" in their EXIF data:
![[png.tar.gz]]

### Part 2: Physical Searching Using Autopsy

![[Pasted image 20240324034018.png|Setting up case (1/2)]]

![[Pasted image 20240324034054.png|Setting up case (2/2)]]

![[Pasted image 20240324034227.png|Selecting Data Source Type]]

![[Pasted image 20240324034341.png|Selecting Data]]

> [!TIP]
> `fas:Lightbulb` While the file is being analyzed/loaded, use the keyword search feature in Autopsy to find relevant documents on the target's USB drive.

Searching for "Birdwatching" reveals a document discussing birdwatching in Thailand (Figure 1). Additionally, searching for "Canon PowerShot" yields multiple images previously found using Foremost and Metacam (Figure 2).

![[Pasted image 20240324042905.png|Figure 1]]

![[Pasted image 20240324044622.png|Figure 2]]