---
sticker: lucide//lightbulb
banner: https://images.foxtv.com/static.fox4news.com/www.fox4news.com/content/uploads/2021/05/932/470/6P-190-TZ_FBI-NEW-COMPUTER-FORENSICS-LAB_00.00.00.08.png?ve=1&tl=1
---
# 🔬 Digital Forensics Lab 9: Analysis and Reconstruction 🕵️‍♀️

Welcome to the exciting world of digital forensics! In this lab, we'll dive deep into the analysis and reconstruction phase of a digital forensic investigation. Get ready to unravel the mystery behind the John Doe case using various tools and techniques! 🎭

## 📚 Required Reading

Before we begin, make sure to read Chapter 7 (Timeline Analysis) from the book "Windows Forensic Analysis" by Harlan Carvery. This chapter provides valuable insights into the process of creating and analysing timelines in digital forensics. 📖

## 🛠️ Tools of the Trade

In this lab, we'll explore several tools that will aid us in our analysis and reconstruction efforts:

- 🐧 Linux-based tools:
  - `fls`: A command-line tool for extracting MAC (Modified, Accessed, Created) data from file systems.
  - `pasco`: A tool for extracting data from Internet Explorer's `index.dat` files.
  - `Zeitline`: A Java-based tool for creating and visualizing timelines from various data sources.

- 🪟 Windows-based tools:
  - `Autopsy 4`: A powerful digital forensics platform for analysing disk images and creating timelines.
  - `Eric Zimmerman's Tools`: A collection of tools developed by a former FBI agent for timeline analysis, including `Timeline Explorer` and `TimeApp`.

- 🌐 Web-based tools:
  - `timeline.js`: A JavaScript library for creating interactive timelines.
  - `Webscavator`: A web-based tool for visualizing and analysing timelines.

- 🕰️ Log2Timeline:
  - `Log2Timeline` is a powerful tool for creating super timelines by combining various data sources, such as file system metadata, event logs, prefetch data, and more.

## 🔍 Analysis and Reconstruction Approaches

### Approach 1: The "Old School" Text-Based Approach (Linux) 📜

1. Process the file system MAC data of the `johnDoe.dd` image using the `fls` command:
   ```
   fls -f ntfs -r -o 63 -m / johnDoe.dd > flsoutput.fls
   ```

2. Process the browser data using `pasco`. Extract data from all the `index.dat` files you discovered in the previous labs.

3. Download and unpack the `Zeitline` tool from the CMP209 MLS webpage. Run it using the command:
   ```
   java -jar Zeitline.jar
   ```

4. Import the `fls` data into `Zeitline` using the FLS version 3 filter. If you encounter issues with the file format, adjust the `fls` command-line options until `Zeitline` accepts the generated file.

5. Import the `pasco` data into `Zeitline` and create an integrated view of the John Doe case activity.

6. Consider using or writing additional `Zeitline` filters to integrate more data types from the case.

### Approach 2: The "Integrated" Approach (Windows & Autopsy 4) 🖥️

1. Launch `Autopsy` and import the `johnDoe.dd` image into a new case.

2. Utilize the timeline view in `Autopsy` to investigate how John Doe's computer was used.

### Approach 3: Alternate Approaches 🌈

1. Explore alternative software and approaches for processing and displaying time-based evidence:
   - `timeline.js`: A JavaScript library for creating interactive timelines.
   - `Webscavator`: A web-based tool for visualizing and analyzing timelines.
   - `Eric Zimmerman's Tools` (for Windows users):
     - `Timeline Explorer`: A tool for exploring and analyzing timelines.
     - `TimeApp`: A tool for creating and comparing timelines.

2. Reflect on the significant incidents and episodes you've discovered. What do you think happened in each case? Do you have sufficient evidence to support your conclusions? 🤔

## 🕰️ Using Log2Timeline

`Log2Timeline` is a powerful tool for creating super timelines by combining various data sources. Here's a step-by-step guide on how to use it:

1. Install `Log2Timeline` on your system. You can follow the installation instructions from the official documentation: https://plaso.readthedocs.io/en/latest/sources/user/Installation.html

2. Prepare your evidence sources. `Log2Timeline` supports various data sources, such as disk images, directories, and individual files.

3. Create a new timeline using the `log2timeline` command:
   ```
   log2timeline.py --source evidence.dd --output timeline.plaso
   ```
   Replace `evidence.dd` with the path to your evidence source and `timeline.plaso` with the desired output file name.

4. `Log2Timeline` will process the evidence and create a timeline file in the `plaso` format.

5. To view and analyze the timeline, you can use the `pinfo.py` command:
   ```
   pinfo.py timeline.plaso
   ```
   This will provide an interactive shell where you can query and filter the timeline data.

