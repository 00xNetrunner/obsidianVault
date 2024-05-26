---
sticker: emoji//1f4a3
banner: https://www.tees.ac.uk/Images/CommonImages/prospectus/course_images/main/digital_forensics.jpg
---
## Directed Searching - Findings.md

> [!INFO]
> `fas:InfoCircle` This document focuses on directed searching using registry hives and tools like RegRipper.

First, check the MD5 hash to ensure the `johnDoe.dd` image remains unchanged:

![[Pasted image 20240418204815.png]]

Mount the `johnDoe.dd` image file using the loopback mounting technique.

After mounting the image file, copy the registry subkeys to the case folder:

```bash
sudo cp ~/suspectDrive/WINDOWS/system32/config/SAM /home/cmp209/jd/registry
sudo cp ~/suspectDrive/WINDOWS/system32/config/SECURITY /home/cmp209/jd/registry
sudo cp ~/suspectDrive/WINDOWS/system32/config/software /home/cmp209/jd/registry
sudo cp ~/suspectDrive/WINDOWS/system32/config/system /home/cmp209/jd/registry
sudo cp ~/suspectDrive/Documents\ and\ Settings/johndoe/NTUSER.DAT /home/cmp209/jd/registry
```

After copying the files, unmount the image file and remove the loopback device:

```bash
sudo umount ~/suspectDrive
sudo losetup -d /dev/loop30
```

Navigate to the directory containing the copied files and change their permissions:

```bash
sudo chown <username> SAM SECURITY software system NTUSER.DAT
chmod u+w SAM SECURITY software system NTUSER.DAT
```

To find all user accounts on John Doe's computer, use the following command:

```bash
sudo regripper -r SAM -p samparse
```

![[Pasted image 20240423013225.png]]

> [!SUCCESS]
> `fas:CheckCircle` The command reveals the usernames and their administrative privileges.

Use **FRED (Forensic Registry EDitor)** to analyze the registry hive files. FRED allows investigators to generate reports containing valuable information about user activity, system configuration, and installed software.

Open each subkey in FRED and generate a report. For example, opening the SAM file and generating a report provides details such as usernames, RIDs, last password changes, last logins, and total logins.

![[Pasted image 20240423015112.png]]

> [!INFO]
> `fas:InfoCircle` All other reports will be included in the accompanying ZIP folder.

To identify connected devices, search for the following registry key:

```
HKEY_LOCAL_MACHINES\System\CurrentControlSet\Enum\USBSTO
```

In FRED, this key can be found under:

```
ControlSet002\Enum\USBSTOR
```

![[Pasted image 20240423020705.png]]

> [!SUCCESS]
> `fas:CheckCircle` One connected USB device was found under this key.

More information about connected USB devices can be found under `Control > Device-Classes`, specifically looking for:

```
{53f56307-b6bf-11d0-94f2-00a0c91efb8b}
```

![[Pasted image 20240423021734.png]]

> [!INFO]
> `fas:InfoCircle` Three devices were connected to John Doe's system.

To determine where these devices were mounted, consult the generated report from the system hive. The device named `Disk&Ven_&Prod_USB_DISK&Rev_1.05` was mounted at `\DosDevices\E:`.

![[Pasted image 20240423023044.png]]

Use **RegRipper** to find additional devices that may have been missed:

```bash
sudo regripper -r system -p mountdev2
```

```bash
Device: \??\FDC#GENERIC_FLOPPY_DRIVE#4&33bc18fa&0&0#{53f5630d-b6bf-11d0-94f2-00a0c91efb8b}
  \??\Volume{30bf5ac0-6e1f-11d9-a7bd-806d6172696f}
  \DosDevices\A:

Device: \??\IDE#CdRomSONY_CDU4811____________________________PY09____#5&1a8e90d7&0&0.1.0#{53f5630d-b6bf-11d0-94f2-00a0c91efb8b}
  \??\Volume{30bf5ac1-6e1f-11d9-a7bd-806d6172696f}
  \DosDevices\D:

Device: \??\STORAGE#RemovableMedia#7&2fb427dc&0&RM#{53f5630d-b6bf-11d0-94f2-00a0c91efb8b}
  \??\Volume{44d36d3b-7525-11d9-ab5a-0048545652e0}
  \DosDevices\E:
```

> [!SUCCESS]
> `fas:CheckCircle` Three devices were found, along with their mount points and volumes.

The following section lists the commands used and the information found:

**Using the Software hive:**
> [!INFO]
> Programs installed with MSI Packages

```bash
sudo regripper -r software -p msis
```

