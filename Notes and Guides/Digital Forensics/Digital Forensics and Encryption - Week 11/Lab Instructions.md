# Digital Forensics and Encryption Lab

## Introduction

In this lab, we will explore digital forensics techniques related to encryption and password cracking. We will be working with the John Doe disk image (`johnDoe.dd`) to decrypt encrypted files and recover passwords using various tools and methods.

## Objectives

1. Decrypt GPG-encrypted files found on John Doe's disk image.
2. Perform brute-force and dictionary attacks to crack passwords.
3. Use John the Ripper (JtR) and Hashcat for password cracking.
4. Analyze encrypted PDF files and recover their passwords.
5. Explore the Thunderbird installation to find stored passwords.
6. Utilize the `rockyou.txt` word list for dictionary attacks.

## Prerequisites

`fas:CheckSquare` Access to the SIFT Workstation.
`fas:CheckSquare` John Doe's disk image (`johnDoe.dd`).
`fas:CheckSquare` `rockyou.txt` word list.

## Instructions

### Part 1: Recovering GPG Keys

1. Mount John Doe's disk image using the loopback mounting technique.
2. Locate the `C:/Documents and Settings/johndoe/Application Data/GnuPG` directory.
3. Copy the entire `GnuPG` directory to your working directory (e.g., `~/jd/GnuPG`).
   ```bash
   cp -r "/mnt/johndoe/Documents and Settings/johndoe/Application Data/GnuPG" ~/jd/GnuPG
   ```
4. Locate the `birdpics.gpg` file and copy it to your copy of the `GnuPG` directory.
5. Change the file permissions of all files in the `GnuPG` directory to read-only.
   ```bash
   chmod 400 ~/jd/GnuPG/*
   ```
6. Convert the stored GPG password to a format compatible with John the Ripper.
   ```bash
   gpg2john ~/jd/GnuPG/secring.gpg > ~/jd/GnuPG/secring.jtr
   ```
7. Use John the Ripper to brute-force the password.
   ```bash
   john ~/jd/GnuPG/secring.jtr
   ```
   > [!TIP]
   > `fas:LightbulbExclamation` Be patient, as the brute-force attack may take some time depending on the complexity of the password.

8. Once the password is recovered, import John Doe's secret key.
   ```bash
   gpg --import ~/jd/GnuPG/secring.gpg
   ```
9. Decrypt the `birdpics.gpg` file.
   ```bash
   gpg -d ~/jd/GnuPG/birdpics.gpg > ~/jd/birdpics.dat
   ```
   > [!WARNING]
   > `fas:ExclamationTriangle` Be cautious when handling decrypted files, as they may contain sensitive or confidential information. Ensure that you have the necessary legal authority and follow proper procedures when working with such files.

10. Determine the file type of `birdpics.dat`.
```bash
file ~/jd/birdpics.dat
```

> [!INFO]
    > `fas:InfoCircle` The output should indicate that `birdpics.dat` is a ZIP archive.

11. Rename `birdpics.dat` to `birdpics.zip` and extract its contents.

### Part 2: Brute-Force vs. Dictionary Attacks

12. Generate a word list from John Doe's disk image.
    ```bash
    strings johnDoe.dd > johnDoe.dd.strings
    ```
13. Use John the Ripper in dictionary mode with the generated word list.
    ```bash
    john -wordlist:johnDoe.dd.strings ~/jd/GnuPG/secring.jtr
    ```
14. Explore the Thunderbird installation on John Doe's disk to find stored passwords.
    - Locate the Thunderbird profile directory on John Doe's disk. The default location for Thunderbird profiles on Windows is:
      ```
      C:\Documents and Settings\johndoe\Application Data\Thunderbird\Profiles\
      ```
    - Copy the `key3.db` or `key4.db` file to your working directory.
    - Install `ssltool` on your SIFT Workstation.
      ```bash
      sudo apt-get install ssltool
      ```
    - Extract the passwords from the `key3.db` or `key4.db` file.
      ```bash
      ssltool -D -d <path_to_key3.db_or_key4.db>
      ```
15. Locate the password-protected PDF files recovered during the file carving process.
16. Copy the password-protected PDF files to a new directory (e.g., `~/jd/encpdf`).
17. ==Create a hash file for each password-protected PDF using the `pdf2john.pl` tool.==
    ```bash
    pdf2john.pl ~/jd/encpdf/RdrMsgENU.pdf > ~/jd/encpdf/RdrMsgENU.hash
    ```
18. Use John the Ripper to crack the PDF password.
    ```bash
    john ~/jd/encpdf/RdrMsgENU.hash
    ```

    > [!TIP]
    > `fas:LightbulbExclamation` Using a combination of the `rockyou.txt` wordlist and your custom-generated wordlist can increase the chances of successfully cracking the PDF password.

### Part 3: Using Hashcat for Password Cracking

19. Install Hashcat on your SIFT Workstation.
20. Prepare the hash files for the encrypted files (GPG and PDF) in a format compatible with Hashcat.
21. Use Hashcat with the `rockyou.txt` word list to perform dictionary attacks on the hash files.
    ```bash
    hashcat -m 0 -a 0 ~/jd/GnuPG/secring.hash rockyou.txt
    ```
22. If the dictionary attack fails, use Hashcat to perform brute-force attacks.
    ```bash
    hashcat -m 0 -a 3 ~/jd/GnuPG/secring.hash
    ```
    > [!WARNING]
    > `fas:ExclamationTriangle` Brute-force attacks can be time-consuming and resource-intensive. Consider the complexity of the password and available computing resources when deciding to perform a brute-force attack.

### Part 4: Additional Techniques

23. Explore online password cracking utilities and services for additional password cracking options.
24. Consider using GPU-accelerated password cracking tools like Hashcat or oclHashcat for faster cracking speeds.
25. Analyze the recovered files and passwords for any relevant information related to the case.

## Conclusion

In this lab, we have explored various techniques for decrypting encrypted files and recovering passwords in the context of digital forensics. We used tools like John the Ripper and Hashcat to perform brute-force and dictionary attacks on GPG-encrypted files and password-protected PDFs. Additionally, we explored the Thunderbird installation to find stored passwords and utilized the `rockyou.txt` word list for dictionary attacks.

Remember to document your findings and maintain a clear chain of custody throughout the investigation process. The recovered files and passwords may provide valuable insights into the case and help uncover additional evidence.

> [!IMPORTANT]
> `fas:ExclamationCircle` Always adhere to legal and ethical guidelines when conducting digital forensics investigations. Ensure that you have the necessary permissions and authority to access and analyze the evidence.

`fas:LaptopCode` Happy investigating! `fas:MagnifyingGlass`

![[Lab_10_Instructions3.pdf]]