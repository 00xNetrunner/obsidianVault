---
sticker: emoji//1f53b
tags:
  - MINDMAP
---
# NMAP Commands

## Scan Types
- ### SYN Scan
  - `-sS`
- ### TCP Connect Scan
  - `-sT`
- ### UDP Scan
  - `-sU`
- ### SCTP INIT Scan
  - `-sY`
- ### TCP Null Scan
  - `-sN`

## Host Discovery
- ### Ping Scan
  - `-sn`
- ### No Ping
  - `-Pn`
- ### ICMP Echo Request
  - `-PE`
`ris:Remixicon`
`rir:Remixicon`
`ris:RemoteControl`
## Port Specification
- ### Specify Ports
  - `-p`
- ### Scan Top Ports
  - `--top-ports`
- ### Exclude Certain Ports
  - `--exclude-ports`

## Service and Version Detection
- ### Version Detection
  - `-sV`
- ### Set Version Intensity
  - `--version-intensity`

## OS Detection
- ### Enable OS Detection
  - `-O`
- ### Limit OS Detection
  - `--osscan-limit`
- ### Guess OS Detection
  - `--osscan-guess`

`ris:Remixicon`
`fas:Asterisk`
`fas:Stream`
## Nmap Scripting Engine (NSE)
- ### Specify Scripts
  - `--script`
- ### Provide Script Arguments
  - `--script-args`

## Output Options
- ### Normal Output
  - `-oN`
- ### XML Output
  - `-oX`
- ### Grepable Output
  - `-oG`

## Timing and Performance
- ### Paranoid Timing
  - `-T0`
- ### Sneaky Timing
  - `-T1`
- ### Polite Timing
  - `-T2`
- ### Normal Timing
  - `-T3`
- ### Aggressive Timing
  - `-T4`
- ### Insane Timing
  - `-T5`
