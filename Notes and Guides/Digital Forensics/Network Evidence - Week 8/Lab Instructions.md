# 🔬 Module CMP209 – Digital Forensics 

## Lab 8: Browser Forensics 🌐

👨‍🏫 **Module Lecturer:** Dr Karl van der Schyff

📅 **Due Date:** Complete before your next lab

### Lab Instructions

#### ❓ Where (and what) to Submit?

Note that this lab is formative in nature. In other words, although you are expected to complete it, I do not require you to hand it in for assessment. Having said this, I am more than happy to give feedback if you wish to receive it. 😊

#### 🚫 What About Using AI? 

Use of Generative AI Tools such as ChatGPT, DALL-E, Bard etc. is **explicitly prohibited** for this module's assessment (i.e., the court report). The output gleaned or generated from the tools mentioned and others that are similar relates to sources already published/available. Also, be aware that the information obtained can be inaccurate or incomplete. Thus, all work should be your own. If the assessment (i.e., court report) is found to have been plagiarised or to have used unauthorised AI tools, you will be referred to the Student Disciplinary Officer within the School and this may result in an Academic Misconduct charge. ⚠️

### Lab Requirements

#### 💻 What Do I Need to Complete It?

- Access to the analysis workstation, which is an Ubuntu VM that can be accessed via VMware Workstation. 
- You can also download the analysis VM from MLS via the tile labelled "OneDrive link to analysis VM download files".
- Access to MLS to download the above (and the supplementary docs), but also the John Doe image file. The image file is labelled `johnDoe.dd.gz`

### 🎯 Aim of This Lab 

The aim of this lab is to:
- Examine the browser data on John Doe's disk image.

### 📚 Core Learning Outcomes

- Increased understanding and competence using the Linux operating system to perform digital forensics. 🐧
- Familiarization with the processes required to conduct a digital forensic investigation (read the supplementary docs for this). 🔍 
- Learn how to forensically examine the browser activity of a Windows user. 🖥️

---

## 🔍 Browser Analysis 

### Part 1: Recovering Data from the Image

Please read all the lab instructions before starting with your analysis. As stated in the learning outcome, the aim of this lab is to understand (and learn) how to extract a user's web-browsing activity from various files, databases and other digital artifacts that have been left behind as a consequence of normal browsing activity. 🌐

For this lab, you have a choice: 

- Follow a command line-based approach 💻
- Select another tool, such as:
  - Autopsy (already installed on the Ubuntu analysis VM). You can also download and install Autopsy for Windows if you are doing your analysis on Windows.
  - WEFA which can be downloaded from the supplementary section for Week 8. 
  - Any of the tools mentioned in the article by Oh, Lee & Lee (2011). The article is also available in the supplementary section for Week 8.

Remember, whichever route you choose, you should recover and analyze **at least** the following from the John Doe image:

✅ Browsing history
✅ Cache  
✅ Bookmarks
✅ Cookies

It is also really important to become increasingly inquisitive. You should be trying to figure out what John Doe has done by trying to figure out what John Doe was viewing and opening. 🤔 Additionally:

- Keep in mind the kind of crime(s) that you are looking for evidence of. 🕵️
- Remember that you are not just looking for images of birds, but wider involvement in any bird-related activities. Eventually you will need to recommend if John Doe should be charged and if so whether it is with "possession of inappropriate images of birds" and/or "distribution of inappropriate images of birds". 🦜
- Keep in mind that you should be looking to reconstruct not only what John Doe did, but also how he did it (and possibly why). Essentially, you are looking to prove mens-rea (i.e., intention). ⚖️

#### Steps:

1. Ensure that you change to the directory that contains your John Doe image and related files. Up to now the labs have been using the `jd` directory for these files. 📂

2. Check the md5 checksum before proceeding. ✅

3. Create a sub-directory called `browser` and change to that directory. Then, create another sub-directory called `ie`. Once created, change to this new sub-directory. 📁

4. As before, mount the `johnDoe.dd` image file using a loopback approach:

