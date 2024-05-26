---
sticker: lucide//shield
banner: https://emtzjggfdh2.exactdn.com/wp-content/uploads/2018/06/What-Is-Digital-Forensics-and-Why-Is-It-Important.jpg?strip=all&lossy=1&ssl=1
---
## Analysis and Reconstruction - Findings.md

> [!INFO]
> `fas:InfoCircle` This document focuses on analyzing and reconstructing the timeline of events using various tools and techniques.

First, check the MD5 hash to ensure the `johnDoe.dd` file is in its original state:

![[Pasted image 20240423221521.png]]

> [!SUCCESS]
> `fas:CheckCircle` Since the file remains unchanged, the analysis can continue.

Process the file system MAC (Modification, Access, Change) data of the `johnDoe.dd` image using the `fls` command:

```bash
fls -f ntfs -r -o 63 -m / johnDoe.dd > flsoutput.fls
```

###### Command Breakdown:
- `-f ntfs`: Specifies the file system type (NTFS).
- `-r`: Enables recursive listing of files and directories.
- `-o 63`: Specifies the sector offset where the file system starts.
- `-m /`: Specifies the mount point or root directory of the file system being analyzed.

The command creates a file that can be used in Zeitline along with the Pasco files created from the `index.dat` files found on the `johnDoe.dd` image.

Start Zeitline with the following command, ensuring the app is in the same directory:

```bash
java -jar Zeitline.jar
```

![[Pasted image 20240424212909.png]]

> [!TIP]
> `fas:Lightbulb` This is what Zeitline looks like.

Navigate to `Event > Import` or use the shortcut `Ctrl + I`.

Select "FLS FilterV3" and navigate to the FLS file created earlier.

![[Pasted image 20240424213329.png]]

![[Pasted image 20240424213341.png]]

Next, import the Pasco files created from the `index.dat` files.

![[Pasted image 20240424213555.png]]

![[Pasted image 20240424214154.png]]

> [!SUCCESS]
> `fas:CheckCircle` With everything imported, the entire timeline of the system, including internet history, can be viewed.

However, this is a relatively old-school way of looking at things, and there are more streamlined methods for viewing a system's timeline.

Before using Autopsy to create a timeline, use `log2timeline.py` to create a timeline using the `johnDoe.dd` image file:

```bash
log2timeline.py --storage-file timeline.plaso johnDoe.dd
```

![[Pasted image 20240424225600.png]]

> [!TIP]
> `fas:Clock` This process may take some time.

Once completed, a file named `timeline.plaso` will be created in the directory. Use `pinfo.py timeline.plaso` to view the Plaso file:

```bash
pinfo.py timeline.plaso
```

The `pinfo.py` command can also be used to generate reports based on plugins, such as "firefox cache".

To create a CSV file that can be viewed, use `psort.py` in the same directory as the Plaso file:

```bash
psort.py -o dynamic -w timeline.csv -a timeline.plaso
```

![[Pasted image 20240424233703.png]]

> [!TIP]
> `fas:Clock` This process may take some time.

Moving on to one of the easier-to-use tools for timeline analysis: the Autopsy timeline.

![[Pasted image 20240425013518.png]]

> [!INFO]
> `fas:InfoCircle` This is what the Autopsy timeline looks like.

Clicking on any of the bars in the graph won't show anything initially, as there will be too many entries. However, the filters tab can be used to narrow down the search criteria. In this case, the focus is on images of birds or anything related to birds. It has been noticed that bird images are always under "File Created," so this should be set in the filter.

For example, a CSV file of the full web activity can be created:

![[Pasted image 20240425015320.png]]

The CSV file can be saved for further analysis:

![[Pasted image 20240425015352.png]]

The same process can be applied to file history, such as when files were created or modified. The "must have text" filter can be used to search for specific keywords, like "bird."

![[Pasted image 20240425015620.png]]

# 🦜 Bird-Related Activity Report 🐦

Based on the analysis of the provided CSV files from the Autopsy timeline feature, the following bird-related activities were identified on John Doe's computer:

## 🌐 Web History

1. **2005-02-02 14:18:13** - Accessed file: `file/Documents%20and%20Settings/johndoe/My%20Documents/My%20Pictures/tn_duck_3.jpg`
   - 💬 Comment: This indicates that John Doe viewed an image of a duck in his "My Pictures" folder.

2. **2005-02-02 14:18:14** - Accessed website: `www.linorg.usp.br`
   - 💬 Comment: John Doe visited a website related to the Mozilla Firefox browser, which he later used to search for bird-related content.

3. **2005-02-02 14:18:53** - Accessed file: `file/Documents%20and%20Settings/johndoe/My%20Documents/My%20Pictures/snow_geese.jpg`
   - 💬 Comment: John Doe viewed an image of snow geese in his "My Pictures" folder.

