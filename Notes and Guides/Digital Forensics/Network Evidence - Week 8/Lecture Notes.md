# CMP209 - Network Evidence (stage 3.4) 📡

## Outcomes of this lecture and associated lab (week 8)
- 🐧 Increased understanding and competence using Linux for digital forensics
- 🔍 Familiarization with digital forensic investigation processes 
  - 🌐 Focus on forensically examining Windows user browser activity

## Practical skills to master by week 8
- 💻 Working with the Linux CLI
- 📀 Imaging with dcfldd, verifying with md5sum, auditing disks with mmls, file carving with foremost
- 🗄️ Performing registry analysis using multiple tools
- 👥 Determining suspect system users

⚠️ Evidence of these skills needed in court report (Sections 2.3 and 2.4)

## Searching for network evidence 🔍
- Directed logical searching of additional evidence sources:
  - 🌐 Browsers
  - 📧 Emails and email programs  
  - 💬 Web/Chat (potentially IRC-based)
  - 🔄 P2P
- Classified as network evidence due to reliance on network communications

### Browser forensics 🌐
- Popular browsers in 2023: Chrome, Safari, Edge, Firefox, Samsung Internet
  - Samsung Internet popularity implies rise of mobile browsing
- Browsers typically record:
  - Cache/temp files
  - Browsing history
  - Cookies
  - Bookmarks
  - Recently used items
  - Autocompletion data
  - Typed URLs

### Browser cache forensics 🖥️
- Browser cache: temporary storage of recently downloaded web pages
  - Speeds up revisiting pages without re-downloading
- Cache includes full page data and linked media
  - Cached media becomes separate GET requests
- Cache analysis provides evidence of suspect browsing activity

### Analyzing Internet Explorer 🔍
- Identifying suspect's browser: process of elimination, trial and error, Autopsy
- Criminals often use outdated/obscure browsers to complicate investigations
- IE components to analyze:
  - Cache, history, cookies
  - Multiple index.dat files containing browsing activity data

#### IE cache
- IE cache: local copy of viewed web pages and media (temp internet files)
- Wayback Machine: view historical versions of web pages
- index.dat files contain timestamps, URLs, accessed files
  - Located in various paths depending on Windows version  

#### IE history
- History data has limited persistence
- History file (index.dat) location varies by Windows version

#### IE cookies
- Cookies store user preferences, login data, tracking info across sessions
  - Contain passwords, IP addresses, timestamps, user clicks
- Indexed in index.dat, contents in .txt (e.g. cookies.txt)
- Cookie file location varies by Windows version
  - Typically same directory as index.dat

#### Other IE evidence sources
- Many hidden/system classified index.dat files 
  - Easier to find on mounted image vs running OS
- Also check bookmarks, recently used items, typed URLs
- Modern browsers use SQLite - find and search these databases
  - Use SQLECmd tool to browse SQLite files

## Email forensics 📧
- Email clients vary: Outlook, Thunderbird, webmail (Gmail, Yahoo, etc.)
  - Locate installed/deleted clients via registry analysis
- Viewing emails:
  - Natively: install client software on analysis machine, open retrieved files
  - Viewer tools like Autopsy
- Check email configuration (IMAP/POP3) 
  - May find copies on ISP servers with their cooperation
- Inspect headers
- Decode MIME/Base64 encoded emails if needed (e.g. with Munpack)
- Email evidence has limitations - messages can be deleted from servers
- Treat webmail as browser-based evidence unless downloaded

## Chat data forensics 💬
- Types: Messenger, in-game, mobile (WhatsApp, Instagram, FB), IRC
- Chat evidence significance: 
  - Info/file exchanges
  - Audio/video
  - Text
  - Added context to investigation
- Investigators could communicate with suspects if unaware of compromise
  - But can be difficult - encryption, data not stored locally, etc.

## Systematic software analysis 🗂️
- Every case is different - adapt to suspect's software usage
  - e.g. Analyzing P2P file sharing activity  
- Research specific software analysis methods as needed
- Follow systematic approach, document findings and methods
  - Build the suspect's "story"

## Tracking time ⏰
- To establish event sequence, account for clock skew
  - e.g. In spreadsheet analysis, add skew to each row to enable sorting
- Accurate timeline critical for reconstruction

## Additional resources
- Advanced evidence collection methodology - Oh et al. (2011)
- Internet activity and cookie file reconstruction methods
- Eric Zimmerman's forensic tools for Windows
  - https://ericzimmerman.github.io/#!index.md
  - SANS cheat sheet: https://www.sans.org/posters/eric-zimmerman-tools-cheat-sheet/ 
- SANS Browser Forensics video: https://www.youtube.com/watch?v=LnhSTZgzKuY