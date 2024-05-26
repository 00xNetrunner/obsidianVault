
### Linux `find` Command Cheat Sheet

#### 1. Basic Syntax
```bash
find [starting-point...] [options...] [expression...]
```

#### 2. Find Files by Name
- **Exact Name**: 
  ```bash
  find / -type f -name "filename"
  ```
- **Case-Insensitive**:
  ```bash
  find / -type f -iname "filename"
  ```

#### 3. Find Directories by Name
- **Exact Name**: 
  ```bash
  find / -type d -name "dirname"
  ```
- **Case-Insensitive**:
  ```bash
  find / -type d -iname "dirname"
  ```

#### 4. Find Files with Wildcards
- **Files Ending with .txt**:
  ```bash
  find / -type f -name "*.txt"
  ```

#### 5. Find by Size
- **Files Larger Than 50MB**:
  ```bash
  find / -type f -size +50M
  ```
- **Files Smaller Than 50MB**:
  ```bash
  find / -type f -size -50M
  ```

#### 6. Find by Modification Time
- **Files Modified Less Than 10 Days Ago**:
  ```bash
  find / -type f -mtime -10
  ```
- **Files Modified More Than 10 Days Ago**:
  ```bash
  find / -type f -mtime +10
  ```

#### 7. Find and Execute a Command on Found Files
- **Delete All .tmp Files**:
  ```bash
  find / -type f -name "*.tmp" -exec rm {} \;
  ```
- **Find and Execute Custom Command**:
  ```bash
  find / -type f -name "filename" -exec <command> {} \;
  ```

#### 8. Limit Depth of Search
- **Limit to 2 Levels Deep**:
  ```bash
  find / -maxdepth 2 -name "filename"
  ```

#### 9. Invert Match
- **Find All Except .txt Files**:
  ```bash
  find / -type f ! -name "*.txt"
  ```

#### 10. Find Files by Permissions
- **Files with Permission 777**:
  ```bash
  find / -type f -perm 0777
  ```

#### 11. Find Files by User/Group
- **Files Owned by User**:
  ```bash
  find / -type f -user username
  ```
- **Files Owned by Group**:
  ```bash
  find / -type f -group groupname
  ```

#### 12. Combining Expressions
- **Files Named 'foo' or 'bar'**:
  ```bash
  find / \( -name "foo" -o -name "bar" \)
  ```

#### 13. Handling 'Permission Denied' Messages
- **Suppress Error Messages**:
  ```bash
  find / -name "filename" 2>/dev/null
  ```

This cheat sheet should cover most of the common use cases for the `find` command. Remember, `find` is a very powerful tool with many more options and capabilities. For more detailed information or for advanced options, refer to the `find` manual by typing `man find` in the terminal.

