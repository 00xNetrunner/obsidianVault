---
banner: https://s3.amazonaws.com/marstranslation.aws.bucket/default/0001/08/15c3c9ed02e9c5f6a53f56b97665088351d790d3.jpeg
sticker: emoji//1f4d3
---
# 📝 CMP209 - Analysis and Reconstruction (Lecture 9) Notes

## 🎯 Outcomes
- Increased understanding and competence using Linux for digital forensics
- Familiarization with digital forensic investigation processes
  - Systematic analysis of gathered evidence
  - Reconstruction of events from evidence (timeline development)
- Understanding how to use various tools to access and filter forensic data to produce a case narrative

## 📁 Current Progress
- Uncovered various aspects of the John Doe case:
  - Emails, browser activity, registry data, log files, and other OS evidence
- Need to understand how these pieces fit into a larger, structured narrative

## 🔍 Styles of Analysis
- Relational: Entities involved, relationships, communications
- Functional: What entities did
- Temporal: When events occurred, order of events
- Spatial: Where events took place

## 🔧 Timeline Development
- Two types of timelines:
  - Filesystem timelines: Easier, based on filesystem metadata, subset of data, faster (e.g., FLS, MFTECmd)
  - Super timelines: Include additional info (event logs, prefetch data, shellbags, link files), more intricate (e.g., Plaso + Log2Timeline)

## 🛠️ Tools to Consider
- Zeitline (use version on MLS, FLS filter v3)
- fls
- Autopsy
- Log2timeline (Plaso with pinfo.py, psort.py, psteal.py)
  - VSS compatible, use parsers and filter_windows.txt
- Timeline.js
- Webscavator
- Kape
- Timeline Explorer
- TimeApp
- Eric Zimmerman's Windows tools (run via PowerShell)

## 📺 Demonstrations
- Focus on Log2Timeline and Plaso
- Additional use of Timeline Explorer to view events

## 📚 Next Steps
- Read supplementary material on MLS
  - Timeline analysis (Chapter 7) by Harlan Carvey
  - SANS Institute timeline development cheat sheet
- For Windows users, consider Eric Zimmerman's tools
- Attend and complete the practical lab for week 9