6. You can also use the `psort.py` command to output the timeline in various formats, such as CSV or JSON:
   ```
   psort.py -o csv timeline.plaso > timeline.csv
   ```
   This command will export the timeline to a CSV file named `timeline.csv`.

7. Explore the timeline data using the available commands and filters in `pinfo.py` or analyze the exported files using other tools like `Timeline Explorer` or spreadsheet software.

Remember, `Log2Timeline` is a powerful tool with many advanced features. Refer to the official documentation and online resources for more detailed information on using it effectively.

## 🎨 Embellishments

To make your lab experience more enjoyable and visually appealing, here are a few embellishments:

- Use emoji to highlight important points or sections. For example, use 🔍 for analysis, 🛠️ for tools, and 📚 for reading materials.
- Add visual elements like images or diagrams to illustrate complex concepts or processes.
- Use formatting like bold, italics, and headings to structure your document and make it easier to read.
- Include code snippets or command-line examples to provide clear instructions on how to use the tools.
- Use blockquotes or callouts to emphasize important information or tips.

> 💡 Tip: Don't forget to document your findings and conclusions throughout the analysis process!

Certainly! Here's an updated version of the lab instructions focusing on using Autopsy for timeline analysis and reconstruction:

# 🔬 Digital Forensics Lab 9: Analysis and Reconstruction with Autopsy 🕵️‍♀️

In this lab, we'll explore how to use Autopsy, a powerful digital forensics platform, to analyze and reconstruct events in the John Doe case. Autopsy provides a comprehensive set of tools for creating and examining timelines, making it an essential tool in our digital forensics arsenal. 🛠️

## 📚 Prerequisites

Before starting this lab, ensure that you have:
- Access to the John Doe case files, including the `johnDoe.dd` disk image.
- Autopsy installed on your Windows or Linux machine. You can download Autopsy from https://www.autopsy.com/download/.

## 🚀 Getting Started

1. Launch Autopsy and create a new case for the John Doe investigation.

2. Add the `johnDoe.dd` disk image as a data source to your case.

3. Configure the ingest modules in Autopsy to extract relevant artifacts for timeline analysis. Some useful modules include:
   - Recent Activity: Extracts recently accessed files, documents, and websites.
   - Web History: Parses web browser history files.
   - Registry Analysis: Extracts timeline-related information from the Windows Registry.
   - Event Log Parser: Parses Windows Event Logs for relevant events.

4. Run the ingest modules and wait for the processing to complete. ⏳

## 🔍 Timeline Analysis in Autopsy

1. Once the ingest modules have finished running, navigate to the "Timeline" view in Autopsy.

2. Explore the generated timeline, which presents a chronological view of the events and activities on John Doe's computer. 📅

3. Use the filtering and search options in the timeline view to focus on specific time ranges, event types, or keywords of interest. 🕵️‍♂️

4. Examine the details of individual events by clicking on them in the timeline. This will provide more information about the event, such as the associated file, artifact, or activity.

5. Look for significant patterns, anomalies, or suspicious activities in the timeline that may be relevant to the John Doe case. 🔍

## 🖥️ Artifact Analysis

1. In addition to the timeline view, explore other artifact-specific views in Autopsy, such as:
   - File System: Browse the directory structure and examine individual files.
   - Web Artifacts: Analyze web browser history, bookmarks, and cached files.
   - Registry: Investigate relevant Registry keys and values.
   - Email Messages: Examine email artifacts, including headers, bodies, and attachments.

2. Correlate the findings from these artifact views with the events in the timeline to gain a more comprehensive understanding of the activities on John Doe's computer. 🧩

## 📝 Reporting and Collaboration

1. Use Autopsy's built-in reporting features to generate a comprehensive report of your findings. The report can include timeline events, notable artifacts, and your analysis conclusions. 📋

2. Collaborate with your team members by sharing the Autopsy case file or exporting relevant artifacts and timelines for further analysis. 👥

## 🎨 Embellishments

To make your lab experience more engaging and visually appealing, consider the following embellishments:

- Use emoji to highlight important points or sections. For example, use 🔍 for analysis, 🛠️ for tools, and 📚 for reading materials.
- Add screenshots or images to illustrate key findings or steps in the analysis process.
- Use formatting like bold, italics, and headings to structure your report and make it easier to read.
- Include code snippets or command-line examples to provide clear instructions on how to use Autopsy features.
- Use blockquotes or callouts to emphasize important information or tips.

> 💡 Tip: Regularly save your Autopsy case and create backups to prevent data loss!

## 🎉 Conclusion

By leveraging the powerful features of Autopsy, you can efficiently analyze and reconstruct events in the John Doe case. The timeline view, combined with artifact-specific analysis, allows you to uncover critical details and piece together the story behind the digital evidence. Remember to document your findings, collaborate with your team, and always strive for thoroughness and accuracy in your analysis. 🕵️‍♀️

Happy investigating with Autopsy! 🔍✨