```bash
sudo losetup -o 32256 /dev/loop30 ~/jd/johnDoe.dd 
sudo mount -o ro -t ntfs /dev/loop30 ~/suspectDrive
```

Ensure that the directory you are mounting the loopback to exists before executing the above command. If not, you will receive errors similar to this:

```
ntfs-3g-mount: bad mount point /home/cmp209/suspectDrive: No such file or directory  
```

### Part 2: Analyzing Internet Explorer Data

6. Given that John Doe used an old computer running XP, we need to locate the Internet Explorer cache at:

```
Documents and Settings/<username>/Local Settings/Temporary Internet Files/Content.IE5
```

Note: the location above depends on what version of Windows is used and may differ depending on the version of Internet Explorer (IE) in use.

7. Locate the cache file (one of the `index.dat` files – there are several) and use `pasco` to examine its contents for evidence that may be of use. Run these commands:

```bash
pasco index.dat > ~/jd/index.pasco
```

Then, open the `index.pasco` file in a text editor or spreadsheet program, such as LibreOffice Calc or Excel. Note that there may be several users' data to inspect, so name these files appropriately.

8. ==Locate the IE History which is at:== 

```
Documents and Settings/<username>/Local Settings/History/History.IE5
``` 

As before, note that this location will vary depending on the operating system and browser versions in use. The IE cookies file is called `index.dat` and is located in a sub-directory called `Cookies`.

9. It might be easier to find the cookies associated with the John Doe case by using Autopsy. The figure below shows what this area in Autopsy would look like. Just please note that the screenshot used below is from another case study (i.e., not the John Doe case).

![Autopsy Cookie View](autopsy_cookies.png)

10. You should also use the `find` command to locate all other `index.dat` files. Once located, please investigate their contents using the `strings` command for example.

11. In addition to the above, bookmarks, typed URLs, recently used items, and autocompletion-type data may be available. Use Google as a means to research how these could be located and attempt to recover additional (and useful) evidence. You could also use Bulk Extractor. Just note that the Windows version of this tool (specifically v1.5.5) is typically used and that you will require Java to run it. There is a video lecture in Week 10's MLS webpage which demonstrates how to use Bulk Extractor.

### Part 3: Cleaning Up

12. Unmount the `johnDoe` image and remove the loopback device:

```bash
sudo umount ~/suspectDrive
sudo losetup -d /dev/loop30  
```

13. Wait a second...maybe John Doe used other browsers in addition to Internet Explorer. 🤔 How should we gather evidence from them? Consider the following:

- Firefox uses SQLite databases to store data like history, bookmarks, etc. You can use tools like SQLite Browser or Autopsy to examine these.
- Chrome also uses SQLite, as well as LevelDB for storing data. Autopsy, WEFA, and Chrome Forensics can help analyze these.
- For Safari, look into Property Lists (plists) which store preference data. Cookies are in Binary Cookies (binarycookies) format.

Certainly! Here's an in-depth guide on how to gather evidence from different browsers in addition to Internet Explorer:

# 🔍 Investigating Browsers on a Windows XP Image

When conducting a digital forensic investigation on a Windows XP system image, the locations of browser-related files differ from newer versions of Windows. In this guide, we'll focus on finding and analyzing web data from Firefox, Chrome, and Safari browsers.

## Mounting the Windows XP Image on SIFT Workstation

Before proceeding with the browser forensics, ensure that you have the Windows XP system image file mounted on your SIFT Workstation. You can use tools like FTK Imager or Arsenal Image Mounter to mount the image file.

Once the image is mounted, take note of the mount point or the directory where the image is accessible.

## Firefox Forensics 🦊

To locate and analyze Firefox data on the mounted Windows XP image, follow these steps:

1. Open a terminal on your SIFT Workstation.

2. Navigate to the mounted Windows XP file system using the `cd` command followed by the mount point or directory path.

3. Change to the Firefox profile directory:
   ```
   cd "Documents and Settings/johndoe/Application Data/Mozilla/Firefox/Profiles/w4nf3obl.default"
   ```

