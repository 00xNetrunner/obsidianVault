## Reverse Image Searching `fas:Search`

Reverse image searching involves using an image to find information about it online. In this course, we will use the provided images:

![[Course Photos.canvas]]

To begin, save the image you want to use and drag and drop it into one of the following websites:

### General Image Searching Sites
- [Google Image Search](https://images.google.com/)
- [Yandex](https://yandex.com/images/)
- [TinEye](https://tineye.com/)

### Face Reverse Image Searching
- [FaceCheck ID {FREE}](https://facecheck.id/)
- [PimEyes {PAID}](https://pimeyes.com/en)

### Google Images (Google Lens) `fas:Google`
![[Pasted image 20240526053615.png]]

When you drag and drop your first image into Google Images/Google Lens, you may find information about the person in the image. For example, we found a person named Heath Adams.

Here is another example with the co-owner of Mezzino, a student accommodation property manager:

![[Pasted image 20240526054438.png]]

These people may have used the picture elsewhere. You can find more sources by clicking the button that says *Find image source*.

![[Pasted image 20240526054637.png]]

By doing so, we discovered two more places where the image was used. The last four images are similar images that Google Lens identified.

Google Lens has another useful feature: it can identify the location where a photo was taken. For instance, by cropping out the couple in the image and focusing on the building behind them, Google Lens can identify the building:

![[Pasted image 20240526055224.png]]

We found that the couple took the photo outside Trinity Church in Boston. This method can also be used to identify houses and office buildings, though it may not work with generic-looking buildings.

### Yandex and TinEye `fas:CameraRetro`
Yandex and TinEye operate similarly to Google Images.

### PimEye and FaceCheck ID `fas:UserCircle`
PimEye is a powerful OSINT tool, though it requires a paid subscription to see the sources of the image. It can still find other photos of the same person, which can be useful.

Let's add some photos and see what we can find:

![[Pasted image 20240526060728.png]]

Select the two images of the same person:

![[Pasted image 20240526060852.png]]

We found more images of the same person, including photos from conventions or YouTube videos, and possibly many from social media. Unfortunately, as a free user, you won't be able to open the sources of these images, but you can save them and use them in Google Lens or Yandex again.

![[Pasted image 20240526061416.png]]

We can use a free alternative, though it's not as effective as PimEye. Let's see what we can find:

![[Pasted image 20240526061821.png]]

While not as comprehensive as PimEye, it still yields some results.

> [!warning]
> Both of these services now require payment. They were free a few months ago, but now you need to pay to use them.

### EXIF Data `fas:InfoCircle`

> [!info]
> **Exchangeable Image File Format (EXIF)** is the standard for storing information in image files. Most digital cameras and phones use it. EXIF data typically includes information such as when the photo was taken, what device was used, resolution, shutter speed, focal length, etc.

There are many sites and programs that allow you to view EXIF data. Jimpl is one such site, but there are many others available for Windows and Linux.

In the example below, I use a tool called Metacam:

![[Pasted image 20240526064056.png]]

Metacam can be a useful tool. For instance, if you have many photos, you can use the `grep` command to find all photos taken with an iPhone or a Canon camera. This can be done with a script like this:

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

### Physical Location OSINT `fas:MapMarkerAlt`

> [!important]
> As a Red Teamer, it is good practice to scout the location via Google Maps. Look for back entries, cameras, and anything that can be exploited by the team.

This approach can be very effective in preparing for on-site assessments and operations.