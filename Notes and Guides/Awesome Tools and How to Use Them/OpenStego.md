

# 📝 OpenStego CLI Cheat Sheet

## 🖼 Embedding a File
To embed a file into an image, follow the command format below. This example demonstrates how to compress and embed `secret.txt` into `wallpaper.png`, saving the output as `hidden.png` using the RandomLSB algorithm.

```plaintext
java -jar path/to/openstego.jar embed -a randomlsb -mf path/to/secret.txt -cf path/to/wallpaper.png -sf path/to/hidden.png -c 
```

### 🛠 Embedding Options
- `-a randomlsb` 🎲: Specifies the steganography algorithm (RandomLSB in this instance).
- `-mf path/to/secret.txt` 📄: Path to the source message/data file.
- `-cf path/to/wallpaper.png` 🖼: Path to the cover image file.
- `-sf path/to/hidden.png` 🗂: Path to the output stego file (where the data is hidden).
- `-c` 🗜: Compress the message file before embedding.

## 🔍 Extracting a File
To extract a file from an image, use the following command structure. This command extracts the message from `hidden.png` into `extracted_secret.txt` using the RandomLSB algorithm.


java -jar path/to/openstego.jar extract -a randomlsb -sf path/to/hidden.png -xf path/to/extracted_secret.txt 🔓


### 🛠 Extracting Options
- `-a randomlsb` 🎲: Specifies the steganography algorithm used during embedding.
- `-sf path/to/hidden.png` 🗂: Path to the stego file from which to extract the message.
- `-xf path/to/extracted_secret.txt` 📄: Path to save the extracted message. If not specified, OpenStego uses the filename embedded in the stego file.


Please ensure to replace `path/to/openstego.jar`, `path/to/secret.txt`, `path/to/wallpaper.png`, `path/to/hidden.png`, and `path/to/extracted_secret.txt` with the actual paths on your system for precise application.

This structure aims to offer a more engaging and organized way to navigate through your Obsidian Notes, making it easier to reference and utilize the commands as needed.