4. Analyze the relevant files:

   - `bookmarks.bak` and `bookmarks.html`:
     - These files contain the user's bookmarks.
     - Use a text editor or a bookmark parser to view and extract the saved bookmarks and organized folders.

   - `cookies.txt`:
     - This file contains the user's cookies.
     - Use a text editor or a cookie parser to examine the stored cookies and identify visited websites.

   - `formhistory.dat`:
     - This file stores the user's form history data.
     - Use a form history parser or a binary file reader to extract entered form data.

   - `history.dat`:
     - This file is related to the user's browsing history.
     - Use a history parser or a binary file reader to reconstruct the browsing history and identify visited websites.

   - `prefs.js`:
     - This file contains Firefox preferences and configurations.
     - Use a text editor to review the user's preferences and settings.

5. Document your findings:
   - Make note of any significant information found in the analyzed files.
   - Record the methods and tools used to extract and interpret the data.

## Chrome Forensics 🌐

To locate and analyze Chrome data on the mounted Windows XP image, follow these steps:

1. Open a terminal on your SIFT Workstation.

2. Navigate to the mounted Windows XP file system using the `cd` command followed by the mount point or directory path.

3. Look for the Chrome user data directory in the following location:
   ```
   C:\Documents and Settings\<username>\Local Settings\Application Data\Google\Chrome\User Data\Default\
   ```
   Replace `<username>` with the actual user profile name.

4. Use the `ls` command to list the contents of the Chrome user data directory and identify the relevant files:
   - `History`: SQLite database containing the browsing history.
   - `Bookmarks`: JSON file storing bookmark information.
   - `Cookies`: SQLite database storing cookie data.
   - `Web Data`: SQLite database containing autofill data and other web-related information.
   - `Login Data`: SQLite database storing saved login credentials.

5. Copy the identified files to your SIFT Workstation using the `cp` command.

6. Use tools like Autopsy or Chrome Forensics to analyze the copied Chrome data files.

## Safari Forensics 🍎

Safari is primarily used on macOS systems. However, if Safari was installed on the Windows XP system, you can attempt to locate its data files.

1. Open a terminal on your SIFT Workstation.

2. Navigate to the mounted Windows XP file system using the `cd` command followed by the mount point or directory path.

3. Look for the Safari data directory in the following location:
   ```
   C:\Documents and Settings\<username>\Application Data\Apple Computer\Safari\
   ```
   Replace `<username>` with the actual user profile name.

4. Use the `ls` command to list the contents of the Safari data directory and identify any relevant files, such as `History.plist`, `Bookmarks.plist`, or `Downloads.plist`.

5. Copy the identified files to your SIFT Workstation using the `cp` command.

6. Analyze the copied Safari data files using tools like Xcode, PlistEdit Pro, or Autopsy.

## Additional Considerations

- If you suspect that important data may have been deleted, consider using file carving techniques or analyzing the unallocated space of the disk image to attempt the recovery of deleted or fragmented data.
- Look for any additional browser-related files or directories in other locations on the Windows XP image, as the user may have multiple profiles or non-default installations.
- Be aware that the absence of SQLite databases in Firefox may limit the amount of recoverable data, as some information might have been stored in those databases in newer versions of Firefox.

## Conclusion

When investigating browsers on a Windows XP system image mounted on SIFT Workstation, it's crucial to navigate to the correct directories and locate the relevant browser data files.

For Firefox, focus on examining the available configuration files and data files, such as `bookmarks.bak`, `cookies.txt`, `formhistory.dat`, `history.dat`, and `prefs.js`. Use a combination of text editors, specialized parsing tools, and custom scripts to extract and interpret the data.

For Chrome, look for the SQLite databases and associated files that store browsing history, bookmarks, cookies, and other web-related information. Use tools like SQLite Browser, Autopsy, or browser-specific forensic tools to examine the data.

If Safari was installed on the Windows XP system, attempt to locate its data files in the corresponding directory and analyze them using appropriate tools.

Remember to document your findings, maintain the integrity of the evidence, and follow proper legal procedures throughout the investigation process. Happy investigating! 🕵️‍♂️💻

![[Lab_8_Instructions_v2.pdf]]