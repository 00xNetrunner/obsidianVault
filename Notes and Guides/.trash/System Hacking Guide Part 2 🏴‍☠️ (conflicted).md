---
aliases:
  - 00xNetrunner
tags:
  - Pentesting
sticker: emoji//1f3f4
---
![[SystemHacking_Figure_1.png|200]]


# 1.0 GENERIC WINDOWS SERVER HACKING METHODS
-------
The following describes some of the generic simple attacks that are used by penetration testers.

## 1.1 INFORMATION FOUND IN THE USER ACCOUNTS DESCRIPTION
------------
AD accounts possess multiple attributes, including phone numbers, email addresses, and others. Among these attributes, the description holds significant importance as it is commonly utilized for concise annotations that depict users, such as "Head of IT." Nevertheless, on certain occasions, penetration testers have detected instances where these comments might contain genuine passwords or indications related to passwords.

**Active Directory Explorer** (AD Explorer) is a simple Active Directory viewer and editor.

- Start **server 1** (Tutorial Network)
- Start **Client 1** 
- Connect to **Client 1**
- Login as the pen test user (**test/test123**)
- Copy **ADexplorer** from the tools folder to the desktop on Client1.
- Run **ADexplorer** 
- Connect to ***192.168.10.1*** with Username *test* and Password *test123*
![[Pasted image 20231119054851.png]]

- Select the server and then the Search icon. 
![[Pasted image 20231119054948.png]]

- search for an **ATTRIBUTE** called *description* of *not empty*
![[Pasted image 20231119055047.png]]
> Select Add


- Then Select Search. 
![[Pasted image 20231119055337.png]]#

This provides a comprehensive compilation of all items in AD accompanied by some form of description. It is worth noting that, upon closer examination of the bottom section of the list, Della Woods is observed to possess the description "**Thisisverysecret1**"

- Double click on Della Woods and we get all the AD Information about the user, including the SamAccountname i.e username of D.Woods.

![[Pasted image 20231119055846.png]]

- To authenticate the validity of the provided password, an empirical measure can be undertaken by initiating a command prompt on Client1 and attempting the following operation: -

```powershell
runas /user:uadtargetnet\D.Woods powershell.exe
```

If this password is input by the user, it will initiate the execution of PowerShell, subsequently granting access to the user's account under the name D.Woods. In the absence of a correct password, the designated command sequence will not be executed.

> Another common search used by pen testers in the description field would include pass or password: - 
>> e.g the description might be **password = test123** 

## 1.2 INFORMATION FOUND IN SHARES
--------
Many times, network files contain a wealth of valuable information. These files may have been placed there either by the user or by the system administrator. System administrators often utilise scripts to automate various processes, but sometimes passwords inadvertently slip through, with some even being hard-coded into the scripts. As an illustration, consider the following file, which contains login credentials for *V. Vargas* with a password of *sHkQa8XFC.*
![[Pasted image 20231119063308.png]]

Common file types and the strings that contain passwords are: -

| File Extension | Keywords                                   |
| -------------- | ------------------------------------------ |
| **.ps1**           | ConvertTo-SecureString, SqlConnection, LdapConnection, NetworkCredential |
| **.vbs**           | strDomain, strPassword                     |
| **.sql**           | Trusted_Connection, Integrated Security, Connect |
| **.txt**           | pwd, pas                                   |

### 1.21 Find all shares on a machine
To find the shared folders on a machine on the network, the net view command can be a good way to do this. form a CMD prompt on Client1: - 

```powershell
net view \\Server1
```

![[Pasted image 20231119064322.png]]
> Note that any machine on a network can be viewed using this command

### 1.2.2 Searching all files in a share for keywords
Lets search all files on a share for password, or other clues, we can do this by using this simple PowerShell command,

	- Run cmd and type in - powershell

Now input the following: - 

```powershell
Get-ChildItem -Path "\\Server1\Resources\" -Recurse | Select-String -Pattern "strPassword"
```

And

```powershell
Get-ChildItem -Path "\\Server1\HR\" -Recurse | Select-String -Pattern
"strPassword"
```
#### Command Breakdown:

1. **Get-ChildItem**
    
    - **Description**: A cmdlet in PowerShell used to retrieve items (files and directories) in a specified location.
    - **Function**: Similar to `ls` in UNIX or `dir` in Windows command line, but more powerful with additional parameters.
2. **-Path "\Server1\Resources"**
    
    - **Parameter**: `-Path`
    - **Function**: Specifies the path of the folder to search in.
    - **Details**: In this case, it's a network location `\\Server1\Resources\`, indicating a shared folder on a server named 'Server1'.
