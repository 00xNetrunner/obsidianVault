## 🌟 Photon - A Crawler Designed for OSINT

GitHub:

Photon is a fast and powerful crawler designed for OSINT (Open Source Intelligence) purposes, capable of extracting URLs, emails, files, website accounts, and much more.

#### 📊 Features

1. **Data Extraction**: Extracts URLs, emails, files, website accounts, and more.
2. **Intelligent Multithreading**: Makes use of multiple threads for faster completion.
3. **Plugins & Scripts**: Extendable with custom scripts for data extraction.

#### 🚀 Usage

1. **Crawling a Website**:
    
    - Command: `python3 photon.py -u http://example.com`
    - Description: This command allows Photon to crawl the entire website and extract the details.
2. **Limiting the Depth**:
    
    - Command: `python3 photon.py -u http://example.com -l 3`
    - Description: This limits the crawling to 3 levels deep.
3. **Exporting Data**:
    
    - Command: `python3 photon.py -u http://example.com --export`
    - Description: Exports the results into a JSON, CSV, or XML file.
4. **Using Plugins**:
    
    - Command: `python3 photon.py -u http://example.com --plugins keyword`
    - Description: Uses a specific plugin (like 'keyword') to extract or analyze information.

#### 🛠️ Advanced Options

- **--dns**: Enumerate subdomains and perform DNS lookups.
- **--keys**: Extract API keys found in JavaScript files.
- **--update**: Update Photon to the latest version.
- **--only-urls**: Output only URLs.
- **--wayback**: Use wayback machine to extract URLs.

## 🌟 theHarvester - E-mail, Subdomain, and Hosts Enumeration Tool

GitHub:

theHarvester is a tool designed for the enumeration of emails, subdomains, hosts, employee names, open ports, and banners from different public sources like search engines and PGP key servers.

#### 📊 Features

1. **Data Collection**: Gathers emails, subdomains, hosts, employee names, and more.
2. **Multiple Sources**: Extracts data from search engines, PGP key servers, and SHODAN computer database.
3. **Exporting Data**: Ability to export data to a text file.

#### 🚀 Usage

1. **Collecting Emails and Hosts**:
    
    - Command: `theHarvester -d example.com -b google`
    - Description: Gathers emails and hosts from 'example.com' using 'google' as the search engine.
2. **Limiting Results**:
    
    - Command: `theHarvester -d example.com -b google -l 500`
    - Description: Limits the search to the first 500 results.
3. **Saving Results**:
    
    - Command: `theHarvester -d example.com -b google -l 500 -f report.html`
    - Description: Saves the results into an HTML file named 'report.html'.

#### 🛠️ Advanced Options

- **-c**: Perform a DNS lookup to verify if the host is alive.
- **-n**: Perform a DNS reverse query on all IP ranges.
- **-v**: Verify host name via DNS resolution and search for virtual hosts.

## 🌟 WPScan - WordPress Security Scanner

GitHub:

WPScan is a black box WordPress security scanner that can be used to scan WordPress websites for security issues.

#### 📊 Features

1. **Enumeration**: Enumerates users, plugins, and themes.
2. **Vulnerability Scanning**: Scans for vulnerabilities in plugins, themes, and WordPress core.
3. **Brute Force**: Attempts to brute force login to WordPress.

#### 🚀 Usage

1. **Scanning a Website**:
    
    - Command: `wpscan --url http://example.com`
    - Description: Scans the specified WordPress website for vulnerabilities.
2. **Enumerating Users**:
    
    - Command: `wpscan --url http://example.com --enumerate u`
    - Description: Enumerates users of the specified WordPress website.
3. **Enumerating Plugins**:
    
    - Command: `wpscan --url http://example.com --enumerate p`
    - Description: Enumerates plugins of the specified WordPress website.
4. **Brute Forcing Passwords**:
    
    - Command: `wpscan --url http://example.com --passwords /path/to/passwords.txt`
    - Description: Performs a password brute force attack.

#### 🛠️ Advanced Options

- **--api-token**: Use the WPScan API token for vulnerability data.
- **--stealthy**: Reduce the number of requests to avoid blocking.
- **--random-user-agent**: Use a random user agent for each scan.