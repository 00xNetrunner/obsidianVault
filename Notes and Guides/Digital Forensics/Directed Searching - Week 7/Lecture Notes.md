---
sticker: lucide//save
banner: https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRYbnYdOkIqW8cmxrQtclh4-HP4lOGpBIwXUvo1mMAuaIjiPdEYW00v8WB5WxszYLFHi9E&usqp=CAU
---
# CMP209 - Digital Forensics Notes and Key Points

## Directed Searching

* Now that we have found images of the birds, we need to start thinking about who else used the computer, and what did they do on it.
* We need to prove mens rea, a Latin phrase meaning "guilty mind." Mens rea can take various forms, including:
    * Intent
    * Knowing
    * Reckless
    * Negligent
* We are presenting evidence, not building a legal case.

## Windows Registry

* Look at the Windows Registry like it's a database
* The Windows Registry is a hierarchical database of registry entries arranged in 'hives'
* There are five Hives in modern Windows OS, all of which are located under `%SystemRoot%\system32\config`
* These five hives (also known as root keys) are:
    * `HKEY_CURRENT_USER` (HKCU)
    * `HKEY_USER` (KHU)
    * `HKEY_CLASSES_ROOT` (HKCR)
    * `HKEY_LOCAL_MACHINE` (HKLM) (Most valuable root key during an investigation)
    * `HKEY_CURRENT_CONFIG` (HKCC)
* The following subkeys in `HKLM` are of particular interest:
    * `SAM` (security accounts manager) - Location of hashed user passwords
    * `Security` - Domain-related security information.
    * `Software` - Repository for software and Windows settings, can also find malware-related software changes

## Forensic Readiness

* Forensic readiness is important from both a technical and managerial perspective
    * Technically, organizations may have network devices configured with high levels of logging and security policies restricting access to certain OS functions
    * Managerially, organizations should have plans in place to deal with incidents and designate responsibility for developing and updating such plans
    * If an organization is prepared and "forensically ready," directed searching becomes easier as evidence is already being collected

## Evidence from the Operating System

* Evidence from the operating system typically consists of:
    * Configuration information relating to the computer's users, installed applications, devices used or connected, and usage-related information
    * Accidental information such as `pagefile.sys`, `hiberfil.sys`, and print spoolers (in Windows)
    * Logged evidence such as system logs, crash logs, and application logs

## Windows Registry as a Valuable Source of Evidence

* `HKEY_CURRENT_USER` (HKCU) is another valuable root key, focusing on specific user accounts or profiles
    * Each user's `NTUSER.DAT` file, containing their profile settings, can be found in the root of their profile directory (e.g., `C:\Documents and Settings\johndoe`)
* `System` subkey in `HKLM` contains Windows system configuration, including currently mounted devices
* Registry entries have specific types: strings, binary values, DWORD values, multi-string, and expandable string
* The LastWrite Time of a registry entry can be useful during investigations

## Determining Who Has Been Using the Computer

* Inspect user directories that have been created (e.g., `C:\Documents and Settings\<userProfile>`)
* Check system profiles for user profiles without a directory
* Recover user passwords using tools like `chntpw`, `pwdump`, or `fgdump`
* Analyze the Windows registry to investigate users, installed software, and attached devices

## Using the Registry to Gather Evidence

* Mount the suspect's disk image as a loopback device
* Locate and open the registry files using a suitable registry viewer (e.g., `regripper`, `chntpw`, `fred`)
* Browse to specific registry locations to find information on users, installed software, USB devices, wireless networks, wired networks, and recently used content

## Accidental Evidence

* Accidental evidence, such as swap files or page files (`pagefile.sys`, `hiberfil.sys`, print spool files), can be investigated using string searching with `grep` or utilities that understand spool file contents

## Logfile Evidence

* Logfile evidence is important, but unlikely to have formal log management in personal computer or mobile phone investigations
* In an organizational context, tools like SIEMs (Splunk, ELK Stack, Security Onion) and following NIST guidelines for log management become relevant
* When examining multiple Windows systems with thousands of logfile entries, tools like `DeepBlueCLI` can help filter events and highlight suspicious entries (e.g., service creation, account creation, high number of login attempts and failures, malicious PowerShell use)

## Investigating Installed Applications

* Investigating installed applications requires looking at multiple sources to eliminate false positives:
    * Data about the application itself (e.g., Word document metadata)
    * Revision logs can be investigated using `strings`, `grep`, and Perl