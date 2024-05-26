
---

### Cheat Sheet for `grep`, `cat`, `sort`, and `uniq`

#### 🐱 `cat` Command
- **Description**: Concatenate files and print on the standard output.
- **Syntax**: `cat [OPTION]... [FILE]...`
- **Examples**:
  - `cat file.txt` ➡️ Display content of `file.txt`.
  - `cat file1.txt file2.txt > combined.txt` ➡️ Combine `file1.txt` and `file2.txt` into `combined.txt`.
  - `cat -n file.txt` ➡️ Number all output lines.

#### 🔍 `grep` Command
- **Description**: Print lines that match patterns.
- **Syntax**: `grep [OPTION]... PATTERNS [FILE]...`
- **Examples**:
  - `grep 'error' demo.txt` ➡️ Search for 'error' in 'demo.txt'.
  - `grep -i 'error' demo.txt` ➡️ Case-insensitive search for 'error'.
  - `grep -r 'config' /etc/` ➡️ Recursively search for 'config' in '/etc/' directory.

#### 🔄 `sort` Command
- **Description**: Sort lines of text files.
- **Syntax**: `sort [OPTION]... [FILE]...`
- **Examples**:
  - `sort names.txt` ➡️ Sort 'names.txt' alphabetically.
  - `sort -r file.txt` ➡️ Sort 'file.txt' in reverse order.
  - `sort -n file.txt` ➡️ Sort 'file.txt' numerically (assumes lines start with numbers).

#### 🦄 `uniq` Command
- **Description**: Report or omit repeated lines.
- **Syntax**: `uniq [OPTION]... [INPUT [OUTPUT]]`
- **Examples**:
  - `uniq file.txt` ➡️ Report or omit repeated lines only.
  - `uniq -c file.txt` ➡️ Prefix lines by their counts.
  - `uniq -d file.txt` ➡️ Only print duplicate lines.

---

### Task Two: Real-World Example Combining Commands

#### Scenario:
You have a log file named `system.log` that records system errors, warnings, and informational messages. You want to extract unique error messages, count their occurrences, and sort them to analyze the frequency of different types of errors.

#### Commands and Explanation:
1. **Extract Errors with `grep`**:
   ```bash
   grep 'ERROR' system.log
   ```
   This filters lines containing 'ERROR'.

2. **Sort Errors with `sort`**:
   ```bash
   grep 'ERROR' system.log | sort
   ```
   This ensures identical error messages are adjacent (a requirement for `uniq`).

3. **Filter Unique Errors with `uniq`**:
   ```bash
   grep 'ERROR' system.log | sort | uniq -c
   ```
   This counts and displays unique error messages.

#### Example Output:
Suppose `system.log` contains multiple entries of different error types. After running the above command chain, you might get output like:
```
4 ERROR: Failed to load configuration
2 ERROR: Network connection lost
3 ERROR: Unable to access database
```
This indicates:
- The configuration load error occurred 4 times.
- The network connection error occurred 2 times.
- The database access error occurred 3 times.

These commands, when used together, provide powerful insights by filtering, sorting, and counting specific patterns in text files, which is immensely helpful in log analysis and data processing.