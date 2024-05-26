---
sticker: emoji//1f6b0
---
## Overview

PsTools is a collection of tools beneficial for both Network Admins and hackers. It's crucial to have the right privileges to use these tools effectively. Below is a guide to some key utilities and their uses:

## Key Utilities 🔑

1. **PsGetSid** 🆔
    
    - **Purpose:** Display the SID (Security Identifier) of a computer or a user.
    - **Command:** `PsGetSid [options] [computer_name] [user_name]`
2. **PsFile** 📂
    
    - **Purpose:** Shows files opened remotely.
    - **Command:** `PsFile [\\computer [-u username] [-p password] [id]]`
3. **PsInfo** ℹ️
    
    - **Purpose:** Lists information about a system.
    - **Command:** `PsInfo [-h -s] [\\computer [-u username] [-p password]]`
4. **PsLoggedOn** 👤
    
    - **Purpose:** View who's currently logged on.
    - **Command:** `PsLoggedOn [\\computer]`

## Additional Utilities 🔍

- **PsLogList** 📜
    
    - **Purpose:** Dumps event log records.
    - **Command:** `PsLogList [options] [\\computer] [log_name]`
- **PsPasswd** 🔐
    
    - **Purpose:** Changes account passwords.
    - **Command:** `PsPasswd [\\computer [-u username] [-p password]] [username [password]]`
- **PsShutdown** 💻
    
    - **Purpose:** Shuts down and optionally reboots a computer.
    - **Command:** `PsShutdown [-r|-s|-h|-d|-k] [-f] [-c] [-t nn] [\\computer [-u username] [-p password]]`
- **PsSuspend** ⏸️
    
    - **Purpose:** Suspends processes.
    - **Command:** `PsSuspend [\\computer [-u username] [-p password]] <process ID | process name>`
- **PsService** 🛠️
    
    - **Purpose:** View and control services.
    - **Command:** `PsService [options] [\\computer [-u username] [-p password]]`
- **PsKill** ☠️
    
    - **Purpose:** Kill processes by name or process ID.
    - **Command:** `PsKill [-t] [\\computer [-u username] [-p password]] <process ID | process name>`
- **PsList** 📋
    
    - **Purpose:** List detailed information about processes.
    - **Command:** `PsList [\\computer [-u username] [-p password]] [process_name | process_ID]`