```bash
2005-02-02 17:00:19Z
Adobe Reader 7.0;C:\Program Files\Adobe\Acrobat 7.0\Setup Files\RdrBig\ENU\Adobe Reader 7.0.msi
2005-01-25 11:27:37Z
Microsoft Office Professional Edition 2003;D:\PRO11.MSI
2005-01-24 16:08:18Z
McAfee VirusScan Enterprise;C:\DOCUME~1\johndoe\LOCALS~1\Temp\McAfee VirusScan Enterprise 80\VSE800.MSI
2005-01-24 15:57:29Z
WebFldrs XP;C:\WINDOWS\system32\webfldrs.msi
```

> [!INFO]
> Uninstall keys from the Software hive.

```bash
sudo regripper -r software -p uninstall
```

```bash
2005-02-09 03:02:55Z
  Windows XP Hotfix - KB885250 v.20050118.202711

2005-02-09 03:02:42Z
  Windows XP Hotfix - KB888113 v.20041116.131036

2005-02-09 03:02:31Z
  Windows XP Hotfix - KB887472 v.20041014.162858

2005-02-09 03:02:18Z
  Windows XP Hotfix - KB891781 v.20050110.165439

2005-02-09 03:02:06Z
  Windows XP Hotfix - KB867282 v.20050127.090417

2005-02-09 03:01:33Z
  Windows XP Hotfix - KB873333 v.20050114.005213

2005-02-09 03:01:16Z
  Windows XP Hotfix - KB890047 v.20041221.124506

2005-02-09 03:00:33Z
  Windows XP Hotfix - KB888302 v.20041207.111426

2005-02-03 11:23:26Z
  WebFldrs XP v.9.50.7523

2005-02-03 10:27:51Z
  Microsoft Office Professional Edition 2003 v.11.0.6361.0

2005-02-02 17:00:18Z
  Adobe Reader 7.0 v.7.0.0

2005-02-02 16:32:02Z
  Windows Privacy Tools v.1.0rc2

2005-02-02 14:50:49Z
  RealJukebox 1.0

2005-02-02 14:50:04Z
  RealPlayer

2005-01-24 16:32:28Z
  Mozilla Thunderbird (1.0) v.1.0 (en)

2005-01-24 16:26:04Z
  Mozilla Firefox (1.0) v.1.0 (en-GB)

2005-01-24 16:21:58Z
  Windows XP Hotfix - KB890175 v.20041201.233338

2005-01-24 16:21:28Z
  Windows XP Hotfix - KB885836 v.20041028.173203

2005-01-24 16:21:16Z
  Windows XP Hotfix - KB886185 v.20041021.090540

2005-01-24 16:20:28Z
  Windows XP Hotfix - KB885835 v.20041027.181713

2005-01-24 16:20:12Z
  Windows XP Hotfix - KB873339 v.20041117.092459

2005-01-24 16:19:26Z
  Windows XP Hotfix - KB834707 v.20040929.110854

2005-01-24 16:08:17Z
  McAfee VirusScan Enterprise v.8.0.0

2005-01-24 15:57:17Z
  MPlayer2

2005-01-24 15:43:43Z
  DXM_Runtime

2005-01-24 15:43:06Z
  Branding

2005-01-24 15:39:50Z
  PCHealth

2005-01-24 15:39:40Z
  AddressBook
  DirectAnimation
  NetMeeting
  OutlookExpress

2005-01-24 15:39:39Z
  ICW

2005-01-24 15:39:29Z
  DirectDrawEx
  Fontcore
  IE40
  IE4Data
  IE5BAKEX
  IEData
  MobileOptionPack
  SchedulingAgent

2005-01-24 15:33:02Z
  Connection Manager
```

**Using the NTUSER.DAT hive:**
> [!INFO]
> Software key from the NTUSER.DAT hive.

```bash
sudo regripper -r NTUSER.DAT -p listsoft
```

```bash
2005-02-02 17:03:31Z    Microsoft
2005-02-02 16:59:47Z    Adobe
2005-02-02 16:32:22Z    WinPT
2005-02-02 16:32:07Z    GNU
2005-02-02 15:04:41Z    RealNetworks
2005-01-25 10:50:39Z    ODBC
2005-01-24 16:47:05Z    Clients
2005-01-24 16:31:47Z    Mozilla
2005-01-24 15:56:58Z    Intel
2005-01-24 15:56:58Z    Netscape
2005-01-24 15:56:58Z    Policies
```

**Using both Software and NTUSER.DAT hives:**
> [!INFO]
> App Paths subkeys from the Software hive.