4. **2005-02-02 14:20:33** - Accessed file: `file/Documents%20and%20Settings/johndoe/My%20Documents/My%20Pictures/7107298.jpg`
   - 💬 Comment: John Doe viewed another bird-related image in his "My Pictures" folder.

5. **2005-02-02 14:25:59** - Accessed file: `file/Documents%20and%20Settings/johndoe/My%20Documents/aa010703a.htm`
   - 💬 Comment: John Doe accessed an HTML file named "aa010703a.htm" in his "My Documents" folder, which contains bird-related content.

6. **2005-02-02 14:29:30** - Accessed file: `file/Documents%20and%20Settings/johndoe/My%20Documents/nestboxtips.txt`
   - 💬 Comment: John Doe accessed a text file named "nestboxtips.txt" in his "My Documents" folder, indicating his interest in bird nesting.

7. **2005-02-02 14:43:36** - Accessed file: `file/Documents%20and%20Settings/johndoe/My%20Documents/My%20Pictures/40m.jpg`
   - 💬 Comment: John Doe viewed another bird-related image in his "My Pictures" folder.

8. **2005-02-03 15:49:39** - Accessed file: `file/birdwatching.doc`
   - 💬 Comment: John Doe accessed a document named "birdwatching.doc", further confirming his interest in bird-watching.

9. **2005-02-03 15:52:01** - Accessed file: `file/birds/non%20images/BirdingGuide.pdf`
   - 💬 Comment: John Doe accessed a PDF file named "BirdingGuide.pdf" in the "birds/non images" folder, providing more evidence of his bird-watching interest.

## 📁 File System Activity

1. **2005-02-02 14:26:00** - Created files:
   - `aa010703a_files/birding.gif`
   - `aa010703a_files/bluebirdhousepic.jpg`
   - 💬 Comment: John Doe created files related to birding and bluebird houses in the "aa010703a_files" folder.

2. **2005-02-02 14:28:19** - Created file: `/Documents and Settings/johndoe/My Documents/My Pictures/wbpremium_s.jpg`
   - 💬 Comment: John Doe saved a bird-related image named "wbpremium_s.jpg" in his "My Pictures" folder.

3. **2005-02-03 11:42:19** - Created files:
   - `/BellbirdJumpingOffBranch.jpg`
   - `/AmericanWhitePelicansCircling.jpg`
   - `/AmericanAvocetWinterPlumage.jpg`
   - 💬 Comment: John Doe saved multiple bird-related images, including bellbirds, pelicans, and avocets, in his computer's root directory.

4. **2005-02-03 15:00:19** - Created file: `/Documents and Settings/johndoe/My Documents/My Pictures/babyscot_vyoung.jpg`
   - 💬 Comment: John Doe saved an image of a young bird named "babyscot_vyoung.jpg" in his "My Pictures" folder.

5. **2005-02-03 15:00:27** - Created file: `/Documents and Settings/johndoe/My Documents/My Pictures/babyscot_2weeks1.jpg`
   - 💬 Comment: John Doe saved another image of a young bird, approximately two weeks old, named "babyscot_2weeks1.jpg" in his "My Pictures" folder.

6. **2005-02-03 15:05:03** - Created file: `/Documents and Settings/johndoe/My Documents/My Pictures/chicks2.jpg`
   - 💬 Comment: John Doe saved an image of chicks named "chicks2.jpg" in his "My Pictures" folder.

7. **2005-02-03 15:06:42** - Created file: `/Documents and Settings/bob/My Documents/My Music/ready2fledge.jpg`
   - 💬 Comment: John Doe saved an image of a bird ready to fledge named "ready2fledge.jpg" in the "My Music" folder of another user, "bob".

## 🕰️ Timeline of Bird-Related Downloads

Based on the file creation timestamps, John Doe first started downloading bird-related images on **2005-02-02 14:18:13** when he accessed the file `tn_duck_3.jpg` in his "My Pictures" folder. He continued to download and view various bird-related images, documents, and web pages throughout **February 2nd and 3rd, 2005**.

## 🎯 Intent and Interest in Birds

The analysis of John Doe's web history and file system activity clearly demonstrates his intent and interest in birds and bird-watching. He actively searched for and downloaded numerous bird-related images, including ducks, geese, bellbirds, pelicans, avocets, and young birds. Additionally, he accessed bird-watching guides, nest box tips, and other bird-related documents.

The timeline of his activities shows a consistent pattern of seeking out and engaging with bird-related content over the course of two days (**February 2nd and 3rd, 2005**). This behavior strongly suggests that John Doe had a genuine interest in birds and bird-watching, rather than the files being present on his computer by mere chance.

In conclusion, the evidence gathered from the Autopsy timeline CSV files provides compelling proof of John Doe's intent and interest in birds and bird-watching. His deliberate actions of downloading, viewing, and saving bird-related content on his computer over a period of two days establish a clear pattern of intent and interest in this subject matter. 🐧