3. **-Recurse**
    
    - **Type**: Switch parameter for `Get-ChildItem`.
    - **Function**: Instructs the cmdlet to include all child items recursively, searching through all subdirectories of the specified path.
4. **| (Pipeline)**
    
    - **Function**: Passes the output of the command on the left (output of `Get-ChildItem`) as input to the command on the right.
5. **Select-String**
    
    - **Description**: A PowerShell cmdlet used for text searching within files.
    - **Function**: Searches through the input it receives for a specified text pattern.
6. **-Pattern "strPassword"**
    
    - **Parameter**: `-Pattern`
    - **Function**: Specifies the text pattern for `Select-String` to search.
    - **Details**: In this command, it's looking for occurrences of the string "strPassword".



![[Pasted image 20231119065615.png]]

This excerpt demonstrates that there exists a file named \\Server1\HR\dogs\script.vbs, which contains the string "strpassword." The file's content may be accessed through a straightforward "cat" command.

![[Pasted image 20231119065843.png]]

As evident from the aforementioned data, the act of performing this action has resulted in the acquisition of the username and password (J.Gray/abdictate9).

Additionally, users have the capability to navigate to the designated folder and access the script.vbs file.
![[Pasted image 20231119070111.png]]
> To access the script, one can locate it, right-click, and choose the "Edit" option.

As before, lets test the credentials, using : - 

```bash
runas /user:uadtargetnet\J.Gray powershell.exe
```



## 1.3 INFORMATION FOUND IN SYSVOL 
-------
**SYSVOL** is a fundamental directory present on each domain controller within a given domain. Its primary purpose is to store public files that must be accessible to clients while also ensuring synchronization among domain controllers. Typically, the default location for **SYSVOL** is **C:\Windows\SYSVOL**, although it is possible to modify this setting and relocate it to an alternative location.

In an academic context, any policy files and login scripts are typically stored in the sysvol folder. For instance, when a student accesses and logs into Abertay University's network, their respective rights, such as desktop settings, are loaded from SYSVOL.

For 2008r2 Server, any required passwords in files in SYSVOL are encrypted using AES (a symmetric key encryption). The secret key was published by Microsoft! [Windows Protocols](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-gppref/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be)

The key : - 

4e 99 06 e8 fc b6 6c c9 fa f4 93 10 62 0f fe e8
f4 96 e8 06 cc 05 79 90 20 9b 09 a4 33 b6 6c 1b

There are alot of different tools that can decrypt this to find the password e.g gp3finder. 

> 💡 Note that this vulnerability does not apply to servers in the year 2019. However, when the servers are upgraded to a more modern operating system, system administrators often replicate the SYSVOL from the previous system to the new one. This ensures the preservation of group policies and other relevant elements during the upgradeprocess. Nonetheless, it should be noted that the mentioned vulnerability remains valid in such cases.

- If we have the capability to navigate to the SYSVOL folder and locate these files, we could conduct a search within sysvol for the cpassword string by employing PowerShell : - 
```powershell
Get-ChildItem -Path "\\Server1\sysvol\" -Recurse | Select-String -Pattern "cPassword"
```
>Note the following command does not work in this network due to server restrictions on SYSVOL.

Now the cPassword for *T.Douglas*. is visible and can now be decrypted. 

This can be accomplished by running gp3finder through  cmd: - 

cd to where the tools are kept in my case 
```cmd
cd D:\VM's\tools>
```

- now type in the following
```powershell
gp3finder_v5.0.exe -D uiVkd5Daq7Tb2VqmH0BV5w
```

![[Pasted image 20231119073730.png]]

## 1.4 PASSWORD SPRAYING ATTACKS
-------
Most of the time, brute-force attacks employ a technique known as a dictionary attack. However, the success of such attacks is not guaranteed due to network lockout policies. This issue can be mitigated by attempting to use a single password across all user accounts. In the context of network security, this practice is commonly referred to as password spraying. It is worth noting that attackers often suggest including the company name followed by the current year or "2021" as part of the password, thereby increasing their chances of success.

- In order to proceed, kindly duplicate the file '*DomainPasswordSpray.ps1*' from the tools directory onto *Client1*. Additionally, it is feasible to request **ChatGPT** to create a password spray script in **.ps1** format for your use.

From a command prompt, please type the following: - 
```powershell
# To open powershell
# Open up CMD first and then run this from the command line
powershell.exe -exec bypass

# To change directory
cd C:\Users\test\Desktop
Import-Module .\DomainPasswordSpray.ps1

# To start the spray attack
Invoke-DomainPasswordSpray -password uadtargetnet21
```
![[DomainPasswordSpray.ps1]]

![[Pasted image 20231123171020.png]]
> the script 