```bash
sudo regripper -r software -p apppaths
```

```bash
2005-02-02 16:59:47Z
  AcroRd32.exe - C:\Program Files\Adobe\Acrobat 7.0\Reader\AcroRd32.exe
2005-02-02 14:50:14Z
  RealPlay.exe - C:\Program Files\Real\RealPlayer\realplay.exe
2005-02-02 14:49:59Z
  rnxproc.exe - C:\Program Files\Common Files\Real\Update_OB\rnxproc.exe
2005-01-25 10:49:45Z
  infopath.exe - C:\Program Files\Microsoft Office\OFFICE11\INFOPATH.EXE
  msoxmled.exe - C:\Program Files\Common Files\Microsoft Shared\OFFICE11\MSOXMLED.EXE
2005-01-25 10:49:44Z
  Winword.exe - C:\PROGRA~1\MICROS~2\OFFICE11\WINWORD.EXE
2005-01-25 10:49:43Z
  MSPUB.EXE - C:\PROGRA~1\MICROS~2\OFFICE11\MSPUB.EXE
  OUTLOOK.EXE - C:\PROGRA~1\MICROS~2\OFFICE11\OUTLOOK.EXE
  powerpnt.exe - C:\PROGRA~1\MICROS~2\OFFICE11\POWERPNT.EXE
2005-01-25 10:49:42Z
  MsoHtmEd.exe - 
2005-01-25 10:49:38Z
  excel.exe - C:\PROGRA~1\MICROS~2\OFFICE11\EXCEL.EXE
2005-01-24 16:31:47Z
  thunderbird.exe - C:\Program Files\Mozilla Thunderbird\thunderbird.exe
2005-01-24 16:24:33Z
  firefox.exe - C:\Program Files\Mozilla Firefox\firefox.exe
2005-01-24 16:12:35Z
  migwiz.exe - %SystemRoot%\system32\usmt\migwiz.exe
2005-01-24 15:43:16Z
  msimn.exe - %ProgramFiles%\Outlook Express\msimn.exe
2005-01-24 15:40:04Z
  install.exe - 
  pbrush.exe - %SystemRoot%\system32\mspaint.exe
  setup.exe - 
  table30.exe - 
  winnt32.exe - 
2005-01-24 15:39:55Z
  wmplayer.exe - C:\Program Files\Windows Media Player\wmplayer.exe
2005-01-24 15:39:51Z
  moviemk.exe - C:\Program Files\Movie Maker\moviemk.exe
2005-01-24 15:39:50Z
  HELPCTR.EXE - C:\WINDOWS\PCHealth\HelpCtr\Binaries\HelpCtr.exe
  MSCONFIG.EXE - C:\WINDOWS\PCHealth\HelpCtr\Binaries\MSConfig.exe
2005-01-24 15:39:42Z
  mplayer2.exe - "C:\Program Files\Windows Media Player\mplayer2.exe"
2005-01-24 15:39:40Z
  CONF.EXE - C:\Program Files\NetMeeting\conf.exe
  msinfo32.exe - C:\Program Files\Common Files\Microsoft Shared\MSInfo\MSInfo32.exe
  wab.exe - %ProgramFiles%\Outlook Express\wab.exe
  wabmig.exe - %ProgramFiles%\Outlook Express\wabmig.exe
2005-01-24 15:39:39Z
  ICWCONN1.EXE - "C:\Program Files\Internet Explorer\Connection Wizard\ICWCONN1.EXE"
  ICWCONN2.EXE - "C:\Program Files\Internet Explorer\Connection Wizard\ICWCONN2.EXE"
  INETWIZ.EXE - "C:\Program Files\Internet Explorer\Connection Wizard\INETWIZ.EXE"
  ISIGNUP.EXE - "C:\Program Files\Internet Explorer\Connection Wizard\ISIGNUP.EXE"
2005-01-24 15:39:29Z
  IEXPLORE.EXE - C:\Program Files\Internet Explorer\iexplore.exe
2005-01-24 15:36:16Z
  bckgzm.exe - C:\Program Files\MSN Gaming Zone\Windows\bckgzm.exe
  chkrzm.exe - C:\Program Files\MSN Gaming Zone\Windows\chkrzm.exe
  hrtzzm.exe - C:\Program Files\MSN Gaming Zone\Windows\hrtzzm.exe
  hypertrm.exe - "C:\Program Files\Windows NT\hypertrm.exe"
  pinball.exe - C:\Program Files\Windows NT\Pinball\pinball.exe
  rvsezm.exe - C:\Program Files\MSN Gaming Zone\Windows\rvsezm.exe
  shvlzm.exe - C:\Program Files\MSN Gaming Zone\Windows\shvlzm.exe
2005-01-24 15:36:15Z
  dialer.exe - C:\Program Files\Windows NT\dialer.exe
  WORDPAD.EXE - "%ProgramFiles%\Windows NT\Accessories\WORDPAD.EXE"
  WRITE.EXE - "%ProgramFiles%\Windows NT\Accessories\WORDPAD.EXE"
2005-01-24 15:35:33Z
  MSMSGS.EXE - C:\Program Files\Messenger\msmsgs.exe
2005-01-24 15:32:42Z
  cmmgr32.exe - C:\WINDOWS\system32\cmmgr32.exe
```


Now that we have a list of all the installed and uninstalled software, let's think outside the box and find more information on the system using the hive files and RegRipper.

### User Information
```bash
sudo regripper -r SAM -p samparse
```

*All Found users and info*
```bash
Username        : Administrator [500]
Full Name       : 
User Comment    : Built-in account for administering the computer/domain
Account Type    : Default Admin User
Account Created : 2005-01-24 16:10:13Z
Name            :  
Last Login Date : Never
Pwd Reset Date  : 2005-01-24 16:31:26Z
Pwd Fail Date   : Never
Login Count     : 0
Embedded RID    : 500
  --> Password does not expire
  --> Normal user account

Username        : Guest [501]
Full Name       : 
User Comment    : Built-in account for guest access to the computer/domain
Account Type    : Default Guest Acct
Account Created : 2005-01-24 16:10:13Z
Name            :  
Last Login Date : Never
Pwd Reset Date  : Never
Pwd Fail Date   : Never
Login Count     : 0
Embedded RID    : 501
  --> Account Disabled
  --> Password not required
  --> Password does not expire
  --> Normal user account

Username        : HelpAssistant [1000]
Full Name       : Remote Desktop Help Assistant Account
User Comment    : Account for Providing Remote Assistance
Account Type    : Custom Limited Acct
Account Created : 2005-01-24 15:37:21Z
Name            :  
Last Login Date : Never
Pwd Reset Date  : 2005-01-24 15:37:21Z
Pwd Fail Date   : Never
Login Count     : 0
Embedded RID    : 1000
  --> Account Disabled
  --> Password does not expire
  --> Normal user account

Username        : SUPPORT_388945a0 [1002]
Full Name       : CN=Microsoft Corporation,L=Redmond,S=Washington,C=US
User Comment    : This is a vendors account for the Help and Support Service
Account Type    : Custom Limited Acct
Account Created : 2005-01-24 15:42:04Z
Name            :  
Last Login Date : Never
Pwd Reset Date  : 2005-01-24 15:42:04Z
Pwd Fail Date   : Never
Login Count     : 0
Embedded RID    : 1002
  --> Account Disabled
  --> Password does not expire
  --> Normal user account

Username        : johndoe [1003]
Full Name       : 
User Comment    : 
Account Type    : Default Admin User
Account Created : 2005-01-24 15:56:49Z
Name            :  
Last Login Date : 2005-02-09 16:49:18Z
Pwd Reset Date  : 2005-01-24 16:36:30Z
Pwd Fail Date   : 2005-02-02 15:08:27Z
Login Count     : 21
Embedded RID    : 1003
  --> Password does not expire
  --> Normal user account

Username        : jane [1004]
Full Name       : jane
User Comment    : 
Account Type    : Custom Limited Acct
Account Created : 2005-02-02 12:36:29Z
Name            :  
Last Login Date : 2005-02-03 11:23:04Z
Pwd Reset Date  : 2005-02-02 12:37:25Z
Pwd Fail Date   : 2005-02-02 15:08:27Z
Login Count     : 1
Embedded RID    : 1004
  --> Password does not expire
  --> Normal user account

Username        : bob [1005]
Full Name       : bob
User Comment    : 
Account Type    : Custom Limited Acct
Account Created : 2005-02-02 15:08:39Z
Name            :  
Last Login Date : 2005-02-03 10:12:34Z
Pwd Reset Date  : 2005-02-02 15:08:54Z
Pwd Fail Date   : Never
Login Count     : 1
Embedded RID    : 1005
  --> Password does not expire
  --> Normal user account
```

*Group Membership Information*
```bash
Group Name    : Guests [1]
LastWrite     : 2005-01-24 16:10:13Z
Group Comment : Guests have the same access as members of the Users group by default, except for the Guest account which is further restricted
Users :
  S-1-5-21-725345543-854245398-1202660629-501

Group Name    : Remote Desktop Users [0]
LastWrite     : 2005-01-24 16:10:13Z
Group Comment : Members in this group are granted the right to logon remotely
Users         : None

Group Name    : Users [4]
LastWrite     : 2005-02-02 15:08:39Z
Group Comment : Users are prevented from making accidental or intentional system-wide changes.  Thus, Users can run certified applications, but not most legacy applications
Users :
  S-1-5-4
  S-1-5-21-725345543-854245398-1202660629-1005
  S-1-5-11
  S-1-5-21-725345543-854245398-1202660629-1004

Group Name    : Replicator [0]
LastWrite     : 2005-01-24 16:10:13Z
Group Comment : Supports file replication in a domain
Users         : None

Group Name    : Power Users [0]
LastWrite     : 2005-01-24 16:10:13Z
Group Comment : Power Users possess most administrative powers with some restrictions.  Thus, Power Users can run legacy applications in addition to certified applications
Users         : None

Group Name    : Administrators [2]
LastWrite     : 2005-01-24 15:56:49Z
Group Comment : Administrators have complete and unrestricted access to the computer/domain
Users :
  S-1-5-21-725345543-854245398-1202660629-1003
  S-1-5-21-725345543-854245398-1202660629-500

Group Name    : Network Configuration Operators [0]
LastWrite     : 2005-01-24 16:10:13Z
Group Comment : Members in this group can have some administrative privileges to manage configuration of networking features
Users         : None

Group Name    : Backup Operators [0]
LastWrite     : 2005-01-24 16:10:13Z
Group Comment : Backup Operators can override security restrictions for the sole purpose of backing up or restoring files
Users         : None

Analysis Tips:
 - For well-known SIDs, see http://support.microsoft.com/kb/243330
     - S-1-5-4  = Interactive
     - S-1-5-11 = Authenticated Users
 - Correlate the user SIDs to the output of the ProfileList plugin
```

### Network Information: 
```bash
regripper -r software -p networkcards
```

```bash
NetworkCards
Microsoft\Windows NT\CurrentVersion\NetworkCards

Description                                        Key LastWrite time                                
Realtek RTL8139 Family PCI Fast Ethernet NIC       2005-01-24 16:15:57Z  
```

### Recent Activity:
```bash
regripper -r NTUSER.DAT -p recentdocs
```

*Recent Docs*
```bash
RecentDocs
**All values printed in MRUList\MRUListEx order.
Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
LastWrite Time: 2005-02-09 17:06:28Z
  38 = New Volume (F:)
  11 = AlmondMarshGreatBlueHeronStalling.jpg
  37 = MSN
  18 = aggressive_song.wav
  36 = stuf.doc
  35 = birds.zip
  34 = WINDOWS
  33 = ODBC.INI
  14 = non images
  13 = BirdingGuide.pdf
  32 = BookList.doc
  21 = Local Disk (C:)
  5 = birdwatching.doc
  31 = My Music
  3 = ready2fledge.jpg
  2 = newbies2.jpg
  1 = My Pictures
  0 = chicks2.jpg
  30 = birdtrans2.jpg
  29 = ostbk2b2.htm
  28 = 177.jpg
  27 = babyscot_2weeks1.jpg
  26 = babyscot_vyoung.jpg
  25 = birds
  24 = Killdeer.jpg
  23 = Sample Music
  22 = Doc1.doc
  20 = EvanstonWoodpecker.jpg
  19 = audio
  17 = bookmarks.html
  15 = cookies.txt
  12 = kakapo.ram
  10 = Q3 Thread (Statechart).gif
  9 = Prac4
  8 = Prac4.gif
  6 = nestboxtips.txt
  4 = aa010703a.htm
```

*Recent jpgs*
```bash
Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.jpg
LastWrite Time 2005-02-09 17:06:28Z
MRUListEx = 4,3,2,1,0,9,8,7,6,5
  4 = AlmondMarshGreatBlueHeronStalling.jpg
  3 = ready2fledge.jpg
  2 = newbies2.jpg
  1 = chicks2.jpg
  0 = birdtrans2.jpg
  9 = 177.jpg
  8 = babyscot_2weeks1.jpg
  7 = babyscot_vyoung.jpg
  6 = Killdeer.jpg
  5 = EvanstonWoodpecker.jpg
```

As always, check the integrity of the `johnDoe.dd` file to ensure the image file remains unchanged:

![[Pasted image 20240423144732.png]]
